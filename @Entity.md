## 🔙 [***Volver***](./definir.md)
<a href="javascript:history.back()">## 🔙 [***Volver***]</a>

---

# `@Entity`

## 📄 El modelo de datos

Como vimos antes, `@Entity` define qué información queremos que sea persistente  
(es decir, que no se borre al apagar la aplicación).

---

# ❓ ¿Qué hace?

Mapea una clase Java a una tabla de la base de datos.

En otras palabras:

> Cada objeto creado a partir de esta clase puede convertirse en un registro dentro de una tabla.

---

# 🕒 ¿Cuándo usarla?

Se utiliza en las clases que representan los objetos reales del sistema.

## 📌 Ejemplos comunes

- 👤 Usuario
- 📦 Producto
- 🧾 Venta
- 🧑 Cliente
- 🧮 Factura

---

# 🎭 Personalidad

Es el **Formulario**.

Define:

- qué datos existen,
- cómo se organizan,
- qué información será almacenada en la base de datos.

---

# 🧠 Idea clave

Una clase con `@Entity` normalmente representa una tabla.

Por ejemplo:

```text
Clase Java        →        Tabla SQL
-------------------------------------
Usuario           →        usuarios
Producto          →        productos
Factura           →        facturas
```

***Nota:*** por convencion las clases con `@Entity` se nombran en singular y en base de datos en plural. 

---

### ⚙️ ¿Qué suele contener una @Entity?

Normalmente incluye:

- atributos,
- identificadores (id),
- relaciones y datos persistentes.

---

Ejemplo conceptual

```java
@Entity
public class Usuario {

    private Long id;
    private String nombre;
    private String correo;

}
```

---

## 📌 Resumen rápido

| Concepto  | Significado                                       |
| --------- | ------------------------------------------------- |
| `@Entity` | Convierte una clase en una tabla de base de datos |
| Objeto    | Representa una fila                               |
| Atributos | Representan columnas                              |
| Instancia | Representa un registro                            |

---

## 🔙 [***Volver***](./definir.md)