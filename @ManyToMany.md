## 🔙 [***Volver***](./entidades.md)
---

# 🔗 `@ManyToMany`
## 🎮 Relación Muchos a Muchos

Esta es una relación donde:

- muchos Clientes pueden jugar muchos Juegos,
- y muchos Juegos pueden pertenecer a muchos Clientes.

---

# 🧠 La idea importante

En bases de datos:

## ❌ Un "Muchos a Muchos" no existe directamente.

Por eso Spring Boot crea una:

# 🌉 Tabla Intermedia

También llamada:

- tabla puente,
- tabla intermedia,
- o tabla de unión.

---

# 👶 Explicación sencilla

Imagina un:

## 📘 Libro de Registro

Cada vez que un Cliente juega un Juego,
el sistema escribe algo así:

- Juan → Minecraft
- María → FIFA
- Luis → Mario Kart

La tabla puente guarda esas conexiones.

---

# 🛣️ Relación Unidireccional
## 🎮 El Juego conoce a sus Clientes

Aquí:

- el Juego sí tiene la lista de Clientes,
- pero el Cliente NO sabe qué Juegos ha jugado.

---

# 👶 Explicación sencilla

Cada Juego tiene un papel pegado atrás con los nombres de los niños que lo jugaron.

Entonces:

- Minecraft sabe quién lo jugó,
- pero Juan no tiene ninguna lista de juegos en su casa.

---

# 🧩 Código del Juego
## 🎮 El dueño de la lista

```java
@ManyToMany
@JoinTable(
    name = "juego_cliente_uni", // Nombre de la tabla puente en la base de datos
    joinColumns = @JoinColumn(name = "juego_id"), // Cómo se llamará mi ID en la tabla puente
    inverseJoinColumns = @JoinColumn(name = "cliente_id") // Cómo se llamará el ID de la otra tabla
)
private List<Cliente> clientes;
```

---

# 🔍 Explicación

## `@ManyToMany`

Muchos Juegos pueden conectarse con muchos Clientes.

---

## `@JoinTable`

Crea la tabla intermedia.

---

## `name = "juego_cliente_uni"`

Nombre de la tabla puente en SQL.

---

## `joinColumns`

Define cómo se llamará el ID del Juego dentro de la tabla puente.

---

## `inverseJoinColumns`

Define cómo se llamará el ID del Cliente dentro de la tabla puente.

---

# 👤 Código del Cliente
## 😴 El Cliente distraído

```java
@Entity
public class Cliente {

    private Long id;
    private String nombre;

}
```

---

# 📌 Importante

Aquí NO existe:

`List<Juego> juegos`

porque el Cliente todavía no sabe qué Juegos ha jugado.

---

# 🔄 Relación Bidireccional
## 🔁 Todos se conocen

Ahora queremos que:

- el Juego conozca sus Clientes,
- y el Cliente conozca sus Juegos.

---

# 👶 Explicación sencilla

El Juego sigue teniendo su lista de niños.

Pero ahora el Cliente también recibe un:

## 📔 Álbum de Juegos

Cada vez que juega algo nuevo,
lo guarda en su álbum.

Ahora ambos se conocen.

---

# 🎮 Código del Juego
## 🏆 El dueño de la tabla puente

```java
@ManyToMany
@JoinTable(
    name = "juego_cliente_bi",
    joinColumns = @JoinColumn(name = "juego_id"),
    inverseJoinColumns = @JoinColumn(name = "cliente_id")
)
private List<Cliente> clientes;
```

---

# 👤 Código del Cliente
## 📔 El dueño del álbum

```java
@ManyToMany(mappedBy = "clientes", fetch = FetchType.LAZY)
private List<Juego> juegos;
```

---

# 🔍 Explicación

## `mappedBy = "clientes"`

Le dice a Spring:

> "La relación real ya fue creada en la variable `clientes` de la clase Juego".

Gracias a esto:

- NO se crea otra tabla,
- NO se duplican relaciones,
- y todo queda conectado correctamente.

---

## `fetch = FetchType.LAZY`

Los Juegos solo se cargarán cuando realmente se necesiten.

Esto protege la memoria del servidor.

---

# ⚠️ Reglas importantes de `@ManyToMany`

---

# 🦥 Usa siempre `LAZY`

Si fuera `EAGER`:

buscar un Cliente podría traer:

- miles de Juegos,
- miles de Clientes,
- y muchísima información innecesaria.

---

# 🚫 Evita `CascadeType.REMOVE`

Imagina esto:

- borras un Cliente,
- y accidentalmente también se borran todos los Juegos.

💥 Sería un desastre.

Por eso en `@ManyToMany` normalmente:

- NO se usa `REMOVE`,
- y tampoco `CascadeType.ALL`.

---

# 🧠 Resumen rápido

## 🛣️ Unidireccional

- Juego → conoce Clientes.
- Cliente → no conoce Juegos.

---

## 🔄 Bidireccional

- Juego → conoce Clientes.
- Cliente → conoce Juegos.

---

# 🌉 La tabla puente

Spring crea automáticamente una tabla intermedia que conecta:

- `juego_id`
- con `cliente_id`

---

# 🎮 Explicación para recordar

## Unidireccional

🎮 El Juego tiene la lista.

👤 El Cliente no recuerda nada.

---

## Bidireccional

🎮 El Juego tiene la lista.

👤 El Cliente tiene un álbum con sus Juegos.

---
## 🔙 [***Volver***](./entidades.md)