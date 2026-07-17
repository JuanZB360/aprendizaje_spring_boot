## 🔙 [***Volver***](./entidades.md)

---

# 👻 `orphanRemoval`: El Limpiador Automático de Hibernate

Hasta ahora nuestra relación `@OneToOne` quedó configurada así:

```java
@OneToOne(
    cascade = CascadeType.ALL,
    fetch = FetchType.LAZY,
    orphanRemoval = true
)
@JoinColumn(name = "gerente_id")
private Gerente gerente;
```

Ya entendemos qué hacen `cascade` y `fetch`.

Ahora nos centraremos en el último atributo:

```java
orphanRemoval = true
```

A primera vista parece una opción sencilla, pero en realidad resuelve un problema muy específico del ciclo de vida de las entidades.

---

# 🤔 ¿Qué significa "Orphan"?

La palabra **Orphan** significa:

> **Huérfano**

En JPA, una entidad huérfana es una entidad que **ha perdido la referencia con su entidad padre**, pero **todavía existe en la base de datos**.

Es decir, sigue ocupando espacio aunque ya nadie la utilice.

---

# 🎮 La Analogía del Local de Videojuegos

Imaginemos el siguiente escenario.

Tenemos este Local.

```text
🎮 GameZone
        │
        ▼
👨‍💼 Juan
```

Todo funciona correctamente.

Ahora el dueño decide cambiar de gerente.

El nuevo gerente será:

```text
👨‍💼 María
```

Entonces hacemos esto:

```java
Gerente nuevoGerente = new Gerente();
nuevoGerente.setNombre("María");

local.setGerente(nuevoGerente);
```

En memoria ya ocurrió el cambio.

Ahora el Local apunta a María.

```text
🎮 GameZone
        │
        ▼
👨‍💼 María
```

Pero...

¿Qué ocurrió con Juan?

---

# 🤔 El problema

Juan ya no administra ningún Local.

Sin embargo...

Todavía sigue existiendo en la base de datos.

```text
Tabla gerentes

id | nombre

1  | Juan
2  | María
```

Juan quedó completamente solo.

Ningún Local apunta hacia él.

Se convirtió en una entidad **huérfana**.

---

# 🗑️ Aquí aparece `orphanRemoval`

Cuando escribimos:

```java
orphanRemoval = true
```

Hibernate interpreta la situación de esta manera:

> "Si una entidad deja de pertenecer a su padre y nadie más la utiliza..."

> "...elimínala automáticamente."

Entonces, cuando guardemos el Local:

```java
localRepository.save(local);
```

Hibernate hará algo parecido a esto.

```text
UPDATE locales

↓

DELETE FROM gerentes
WHERE id = 1;
```

El antiguo gerente desaparece automáticamente.

No necesitamos escribir:

```java
gerenteRepository.delete(gerenteViejo);
```

Hibernate lo hace por nosotros.

---

# 🔍 ¿Cuándo actúa `orphanRemoval`?

No elimina entidades porque sí.

Solo actúa cuando una entidad relacionada deja de pertenecer a su entidad padre.

Por ejemplo:

```java
local.setGerente(null);
```

O:

```java
local.setGerente(nuevoGerente);
```

En ambos casos, el gerente anterior pierde su relación con el Local.

Hibernate detecta que quedó huérfano y lo elimina.

---

# 🎯 Ejemplo completo

Estado inicial.

```text
🎮 Local

↓

👨‍💼 Juan
```

Ahora cambiamos el gerente.

```java
local.setGerente(maria);
```

Visualmente ocurre esto.

Antes:

```text
🎮 Local
      │
      ▼
👨‍💼 Juan
```

Después:

```text
🎮 Local
      │
      ▼
👨‍💼 María

👨‍💼 Juan

❌ Sin referencias
```

Como Juan ya no pertenece a ningún Local, Hibernate lo elimina.

---

# ⚠️ ¿Qué pasa si `orphanRemoval` es `false`?

Supongamos exactamente el mismo escenario.

```java
local.setGerente(maria);
```

Hibernate actualizará el Local.

Pero el gerente anterior permanecerá en la base de datos.

```text
Tabla gerentes

Juan

María
```

Aunque Juan ya no tenga ninguna utilidad.

Con el tiempo la base de datos comenzaría a llenarse de registros abandonados.

---

# 🚨 No confundas `orphanRemoval` con `CascadeType.REMOVE`

Esta es una de las confusiones más comunes cuando se aprende JPA.

Aunque ambos pueden provocar eliminaciones automáticas, **no trabajan en el mismo momento**.

---

# 🗑️ `CascadeType.REMOVE`

Actúa cuando eliminas la entidad padre.

Por ejemplo:

```java
localRepository.delete(local);
```

Hibernate piensa:

> "Si el Local desaparece, también debe desaparecer su Gerente."

Visualmente:

```text
Eliminar Local

↓

Eliminar Gerente
```

---

# 👻 `orphanRemoval`

Actúa cuando el hijo deja de pertenecer al padre.

No es necesario eliminar el Local.

Basta con romper la relación.

```java
local.setGerente(null);
```

O:

```java
local.setGerente(nuevoGerente);
```

Hibernate detecta que el gerente anterior quedó sin dueño y lo elimina.

---

# 📊 Comparación rápida

| Situación                 | `CascadeType.REMOVE`     | `orphanRemoval`       |
| ------------------------- | ------------------------ | --------------------- |
| Eliminas el Local         | ✅ Elimina el Gerente     | ❌ No interviene       |
| Cambias el Gerente        | ❌ No elimina el anterior | ✅ Elimina el anterior |
| Asignas `null` al Gerente | ❌ No elimina             | ✅ Elimina             |

---

# 🧠 ¿Cuándo tiene sentido usar `orphanRemoval`?

Debes hacerte esta pregunta.

> **¿La entidad hija puede existir sin la entidad padre?**

Si la respuesta es:

**No**

Entonces `orphanRemoval` suele ser una buena opción.

---

# 🎮 En nuestro ejemplo

Un gerente únicamente existe porque administra un Local.

Si deja de administrarlo y no será reutilizado en otro lugar, tiene sentido eliminarlo.

Por eso esta configuración resulta lógica.

```java
@OneToOne(
    orphanRemoval = true
)
```

---

# ⚠️ Pero no siempre debe utilizarse

Imagina una entidad diferente.

```text
👤 Usuario

↓

📍 Dirección
```

Si el usuario cambia de dirección, quizá quieras conservar la dirección anterior por motivos de auditoría o historial.

En ese caso **no sería buena idea utilizar `orphanRemoval`**.

Todo depende de las reglas del negocio.

---

# 🎒 Resumen

| Configuración           | ¿Qué hace?                                                                        |
| ----------------------- | --------------------------------------------------------------------------------- |
| `orphanRemoval = true`  | Elimina automáticamente las entidades hijas que quedan sin relación con su padre. |
| `orphanRemoval = false` | Conserva esas entidades aunque ya no estén relacionadas.                          |

---

# 🏆 Conclusión

`orphanRemoval` no crea relaciones, no modifica el tipo de carga y tampoco controla cómo se guardan las entidades.

Su única responsabilidad es mantener limpia la base de datos eliminando automáticamente las entidades que han quedado sin un padre.

Podemos resumirlo así:

```text
Si una entidad hija pierde a su padre...

↓

Hibernate pregunta:

"¿Tiene sentido que siga existiendo?"

↓

Si la respuesta es NO
y orphanRemoval = true

↓

La elimina automáticamente.
```

---

## 🔙 [***Volver***](./entidades.md)