## 🔙 [***Volver***](./anotaciones.md)

---

# A. Para definir “Quién es quién”
## 🧩 Estereotipos

Estas anotaciones le indican a Spring que una clase es un Bean y cuál es su responsabilidad dentro de la aplicación.

---

# 📚 Principales Anotaciones en Spring Boot

| Anotación | Función |
|---|---|
| 🧩 [**@Component**](./@Component.md) | Componente base administrado por Spring. |
| 🌐 [**@Controller / @RestController**](./@Controller.md) | Manejo de rutas y peticiones HTTP. |
| 🧠 [**@Service**](./@Service.md) | Contiene la lógica de negocio. |
| 🗄️ [**@Repository**](./@Repository.md) | Comunicación con la base de datos. |
| 📄 [**@Entity**](./@Entity.md) | Representa una tabla de la base de datos. |

---

# 🔄 ¿Cómo se conectan todas?

Para que una persona sin conocimientos lo visualice, el flujo normalmente funciona así:

```text
Cliente
   ↓
@RestController
   ↓
@Service
   ↓
@Repository
   ↓
@Entity
   ↓
Base de Datos
```

---

📌 **Flujo explicado**
- El cliente hace una petición.
- La recibe el @RestController.
- El @RestController le pide ayuda al @Service.
- El @Service hace los cálculos y validaciones.
- Luego le pide datos al @Repository.
- El @Repository busca o guarda una @Entity en la base de datos.

---

## 🔙 [***Volver***](./anotaciones.md)