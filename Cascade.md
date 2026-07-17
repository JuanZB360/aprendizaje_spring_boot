## 🔙 [***Volver***](./entidades.md)

---

---

# 🌊 `cascade`: El Efecto Dominó de Hibernate

Hasta este momento ya sabemos construir una relación bidireccional.

Nuestra entidad `Local` quedó así:

```java
@OneToOne(
    cascade = CascadeType.ALL
)
@JoinColumn(name = "gerente_id")
private Gerente gerente;
```

Probablemente ahora te estés preguntando:

> 🤔 ¿Qué hace realmente `cascade`?

La respuesta corta es:

> **Le dice a Hibernate qué operaciones debe propagar automáticamente desde una entidad hacia otra.**

Pero entendamos esto con calma.

---

# 🎮 La Analogía del Local de Videojuegos

Imagina que acabas de inaugurar un nuevo local.

Todavía no existe nada en la base de datos.

Creas los siguientes objetos.

```text
🎮 Local

GameZone
```

Y también:

```text
👨‍💼 Gerente

Juan
```

Ahora los conectas.

```java
Local local = new Local();
Gerente gerente = new Gerente();

local.setGerente(gerente);
```

Hasta aquí únicamente existen objetos en memoria.

Nada ha sido guardado en la base de datos.

---

# 💾 Guardando el Local

Ahora haces lo siguiente.

```java
localRepository.save(local);
```

Aquí Hibernate debe tomar una decisión importante.

Pregunta internamente:

> "¿Qué hago con el objeto Gerente?"

Tiene dos opciones.

---

# ❌ Sin `cascade`

Si la relación NO tiene `cascade`, Hibernate únicamente guardará el objeto que le pediste.

```text
💾 Guarda:

✅ Local

❌ Gerente NO
```

Pero aparece un problema.

El Local necesita guardar una llave foránea.

```text
gerente_id
```

Sin embargo...

Ese gerente todavía no existe en la base de datos.

Entonces Hibernate lanzará un error parecido a este.

```text
TransientObjectException

object references an unsaved transient instance
```

En otras palabras:

> "Intentas guardar un Local que apunta a un Gerente que todavía no existe."

---

# 😓 ¿Cómo solucionaríamos esto manualmente?

Tendríamos que guardar primero el gerente.

```java
gerenteRepository.save(gerente);
```

Después guardar el Local.

```java
localRepository.save(local);
```

Dos operaciones.

Dos consultas.

Más código.

---

# ✅ Con `CascadeType.PERSIST`

Ahora agregamos:

```java
cascade = CascadeType.PERSIST
```

Cuando hacemos:

```java
localRepository.save(local);
```

Hibernate piensa:

> "También debo guardar el gerente."

Entonces ejecuta automáticamente:

```text
INSERT INTO gerentes...

INSERT INTO locales...
```

Todo ocurre de forma transparente.

Nosotros solamente escribimos:

```java
localRepository.save(local);
```

Hibernate hace el resto.

---

# 🌊 ¿Por qué se llama Cascade?

La palabra **Cascade** significa:

> **Cascada**

o

> **Efecto dominó**.

Visualmente ocurre algo parecido a esto.

```text
Guardar Local

      │

      ▼

Guardar Gerente
```

Una operación provoca otra.

Y esa puede provocar otra más.

---

# 🎯 ¿Qué operaciones pueden propagarse?

Hibernate permite propagar diferentes operaciones.

Cada una tiene un propósito específico.

---

# 💾 `CascadeType.PERSIST`

Propaga la operación de guardar.

```java
cascade = CascadeType.PERSIST
```

Cuando guardas:

```java
localRepository.save(local);
```

Hibernate también guarda automáticamente:

```text
Gerente
```

---

# ✏️ `CascadeType.MERGE`

Propaga las actualizaciones.

Supongamos que cambiamos el nombre del gerente.

```java
gerente.setNombre("Carlos");
```

