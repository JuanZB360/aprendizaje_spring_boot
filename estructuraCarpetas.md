## 🔙 [***Volver***](./project.md)

---

Mapa Estructural del Proyecto Spring Boot

# 🗺️ Mapa Estructural del Proyecto Spring Boot

```plaintext
nombre-de-tu-proyecto/
│
├── .mvn/                         ← Carpeta del Maven Wrapper
│   └── wrapper/
│       ├── maven-wrapper.jar
│       └── maven-wrapper.properties
│
├── src/                          ← Carpeta principal del código fuente
│
│   ├── main/                     ← Código de producción
│   │
│   │   ├── java/                 ← Archivos Java
│   │   │   └── com/tuempresa/
│   │   │       └── ProyectoApp.java
│   │   │
│   │   └── resources/            ← Recursos y configuración
│   │       ├── static/
│   │       ├── templates/
│   │       └── application.properties
│   │
│   └── test/                     ← Código de pruebas
│       └── java/
│
├── .gitignore                    ← Archivos ignorados por Git
├── HELP.md                       ← Guía rápida generada por Spring
├── mvnw                          ← Script Maven para Linux/Mac
├── mvnw.cmd                      ← Script Maven para Windows
└── pom.xml                       ← Dependencias y configuración Maven
```
---

- [`mvnw` y `mvnw.cmd`](./mvnw.md)
- [`src/main/java`](./src_main_java.md)
- [`src/main/resources`](./src_main_resources.md)
- [`pom.xml`](./pom.md)

---

## 🔙 [***Volver***](./project.md)