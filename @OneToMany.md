## 🔙 [***Volver***](./entidades.md)

---

# 🔗 `@ManyToOne` y `@OneToMany`
## 🎮 Relación entre Local y Juegos

En esta relación:

- muchos Juegos pueden pertenecer a un solo Local.
- y un Local puede tener muchos Juegos.

---

# 🛣️ En el Camino de Una Sola Vía
## 📍 La Relación Unidireccional

En este mundo, los Juegos son los únicos que saben en qué Local están guardados.

El Local no tiene ninguna lista de juegos.

---

# 👶 Explicación sencilla

Imagina una tienda de videojuegos.

Cada juego tiene un sticker pegado atrás que dice:

`"Este juego pertenece al Local del Centro"`.

Entonces:

- el Juego sí sabe cuál es su Local,
- pero el Local NO sabe cuáles juegos tiene guardados.

Si le preguntas al Local:

> "Muéstrame todos tus juegos"

el Local respondería:

> "No tengo ninguna lista".

---

# 🧠 Idea importante

En `@ManyToOne`:

- la entidad `Juego`
es quien guarda la llave real de la relación.

Por eso:

- el candado físico (`Foreign Key`)
vive dentro de la tabla de Juegos.

---

# 🧩 Código del Juego
## 🎮 El que conoce al Local

```java
@ManyToOne(fetch = FetchType.LAZY)
@JoinColumn(name = "local_id")
private Local local;
```

---

# 🔍 Explicación

## `@ManyToOne`

Muchos juegos pueden pertenecer a un único Local.

---

## `fetch = FetchType.LAZY`

El Local solo se buscará en la base de datos cuando realmente lo necesites.

---

## `@JoinColumn(name = "local_id")`

Crea la columna:

`local_id`

dentro de la tabla de juegos.

Esa columna guarda el ID del Local al que pertenece cada juego.

---

# 🏢 Código del Local
## 😴 El Local distraído

`@Entity
public class Local {

    private Long id;
    private String nombreLocal;

}`

---

# 📌 Importante

Aquí NO existe:

`List<Juego> juegos`

porque el Local todavía no conoce a sus juegos.

---

# 🔄 El Camino de Doble Vía
## 🔁 Relación Bidireccional

Ahora queremos que:

- el Juego conozca al Local,
- y el Local conozca todos sus Juegos.

---

# 👶 Explicación sencilla

Ahora el Local recibe una:

## 📂 Súper Carpeta

Cada vez que llega un juego nuevo,
el Local lo apunta dentro de su lista.

Entonces:

- el Juego sigue teniendo su sticker,
- pero ahora el Local también tiene una lista completa de juegos.

---

# 🏢 Código del Local
## 📂 El dueño de la lista

`@OneToMany(
    mappedBy = "local",
    cascade = CascadeType.ALL,
    fetch = FetchType.LAZY
)
private List<Juego> juegos;`

---

# 🔍 Explicación

## `@OneToMany`

Un Local puede tener muchos Juegos.

---

## `mappedBy = "local"`

Le dice a Spring:

> "La relación verdadera ya existe dentro de la variable `local` de la clase Juego".

⚠️ Gracias a esto:

- NO se crea otra columna,
- NO se crea otra tabla,
- y se evita duplicar relaciones.

---

## `cascade = CascadeType.ALL`

Si el Local:

- se crea,
- se actualiza,
- o se elimina,

sus Juegos también serán afectados automáticamente.

---

## `fetch = FetchType.LAZY`

Los Juegos solo se cargarán cuando realmente los necesites.

---

# 🎮 Código del Juego
## 🔒 El dueño del candado real

`@ManyToOne(fetch = FetchType.LAZY)
@JoinColumn(name = "local_id")
private Local local;`

---

# 📌 Regla importante

Aunque la relación sea bidireccional:

la columna física sigue existiendo solamente en la tabla Juegos.

---

# 🧠 Resumen rápido

## 🛣️ Unidireccional

- Solo `Juego` conoce al `Local`.
- El `Local` no tiene lista de juegos.
- Más simple y ligero.

---

## 🔄 Bidireccional

- `Juego` conoce al `Local`.
- `Local` tiene `List<Juego>`.
- Permite navegar en ambas direcciones.

---

# 🎮 Explicación para recordar

## Unidireccional

📦 El Juego tiene el sticker.

🏢 El Local no tiene lista.

---

## Bidireccional

📦 El Juego tiene el sticker.

🏢 El Local tiene una carpeta con todos sus juegos.

---

## 🔙 [***Volver***](./entidades.md)