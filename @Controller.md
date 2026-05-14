## 🔙 [***Volver***](./definir.md)

---

# 2. `@Controller` / `@RestController`
## 🌐 La cara al público

Estas anotaciones definen cómo se deben comportar los componentes que reciben solicitudes externas dentro de la aplicación.

Son la puerta de entrada entre el usuario y el sistema.

---

# ❓ ¿Qué hacen?

Le indican a Spring que esta clase se encargará de recibir peticiones HTTP.

Estas peticiones pueden venir desde:

- un navegador,
- una aplicación móvil,
- otra API,
- o cualquier cliente externo.

---

# 🌍 ¿Qué es una petición HTTP?

Es una solicitud que hace un usuario o sistema para pedir información o ejecutar una acción.

## 📌 Ejemplos comunes

```text
GET    /usuarios
POST   /productos
DELETE /ventas/1
PUT    /clientes/5
```

---

## @Controller

Se utiliza principalmente cuando la aplicación devuelve vistas o páginas web.

Por ejemplo:

- HTML
- JSP
- Thymeleaf
- @RestController

Es la versión moderna y más utilizada en APIs REST.

---

Devuelve datos directamente, normalmente en formato:

```json
{
  "nombre": "Juan",
  "edad": 25
}
```

---

## 🕒 ¿Cuándo usarla?

Siempre que necesites crear:

- 🌐 rutas,
- 🔗 endpoints,
- 📡 APIs,
- 📥 respuestas HTTP.
- 🎭 Personalidad

Es el Recepcionista del sistema.

---

Su trabajo consiste en:

- escuchar al cliente,
- entender qué necesita,
- y pasarle la solicitud al @Service.
- 🔄 Relación con otras capas

El @RestController normalmente es el primer componente que participa en el flujo de la aplicación.

---
```text
Cliente
   ↓
@RestController
   ↓
@Service
   ↓
@Repository
   ↓
Base de Datos
```
Spring detecta esta clase automáticamente y la registra como un Bean encargado de manejar peticiones HTTP.

---

## 🧠 Idea clave

El @Controller o @RestController NO contiene la lógica del negocio.

Su responsabilidad es únicamente:

- recibir solicitudes,
- devolver respuestas,
- y comunicarse con el @Service.

---

## 📌 Resumen rápido

| Concepto          | Significado                  |
| ----------------- | ---------------------------- |
| `@Controller`     | Maneja vistas y páginas web  |
| `@RestController` | Maneja APIs y devuelve datos |
| Función principal | Recibir peticiones HTTP      |
| Trabaja junto a   | `@Service`                   |
| Personalidad      | El Recepcionista del sistema |

---

## 🔙 [***Volver***](./definir.md)