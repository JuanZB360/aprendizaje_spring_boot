## 🔙 [***Volver***](./persistencia.md)

---

# 🚀 El Superpoder de `fetch`
## ¿Traer rápido o traer después?

Este superpoder controla:

# 🧠 La paciencia de la computadora

para traer información desde:

🗄️ la base de datos

hasta:

💻 tu aplicación.

---

# 🎮 La historia del Local y Juan

Imagina que tienes:

- un 🎮 Local de Videojuegos,
- y un 👨‍💼 Gerente llamado Juan.

Ambos están conectados.

Ahora imagina que le preguntas a Spring:

> “Muéstrame el Local”.

La gran pregunta es:

## 🤔 ¿También debería traer automáticamente a Juan?

Ahí entra:

```text
fetch
```

---

# 🏃 `FetchType.EAGER`
## El ansioso

Este poder hace que Spring sea:

# ⚡ Súper impaciente

---

# 💡 ¿Qué pasa?

Cuando pides:

🎮 ****el Local****

Spring inmediatamente trae:

- el Local,
- y también a 👨‍💼 Juan.

Aunque tú no hayas pedido al gerente.

---

# 📦 Resultado

Spring trae:

🎮 ***Local*** + 👨‍💼 ***Gerente***

todo junto.

---

# 🧠 Analogía fácil

Es como pedir:

🍔 una hamburguesa

y que el mesero también te traiga:

🍟 papas,
🥤 gaseosa,
🍰 postre

aunque tú solo querías la hamburguesa.

---

# ⚠️ Problema real

Si tienes:

- 1000 locales,

Spring también traerá:

- 1000 gerentes.

Eso puede:

- consumir mucha memoria.
- volver lento el servidor.
- cansar la aplicación.

---

# 📌 Importante

En `@OneToOne`:

## ⚠️ `EAGER` viene activado por defecto

Por eso muchos desarrolladores lo cambian manualmente.

---

# 🦥 `FetchType.LAZY`
## El inteligente

Este poder hace que Spring sea:

# 🧠 Más calmado y eficiente

---

# 💡 ¿Qué pasa?

Cuando pides:

🎮 ***el Local***

Spring trae solamente:

🎮 ***el Local***.

Juan se queda tranquilo en su oficina.

---

# 👨‍💼 ¿Cuándo aparece Juan?

Solo cuando realmente lo necesitas.

Por ejemplo:

> “Ahora sí quiero ver al gerente”.

En ese momento Spring va a buscarlo.

---

# 📦 Resultado

Primero llega:

🎮 Local

Después, si lo necesitas:

👨‍💼 Juan

---

# ✅ Ventaja principal

Consume:

- menos memoria.
- menos recursos.
- mejora el rendimiento.

---

# 🧠 Idea fácil

| Tipo | Comportamiento |
|---|---|
| `EAGER` | Trae todo inmediatamente |
| `LAZY` | Trae solo lo necesario |

---

# 📌 ¿Dónde se coloca `fetch`?

Como la relación es:

# 🔗 Bidireccional

puedes navegar desde ambos lados.

Entonces:

## ✅ normalmente se configura en AMBAS entidades

---

# 🎮 En `Local`

Usamos:

`FetchType.LAZY`

para que al buscar el Local:

❌ NO traiga automáticamente al Gerente.

---

# 👨‍💼 En `Gerente`

También usamos:

`FetchType.LAZY`

para que al buscar un Gerente:

❌ NO traiga automáticamente todo el Local.

---

# 💻 Ejemplo conceptual

## 🎮 Local

```java
@OneToOne(fetch = FetchType.LAZY)
private Gerente gerente;
```

---

## 👨‍💼 Gerente

```java
@OneToOne(mappedBy = "gerente", fetch = FetchType.LAZY)
private Local local;
```

---

# ⚠️ Nota de Senior

La buena práctica en proyectos reales es:

# ✅ usar `LAZY` casi siempre

Porque protege la memoria del servidor.

---

# 🎯 Resumen fácil

| Poder | ¿Qué hace? |
|---|---|
| `EAGER` | Trae todo de inmediato |
| `LAZY` | Trae datos solo cuando se necesitan |

---

# 💡 Idea final

`fetch` decide si Spring debe ser:

# 🏃 Ansioso
o
# 🦥 Paciente

al traer información desde la base de datos.

---

## 🔙 [***Volver***](./persistencia.md)