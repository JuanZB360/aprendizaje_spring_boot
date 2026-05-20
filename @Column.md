## 🔙 [***Volver***](./persistencia.md)

---

# 📄 `@Column`
## La anotación para columnas

La anotación:

```java
@Column
```

se utiliza para configurar cómo un atributo de una clase se guarda dentro de una columna de la base de datos.

---

# ⚙️ ¿Qué hace?

Le dice a JPA y Hibernate:

> “Este atributo debe almacenarse en esta columna”.

---

# 📌 Ejemplo conceptual

Si tienes un atributo:

```java
private String nombre;
```

Spring normalmente creará una columna llamada:

```text
nombre
```

Pero con `@Column` puedes personalizar su comportamiento.

---

# 🧩 Ejemplo simple

```java
@Column(name = "nombre_completo")
```

Esto conecta el atributo Java con la columna:

```text
nombre_completo
```

---

# 🚀 ¿Qué se puede configurar?

Con `@Column` puedes definir:

- nombre de la columna,
- longitud máxima,
- si acepta valores nulos,
- si es única,
- otras restricciones.

---

# ⚙️ Configuraciones de `@Column`

La anotación:

`@Column`

permite personalizar cómo un atributo Java será almacenado dentro de una columna de la base de datos.

---

# 🧩 Estructura básica

`@Column(...)`

Dentro de los paréntesis se agregan configuraciones.

---

# 📚 Configuraciones más importantes

---

## 🏷️ `name`

Define el nombre exacto de la columna en la base de datos.

### 📌 Ejemplo

`@Column(name = "nombre_completo")`

---

## ❌ `nullable`

Define si la columna permite valores `null`.

| Valor | Significado |
|---|---|
| `true` | Permite valores vacíos |
| `false` | Obliga a tener un valor |

### 📌 Ejemplo

`@Column(nullable = false)`

---

## 🔒 `unique`

Indica si los valores deben ser únicos.

Evita datos repetidos.

### 📌 Ejemplo

`@Column(unique = true)`

---

## 📏 `length`

Define el tamaño máximo de caracteres para textos (`String`).

### 📌 Ejemplo

`@Column(length = 100)`

---

## 🔢 `precision`

Define la cantidad total de dígitos en números decimales.

Se usa con:

- `BigDecimal`
- `Double`
- `Float`

### 📌 Ejemplo

`@Column(precision = 10)`

Permite hasta 10 dígitos en total.

---

## 🎯 `scale`

Define cuántos decimales puede tener un número.

Se usa junto con `precision`.

### 📌 Ejemplo

`@Column(precision = 10, scale = 2)`

Resultado:

`12345678.90`

---

## 📝 `columnDefinition`

Permite escribir manualmente la definición SQL de la columna.

### 📌 Ejemplo

`@Column(columnDefinition = "TEXT")`

---

## 🚫 `insertable`

Define si el campo puede insertarse al crear registros.

| Valor | Significado |
|---|---|
| `true` | Puede insertarse |
| `false` | No se insertará automáticamente |

### 📌 Ejemplo

`@Column(insertable = false)`

---

## 🔄 `updatable`

Define si el campo puede modificarse después de creado.

### 📌 Ejemplo

`@Column(updatable = false)`

---

## 📥 `table`

Permite indicar a qué tabla pertenece la columna.

Se usa principalmente en mapeos avanzados.

### 📌 Ejemplo

`@Column(table = "usuarios")`

---

# 🧠 Ejemplo completo

`@Column(`

`    name = "correo",`

`    nullable = false,`

`    unique = true,`

`    length = 120`

`)`

---

# 💡 Idea rápida

`@Column`
permite controlar:

- cómo se guarda el dato,
- qué restricciones tiene,
- y cómo se comporta dentro de la base de datos.

---

# 📌 Resumen rápido

| Configuración | Función |
|---|---|
| `name` | Cambia el nombre de la columna |
| `nullable` | Permite o bloquea valores nulos |
| `unique` | Evita valores repetidos |
| `length` | Tamaño máximo de texto |
| `precision` | Cantidad total de dígitos |
| `scale` | Cantidad de decimales |
| `columnDefinition` | SQL personalizado |
| `insertable` | Permite insertar valores |
| `updatable` | Permite actualizar valores |
| `table` | Define tabla específica |

---

---

# 💡 Idea rápida

`@Column`
funciona como:

## 🏷️ La etiqueta de una columna

Define cómo se almacenará un dato dentro de la tabla.

---

# 🧠 Relación con otras anotaciones

| Anotación | Función |
|---|---|
| `@Entity` | Define una tabla |
| `@Table` | Define el nombre de la tabla |
| `@Column` | Define columnas de la tabla |

---

## 🔙 [***Volver***](./persistencia.md)