## 🔙 [***Volver***](./entidades.md)

---

# 🆔 `@Id` y `@GeneratedValue`
## Identificadores automáticos en JPA

Estas anotaciones se utilizan dentro de una `@Entity`
para definir la clave primaria de una tabla.

---

# 🆔 `@Id`
## La clave primaria

La anotación:

```java
@Id
```

indica cuál atributo será el identificador único del registro.

---

## ⚙️ ¿Qué hace?

Le dice a JPA y Hibernate:

> “Este campo identifica de forma única cada fila de la tabla”.

---

## 📌 Ejemplo conceptual

```java
@Id
private UUID id;
```

---

## 💡 Idea rápida

```java
@Id
```

funciona como:

## 🪪 El documento de identidad del registro

No puede repetirse.

---

# 🔄 `@GeneratedValue`
## Generación automática de IDs

La anotación:

```java
@GeneratedValue
```

permite que la base de datos genere automáticamente el valor del `id`.

---

## ⚙️ ¿Qué hace?

Evita que tengas que escribir manualmente los IDs.
La base de datos los crea automáticamente.

---

## 📌 Ejemplo conceptual

```java
@Id
@GeneratedValue
private UUID id;
```

---

# 🧩 Estrategias de generación

```java
@GeneratedValue
```

puede usar diferentes estrategias.

---

## 🔢 `GenerationType.IDENTITY`

La base de datos incrementa el ID automáticamente.
Es la más usada con MySQL.

### 📌 Ejemplo

```java
@GeneratedValue(strategy = GenerationType.IDENTITY)
```

---

## 🧠 `GenerationType.AUTO`

Hibernate decide automáticamente la mejor estrategia.

---

## 📚 `GenerationType.SEQUENCE`

Usa secuencias numéricas.
Muy común en PostgreSQL y Oracle.

---

## 🗂️ `GenerationType.TABLE`

Guarda los IDs en una tabla especial.
Menos utilizada actualmente.

---

# 📌 Resumen rápido

| Anotación | Función |
|---|---|
| ***`@Id`*** | Define la clave primaria |
| ***`@GeneratedValue`*** | Genera IDs automáticamente |

---

# 💡 Idea rápida

Estas anotaciones trabajan juntas para:

- identificar registros,
- evitar IDs repetidos,
- automatizar la generación de claves primarias.

---

## 🔙 [***Volver***](./entidades.md)