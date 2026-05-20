## 🔙 [***Volver***](./desarrolloAplicacion.md)

---

# 🗄️ La Capa de Persistencia

Este módulo se encarga de trabajar con la base de datos.

Aquí definimos cómo las clases Java se conectan con tablas reales de una base de datos relacional.

---

# 🧩 Tecnologías utilizadas

## ☕ JPA
### *(Java Persistence API)*

Es la especificación que define cómo trabajar con persistencia de datos en Java.

Permite:

- guardar objetos,
- consultar información,
- actualizar datos,
- eliminar registros.

---

## ⚙️ Hibernate

Es la implementación más usada de JPA en Spring Boot.

Hibernate se encarga de convertir automáticamente:

- objetos Java → tablas SQL,
- tablas SQL → objetos Java.

---

# 📦 Starter necesario

Para usar persistencia de datos debes agregar dependencias en el archivo:

```text
    `pom.xml`
```

---

# 📚 Dependencias necesarias

## spring-boot-starter-data-jpa
```xml
<dependency>

    <groupId>org.springframework.boot</groupId>

    <artifactId>spring-boot-starter-data-jpa</artifactId>

</dependency>
```

---

## Coneccion Base de datos (mysql)

```xml
<dependency>

    <groupId>com.mysql</groupId>

    <artifactId>mysql-connector-j</artifactId>

    <scope>runtime</scope>

</dependency>
```

---

# ⚙️ ¿Qué hace cada dependencia?

| Dependencia | Función |
|---|---|
| ***`spring-boot-starter-data-jpa`*** | Agrega soporte para JPA y Hibernate |
| ***`mysql-connector-j`*** | Permite conectar Spring Boot con MySQL |

---

- [***conexión Base de Datos***](./conexiónBD.md)
- [***Entidades***](./entidades.md)
- [***Repositorio***](./Repositorio.md)

---

## 🔙 [***Volver***](./desarrolloAplicacion.md)