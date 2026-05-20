## 🔙 [***Volver***](./persistencia.md)

---

# 🗄️ `@Table`
## La anotación para nombrar tablas

La anotación:

```java
@Table
```
se utiliza junto con `@Entity`
para indicar a qué tabla de la base de datos pertenece una clase.

---

# ⚙️ ¿Qué hace?

Le dice a Hibernate y a JPA:

> “Esta clase debe conectarse con esta tabla específica”.

---

# 📌 Ejemplo simple

Si tienes una clase llamada:

`Usuario`

Spring normalmente intentará crear una tabla llamada:

`usuario`

Pero con `@Table`
puedes personalizar el nombre.

---

# 🧩 Ejemplo conceptual

```java
@Entity
@Table(name = "usuarios")
```

La clase quedará vinculada a la tabla:

`usuarios`

---

# 🚀 ¿Por qué es útil?

Permite:

- usar nombres personalizados.
- conectar tablas existentes.
- mantener una mejor organización en la base de datos.

---

# 💡 Idea rápida

```java
@Entity
```
dice:

> “Esta clase es una tabla”.

Mientras que:

```java
@Table
```
dice:

> “Y este es el nombre exacto de esa tabla”.

---

# 📌 Uso común

`@Table`
normalmente se usa para:

- cambiar nombres de tablas.
- evitar conflictos de nombres.
- mapear bases de datos ya existentes.

---

## 🔙 [***Volver***](./persistencia.md)