## 🔙 [***Volver***](./definir.md)

---

# 4. `@Repository`
## 🗄️ El bibliotecario

Esta anotación se encarga de gestionar cómo se acceden, consultan y almacenan los datos dentro de la aplicación.

---

# ❓ ¿Qué hace?

Le dice a Spring que esta clase se comunicará directamente con la base de datos.

Además, posee un “superpoder” muy importante:

> Traduce errores complejos de la base de datos a excepciones de Spring más fáciles de entender y manejar.

Esto ayuda a que el código sea más limpio y más sencillo de depurar.

---

# 🕒 ¿Cuándo usarla?

Se utiliza en clases o interfaces encargadas de realizar operaciones CRUD.

## 📌 CRUD significa:

| Operación | Acción |
|---|---|
| ➕ Create | Crear datos |
| 📖 Read | Leer datos |
| ✏️ Update | Actualizar datos |
| ❌ Delete | Eliminar datos |

---

# 🎭 Personalidad

Es el **Bibliotecario** del sistema.

Su trabajo consiste en:

- entrar al depósito (la base de datos),
- buscar la información,
- guardarla,
- actualizarla,
- y entregársela al `@Service`.

---

# 🔄 Relación con otras capas

El `@Repository` normalmente trabaja junto a:

```text
@RestController
      ↓
@Service
      ↓
@Repository
      ↓
Base de Datos
```

---

### ***El @Service le pide datos al @Repository, y este se encarga de buscarlos o guardarlos.***

---

## ⚙️ Ejemplo conceptual

```java
@Repository
public class UsuarioRepository {

}
```

Spring detecta esta clase automáticamente y la registra como un Bean especializado en acceso a datos.

---

## 🧠 Idea clave

El @Repository NO contiene lógica de negocio.

Su responsabilidad es únicamente:

- consultar datos,
- guardar información,
- actualizar registros,
- y eliminar datos.

---

## 📌 Resumen rápido

| Concepto          | Significado                          |
| ----------------- | ------------------------------------ |
| `@Repository`     | Capa de acceso a datos               |
| Función principal | Comunicación con la base de datos    |
| CRUD              | Crear, Leer, Actualizar y Borrar     |
| Trabaja junto a   | `@Service`                           |
| Especialidad      | Manejo y persistencia de información |

---

## 🔙 [***Volver***](./definir.md)