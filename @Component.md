## 🔙 [***Volver***](./definir.md)

---

# 1. `@Component`
## 🧩 La etiqueta base

`@Component` es la madre de todas las anotaciones de estereotipo en Spring.

Es la forma más básica de decirle al framework:

> “Esta clase es importante, regístrala y adminístrala automáticamente”.

---

# ❓ ¿Qué hace?

Marca una clase como un **Componente de Spring**.

Cuando Spring detecta esta anotación:

- crea un objeto automáticamente,
- lo guarda en el contenedor de Beans,
- y permite que otras clases puedan usarlo mediante inyección de dependencias.

---

# 🧠 ¿Qué significa esto?

En Java tradicional normalmente crearías objetos así:

```java id="jav-new1"
Calculadora calculadora = new Calculadora();
```

Con Spring y @Component, ya no necesitas hacerlo manualmente. Spring crea y administra el objeto por ti.

---

## 🕒 ¿Cuándo usarla?

Se utiliza cuando tienes una clase que realiza una tarea general y no encaja exactamente en otras categorías más específicas.

## 📌 Ejemplos comunes

- utilidades de fechas,
- validadores personalizados,
- convertidores,
- helpers,
- herramientas internas.

---

## ⚠️ ¿Cuándo NO usarla?

Si la clase tiene una responsabilidad más específica, es mejor usar:

| Anotación         | Uso recomendado   |
| ----------------- | ----------------- |
| `@Service`        | Lógica de negocio |
| `@Repository`     | Acceso a datos    |
| `@RestController` | Peticiones HTTP   |

Todas estas anotaciones internamente son una forma especializada de @Component.

---

## 🎭 Personalidad

Es el comodín del sistema.

Si no sabes exactamente qué categoría usar, pero quieres que Spring gestione la clase, puedes usar @Component.

---

## 🔄 Relación con Spring

Cuando Spring inicia la aplicación:

- escanea el proyecto,
- busca anotaciones como @Component,
- crea los objetos automáticamente,
- y los guarda dentro del contenedor IoC.

---

## ⚙️ Ejemplo conceptual

```java
@Component
public class ValidadorCorreo {

}
```
Spring detectará esta clase y la convertirá automáticamente en un Bean.

---

## 🧠 Idea clave

@Component convierte una clase común en un objeto administrado por Spring.

Esto permite:

- reutilizar componentes,
- conectar clases automáticamente,
- y reducir código manual.

---

## 📌 Resumen rápido

| Concepto          | Significado                        |
| ----------------- | ---------------------------------- |
| `@Component`      | Componente administrado por Spring |
| Función principal | Registrar clases como Beans        |
| Uso común         | Utilidades y componentes generales |
| Trabaja junto a   | Inyección de dependencias          |
| Personalidad      | El comodín del sistema             |

---

## 🔙 [***Volver***](./definir.md)