Luego actualizamos el Local.

```java
localRepository.save(local);
```

Gracias al `MERGE`, Hibernate también actualizará el gerente.

---

# 🗑️ `CascadeType.REMOVE`

Propaga la eliminación.

Si hacemos:

```java
localRepository.delete(local);
```

Hibernate eliminará:

- el Local
- el Gerente

Automáticamente.

---

# 🔄 `CascadeType.REFRESH`

Actualiza los datos desde la base de datos.

Imagina que otra aplicación modificó el gerente.

Con `REFRESH`, Hibernate vuelve a consultar la base de datos y reemplaza los datos del objeto actual.

---

# 📋 `CascadeType.DETACH`

Desconecta ambas entidades del contexto de persistencia.

Es una operación mucho menos utilizada, pero también puede propagarse.

---

# ⭐ `CascadeType.ALL`

Es simplemente un atajo.

Equivale a escribir:

```java
cascade = {
    CascadeType.PERSIST,
    CascadeType.MERGE,
    CascadeType.REMOVE,
    CascadeType.REFRESH,
    CascadeType.DETACH
}
```

Es decir:

> "Propaga todas las operaciones."

---

# 🧠 ¿Entonces siempre debo usar `CascadeType.ALL`?

No.

Y aquí aparece una de las decisiones más importantes al diseñar entidades.

Debes preguntarte:

> ¿Tiene sentido que ambas entidades vivan y mueran juntas?

Si la respuesta es **sí**, `CascadeType.ALL` suele ser una buena opción.

Si la respuesta es **no**, probablemente debas utilizar únicamente algunos tipos de cascada.

---

# 🎮 En nuestro ejemplo

Supongamos que un Local cierra definitivamente.

¿Tiene sentido conservar un gerente que solamente trabajaba en ese local?

Probablemente no.

Entonces esta configuración tiene lógica.

```java
@OneToOne(
    cascade = CascadeType.ALL
)
```

Porque ambas entidades forman parte del mismo ciclo de vida.

---

# ⚠️ Pero no siempre ocurre así

Imagina ahora otra relación.

```text
Empleado
        │
        ▼
Departamento
```

Muchos empleados pertenecen al mismo departamento.

Si eliminas un empleado...

¿También debería desaparecer el departamento?

Obviamente no.

Sería un desastre.

Por eso nunca debemos agregar `CascadeType.ALL` sin analizar primero la lógica del negocio.

---

# 🎯 Una regla sencilla para recordar

Antes de agregar una cascada pregúntate siempre:

> **¿La entidad hija puede existir sin la entidad padre?**

Si la respuesta es:

**NO**

La cascada suele tener sentido.

Si la respuesta es:

**SÍ**

Debes pensar muy bien qué operaciones quieres propagar.

---

# 🎒 Resumen

| Tipo de Cascade | ¿Qué hace? |
|-----------------|------------|
| `PERSIST` | Guarda automáticamente las entidades relacionadas. |
| `MERGE` | Actualiza automáticamente las entidades relacionadas. |
| `REMOVE` | Elimina automáticamente las entidades relacionadas. |
| `REFRESH` | Recarga la información desde la base de datos. |
| `DETACH` | Desconecta las entidades del contexto de persistencia. |
| `ALL` | Incluye todas las operaciones anteriores. |

---

# 🏆 Conclusión

`CascadeType` no crea relaciones entre entidades.

La relación ya existe gracias a `@OneToOne`, `@OneToMany`, `@ManyToOne` o `@ManyToMany`.

Lo que hace `cascade` es controlar cómo se comportan esas entidades durante su ciclo de vida.

Podemos imaginarlo como un efecto dominó:

```text
Guardar Padre
      │
      ▼
Guardar Hijo

Actualizar Padre
      │
      ▼
Actualizar Hijo

Eliminar Padre
      │
      ▼
Eliminar Hijo
```

---

## 🔙 [***Volver***](./entidades.md)