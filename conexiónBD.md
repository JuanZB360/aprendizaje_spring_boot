## 🔙 [***Volver***](./persistencia.md)

---

# 🗄️ Configuración de Base de Datos
## `application.properties`

Estas propiedades permiten conectar Spring Boot con un motor de base de datos y configurar el comportamiento de JPA e Hibernate.

---

# 🌐 1. Dirección de la Base de Datos

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/monto_app_db?useSSL=false&serverTimezone=UTC&allowPublicKeyRetrieval=true
```
---

## ⚙️ `spring.datasource.url`

Es la propiedad que Spring Boot utiliza para saber dónde se encuentra la base de datos.

---

## ☕ `jdbc:mysql://`

Define el protocolo de conexión.

- `JDBC`
  → Tecnología de Java para conectarse a bases de datos.

- `mysql`
  → Indica que el motor utilizado es MySQL.

---

## 💻 `localhost`

Indica que la base de datos está ejecutándose en la misma computadora.

Si estuviera en otro servidor, aquí se colocaría:

- una IP,
- un dominio.

---

## 🔌 `3306`

Es el puerto por defecto utilizado por MySQL.

---

## 🗂️ `/monto_app_db`

Es el nombre exacto de la base de datos.

---

## 🔒 `useSSL=false`

Desactiva SSL en desarrollo local.
Ayuda a evitar errores de conexión innecesarios.

---

## 🕒 `serverTimezone=UTC`

Sincroniza la hora de Java con la base de datos usando el estándar UTC.

Evita problemas de fechas y horarios incorrectos.

---

## 🔑 `allowPublicKeyRetrieval=true`

Permite que MySQL acepte conexiones modernas desde Java usando autenticación segura.

---

# 🔐 2. Credenciales de Seguridad

---

## 👤 Usuario

```properties
spring.datasource.username=root
```

Define el usuario de MySQL.

Normalmente:

```text
root
```
---

## 🔑 Contraseña

```propierties
spring.datasource.password=TuContraseña
```

Define la contraseña del usuario de MySQL.

---

# 🔄 3. El Driver de Conexión

```propierties
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
```

---

## ⚙️ ¿Qué hace?

Define qué librería utilizará Spring Boot para comunicarse con MySQL.

---

## 💡 Idea rápida

El Driver funciona como:

## 🌍 Un traductor

Convierte instrucciones Java en consultas SQL que MySQL pueda entender.

---

# 🧩 4. Configuración de JPA e Hibernate

---

## ⚙️ `spring.jpa.hibernate.ddl-auto=update`

Controla cómo Hibernate maneja las tablas de la base de datos.

---

## 🔄 `update`

Compara las clases `@Entity`
con las tablas reales de MySQL.

Si detecta cambios:

- crea tablas,
- agrega columnas,
- actualiza la estructura automáticamente.

---

## ✅ Ventaja

No elimina los datos existentes.

---

# 📋 5. Logs y Diagnóstico SQL

---

## 👀 Mostrar consultas SQL

```properties
spring.jpa.show-sql=true
```

Hace que Spring imprima en consola todas las consultas SQL ejecutadas.

Por ejemplo:

- `SELECT`
- `INSERT`
- `UPDATE`
- `DELETE`

---

## 🧼 Formatear SQL

```properties
spring.jpa.properties.hibernate.format_sql=true
```

Organiza las consultas SQL para que sean más fáciles de leer.

---

# 💡 Idea rápida

Estas configuraciones ayudan a:

- depurar errores,
- entender qué hace Hibernate,
- visualizar cómo trabaja la base de datos.

---

# 📌 Resumen rápido

| Propiedad | Función |
|---|---|
| `spring.datasource.url` | Dirección de la base de datos |
| `spring.datasource.username` | Usuario MySQL |
| `spring.datasource.password` | Contraseña MySQL |
| `driver-class-name` | Driver de conexión |
| `ddl-auto=update` | Actualiza tablas automáticamente |
| `show-sql=true` | Muestra consultas SQL |
| `format_sql=true` | Formatea consultas SQL |

---
## 🔙 [***Volver***](./persistencia.md)