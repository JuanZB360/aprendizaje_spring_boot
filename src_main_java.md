## 🔙 [***Volver***](./estructuraCarpetas.md)

---

# ☕ El Directorio de Código
## `src/main/java`

Este es el lugar principal donde vive todo el código Java de la aplicación.

Aquí es donde crearás:

- controllers.
- services.
- repositories.
- entities.
- y configuraciones.

También es donde aplicarás las anotaciones de Spring para gestionar el flujo de la aplicación.

---

# 🚀 La Clase Principal

Dentro de este directorio se encuentra la clase principal del proyecto.

Normalmente contiene la anotación:

- [***`@SpringBootApplication`***](./@SpringBootApplication.md)
- [***`@SpringBootConfiguration o @Configuration`***](./@SpringBootConfiguration.md)
- [***`@ENableAutoConfiguration`***](./@ENableAutoConfiguration.md)
- [***`@ComponentScan`***](./@ComponentScan.md)
---

# 📌 ¿Por qué es importante?

La clase principal debe estar ubicada en la raíz del proyecto Java.

Esto permite que Spring Boot pueda encontrar automáticamente:

- `@Component`
- `@Service`
- `@Repository`
- `@Controller`
- y otros Beans.

Gracias al escaneo automático llamado:

## 🔍 `@ComponentScan`

---

# ⚙️ ¿Qué hace `@ComponentScan`?

Busca clases anotadas dentro del proyecto y las registra automáticamente como Beans de Spring.

Así Spring puede:

- crear objetos,
- conectarlos,
- e inyectar dependencias automáticamente.

---

# 💡 Idea rápida

La clase principal funciona como el:

## 🎯 Punto de inicio de toda la aplicación

Desde ahí Spring Boot comienza a:

- escanear paquetes.
- registrar componentes.
- y levantar todo el sistema.

---

## 🔙 [***Volver***](./estructuraCarpetas.md)