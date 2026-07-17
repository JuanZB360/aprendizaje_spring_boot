## 🔙 [***Volver***](./Readme.md)

---

# 🏗️ Arquitectura por Capas
## El flujo de datos

En Spring Boot no se recomienda colocar todo el código en un solo archivo.

La aplicación se divide en varias capas, cada una con una responsabilidad específica.

---

# 🎯 Objetivo

Separar responsabilidades para que el proyecto sea:

- más organizado.
- más fácil de mantener.
- más sencillo de escalar.

---

# 🔄 Flujo general

Las capas se comunican en un solo sentido:

Cliente → Controller → Service → Repository → Base de Datos

---

# 🧩 Capas principales

---

- [`Persistencia de datos`](./persistencia.md)
- [`Servicio`](./servicios.md)
- [`Control`](./control.md)

---

## 🔙 [***Volver***](./Readme.md)