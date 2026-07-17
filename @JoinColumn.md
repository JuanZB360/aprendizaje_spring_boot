## 🔙 [***Volver***](./entidades.md)

---

# 🛠️ Los atributos ocultos de `@JoinColumn`

La anotación `@JoinColumn` también tiene reglas especiales que permiten controlar cómo se comporta la conexión entre tablas en la base de datos.

Estas configuraciones ayudan a crear relaciones más seguras, ordenadas y estrictas.

---

# 1. `nullable`
## 🚫 ¿La relación es obligatoria?

Define si la columna puede quedar vacía (`NULL`) o no.

---

### `nullable = false`

El Local está obligado a tener un Gerente.

Si intentas guardar un Local sin gerente, la base de datos lanzará un error.

💡 Ejemplo simple:

- Un local comercial no puede abrir sin administrador.

---

### `nullable = true`

El Local puede existir temporalmente sin gerente.

La columna podrá guardar un valor vacío (`NULL`).

💡 Ejemplo simple:

- El gerente renunció y todavía no han contratado uno nuevo.

---

## 📌 Ejemplo

`@JoinColumn(name = "gerente_id", nullable = false)`

---

# 2. `unique`
## 🔒 ¿Se puede repetir?

Controla si el mismo gerente puede estar conectado a varios locales.

---

### `unique = true`

El gerente solo puede pertenecer a un único local.

La base de datos bloqueará duplicados automáticamente.

💡 Ejemplo simple:

- Carlos ya administra el Local A.
- No puede aparecer también como gerente del Local B.

---

### `unique = false`

El mismo gerente podría repetirse en varios registros.

⚠️ Esto normalmente NO se usa en relaciones `@OneToOne`.

---

## 📌 Ejemplo

`@JoinColumn(name = "gerente_id", unique = true)`

---

# 3. `insertable`
## ✍️ ¿Se puede escribir?

Define si Java puede insertar valores en esa columna cuando se crea el registro.

---

### `insertable = true`

Spring puede guardar el valor normalmente.

---

### `insertable = false`

Spring NO podrá escribir en esa columna al crear el registro.

💡 Uso común:

Cuando la columna es controlada automáticamente por otra parte del sistema.

---

## 📌 Ejemplo

`@JoinColumn(name = "gerente_id", insertable = false)`

---

# 4. `updatable`
## 🔄 ¿Se puede modificar?

Define si la columna puede cambiar después de guardar el registro.

---

### `updatable = true`

La relación puede modificarse normalmente.

💡 Ejemplo:

- Cambiar un gerente viejo por uno nuevo.

---

### `updatable = false`

La relación queda bloqueada después de crearse.

💡 Ejemplo:

- Una vez asignado el gerente, nadie puede cambiarlo.

---

## 📌 Ejemplo

`@JoinColumn(name = "gerente_id", updatable = false)`

---

# 🧠 Ejemplo completo

`@JoinColumn(
    name = "gerente_id",
    nullable = false,
    unique = true,
    insertable = true,
    updatable = false
)`

---

# 🎮 Explicación sencilla

Imagina que `@JoinColumn` es el carnet de identificación del gerente dentro del Local.

Los atributos funcionan como reglas del sistema:

- `nullable` → ¿Puede faltar el gerente?
- `unique` → ¿Puede repetirse?
- `insertable` → ¿Se puede registrar?
- `updatable` → ¿Se puede cambiar después?

---

# 📌 Resumen rápido

| Configuración | Función |
|---|---|
| `nullable` | Permite o prohíbe valores vacíos |
| `unique` | Evita valores repetidos |
| `insertable` | Permite insertar datos |
| `updatable` | Permite modificar datos |

---

## 🔙 [***Volver***](./entidades.md)