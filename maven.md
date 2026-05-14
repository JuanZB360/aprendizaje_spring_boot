## 🔙 [***Volver***](./project.md)

---

# 📦 Apache Maven
## El estándar del mundo Java

Apache Maven es el gestor de dependencias más antiguo y más utilizado dentro del ecosistema Java.

Spring Boot lo usa ampliamente para:

- 📥 descargar librerías,
- 🔄 manejar dependencias,
- ☕ compilar proyectos,
- 🧪 ejecutar pruebas,
- 📦 generar aplicaciones listas para producción.

---

# 📄 Archivo principal

Maven trabaja utilizando un archivo llamado:

```text
pom.xml
```

Dentro de este archivo se define toda la configuración del proyecto.

---

# ⚙️ ¿Cómo funciona?

Maven sigue un ciclo de vida estricto y muy organizado.

Cada proyecto pasa automáticamente por varias etapas.

---

## 📌 Ciclo de vida típico

```text
Limpiar → Compilar → Probar → Empaquetar
```

---

# 🧹 Fases más importantes

| Fase | Función |
|---|---|
| `clean` | Limpia archivos anteriores |
| `compile` | Compila el código Java |
| `test` | Ejecuta pruebas automáticas |
| `package` | Genera el archivo `.jar` o `.war` |

---

# 🧩 Formato XML

Maven utiliza XML para definir configuraciones y dependencias.

---

## 📌 Ejemplo de dependencia

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

---

Aquí se le indica a Maven que descargue el Starter Web de Spring Boot.

---

# ✅ Ventajas de Maven

---

    ## 🌍 Es el estándar de Java

    La mayoría de empresas tradicionales utilizan Maven.

---

    ## 📚 Muchísima documentación

    Casi cualquier error que encuentres en internet tendrá ejemplos o soluciones usando Maven.

---

    ## 🧠 Fácil de aprender

    Aunque el archivo `pom.xml` puede verse grande,
    su estructura es muy descriptiva y organizada.

---

    ## 🔒 Configuración estable

    Maven es rígido y estructurado.

    Eso ayuda a mantener proyectos:

    - ordenados,
    - consistentes,
    - y fáciles de entender.

---

# ⚠️ Desventajas

---

    ## 📄 XML muy extenso

    Los archivos `pom.xml` pueden hacerse largos y difíciles de leer en proyectos grandes.

---

    ## 🐢 Menos flexible

    Comparado con Gradle,
    Maven tiene menos libertad para personalizar tareas complejas.

---

# 🎭 Personalidad

Maven es como un:

## 🏢 Gerente tradicional

Todo está:

- organizado,
- documentado,
- estructurado,
- siguiendo reglas claras.

---

# 📌 Resumen rápido

| Concepto | Significado |
|---|---|
| Maven | Gestor de dependencias para Java |
| Archivo principal | `pom.xml` |
| Función principal | Administrar librerías y construir proyectos |
| Ventaja principal | Estabilidad y popularidad |
| Desventaja principal | XML extenso |
| Ideal para | Principiantes y proyectos empresariales |

---

## 🔙 [***Volver***](./project.md)