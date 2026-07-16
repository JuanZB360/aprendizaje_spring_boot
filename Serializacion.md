## 🔙 [***Volver***](./entidades.md)

---

# 🎮 JPA para Niños: El Misterio del Bucle Infinito

> **Aprende a utilizar `@JsonManagedReference` y `@JsonBackReference` para evitar ciclos infinitos al serializar entidades JPA con Spring Boot.**

Cuando trabajamos con **Spring Boot**, **JPA/Hibernate** y **Jackson**, es muy común crear relaciones entre entidades, como **Cliente → Pedido** o **Usuario → Roles**.

Estas relaciones funcionan perfectamente dentro de Java, pero cuando Spring intenta convertir esos objetos en **JSON** para enviarlos a un navegador o una aplicación móvil, puede aparecer uno de los errores más comunes para quienes comienzan con JPA:

> **`StackOverflowError`** (desbordamiento de pila por recursión infinita).

En esta guía aprenderás por qué ocurre este problema y cómo solucionarlo de forma sencilla utilizando `@JsonManagedReference` y `@JsonBackReference`.

---

# 🎯 ¿Qué es la Serialización?

Antes de entender el problema, necesitamos conocer un concepto importante.

## 📖 Definición

La **serialización** es el proceso de convertir un objeto Java en otro formato que pueda ser enviado o almacenado.

Generalmente, en las APIs REST, ese formato es **JSON**.

Por ejemplo, este objeto Java:

```java
Cliente cliente = new Cliente(1L, "Carlos");
```

Puede convertirse automáticamente en:

```json
{
    "id": 1,
    "nombre": "Carlos"
}
```

En Spring Boot, quien realiza esta conversión es la librería **Jackson**.

---

# 🎮 La Analogía del Local de Videojuegos

Imaginemos un local donde las personas alquilan consolas para jugar.

Tenemos dos entidades principales:

* 👤 **Cliente**
* 🎟️ **Pedido**

Un cliente puede realizar muchos pedidos.

Cada pedido pertenece únicamente a un cliente.

Visualmente sería algo así:

```text
Cliente
   │
   ├── Pedido 1
   ├── Pedido 2
   ├── Pedido 3
```

---

## 👤 El Cliente (Entidad Padre)

Representa al jugador registrado en el local.

Ejemplo:

* Carlos
* Juan
* Laura

Un cliente puede existir aunque todavía no haya realizado ningún pedido.

Por esta razón, normalmente se considera la **entidad padre**.

---

## 🎟️ El Pedido (Entidad Hija)

Representa un turno de juego.

Ejemplo:

* Xbox Series X por 2 horas
* PlayStation 5 por 1 hora

Un pedido no tiene sentido sin un cliente asociado.

Por eso se considera la **entidad hija**.

---

# 🔄 La Relación Bidireccional

En JPA es muy común crear una relación donde ambas entidades se conocen entre sí.

```text
Cliente
    │
    └──────────────► Lista de Pedidos

Pedido
    │
    └──────────────► Cliente
```

Es decir:

* El cliente conoce todos sus pedidos.
* Cada pedido conoce quién es su cliente.

En Java esto funciona perfectamente.

El problema aparece cuando Jackson intenta convertir estos objetos a JSON.

---

# 🚨 El Problema: El Bucle Infinito

Supongamos que solicitamos este endpoint:

```http
GET /clientes/1
```

Jackson comienza a construir el JSON.

## Paso 1

Pregunta al cliente:

> "¿Qué información tienes?"

El cliente responde:

* id
* nombre
* lista de pedidos

Entonces Jackson entra a la lista de pedidos.

---

## Paso 2

Ahora Jackson analiza el primer pedido.

Pregunta:

> "¿Qué información tienes?"

El pedido responde:

* id
* consola
* cliente

Entonces Jackson vuelve al cliente.

---

## Paso 3

El cliente vuelve a responder:

* id
* nombre
* pedidos

Jackson entra nuevamente a los pedidos...

Después vuelve al cliente...

Después otra vez a los pedidos...

Y así sucesivamente.

```text
Cliente
   ↓
Pedido
   ↓
Cliente
   ↓
Pedido
   ↓
Cliente
   ↓
Pedido
   ↓
∞
```

Nunca encuentra un punto donde detenerse.

Finalmente ocurre:

```text
StackOverflowError
```

Porque la recursión nunca termina.

---

# 🚦 La Solución: Las Señales de Tránsito

Jackson necesita saber por dónde puede avanzar y dónde debe detenerse.

Para eso existen dos anotaciones complementarias.

## ✅ `@JsonManagedReference`

Se coloca en la entidad **Padre**.

Le indica a Jackson:

> "Puedes recorrer esta relación y mostrar su contenido."

Es la referencia principal que sí aparecerá en el JSON.

---

## 🚫 `@JsonBackReference`

Se coloca en la entidad **Hija**.

Le indica a Jackson:

> "No vuelvas hacia atrás."

Cuando Jackson llega a esta propiedad deja de recorrer la relación, evitando el ciclo infinito.

---

# 💻 Paso 1: Configurar la Entidad Padre

```java
import com.fasterxml.jackson.annotation.JsonManagedReference;
import jakarta.persistence.*;
import java.util.ArrayList;
import java.util.List;

@Entity
@Table(name = "clientes")
public class Cliente {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String nombre;

    @OneToMany(
            mappedBy = "cliente",
            cascade = CascadeType.ALL
    )
    @JsonManagedReference("cliente-pedidos")
    private List<Pedido> pedidos = new ArrayList<>();

}
```

## ¿Qué significa cada parte?

| Código                  | Función                                                                         |
| ----------------------- | ------------------------------------------------------------------------------- |
| `mappedBy = "cliente"`  | Indica que la relación está administrada por la entidad `Pedido`.               |
| `CascadeType.ALL`       | Las operaciones realizadas sobre el cliente también se aplicarán a sus pedidos. |
| `@JsonManagedReference` | Permite que la lista de pedidos sea incluida en el JSON.                        |

---

# 💻 Paso 2: Configurar la Entidad Hija

```java
import com.fasterxml.jackson.annotation.JsonBackReference;
import jakarta.persistence.*;

@Entity
@Table(name = "pedidos")
public class Pedido {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String consolaAsignada;

    @ManyToOne
    @JoinColumn(name = "cliente_id")
    @JsonBackReference("cliente-pedidos")
    private Cliente cliente;

}
```

## ¿Qué significa cada parte?

| Código               | Función                                                        |
| -------------------- | -------------------------------------------------------------- |
| `@ManyToOne`         | Muchos pedidos pueden pertenecer a un solo cliente.            |
| `@JoinColumn`        | Define la llave foránea en la base de datos.                   |
| `@JsonBackReference` | Evita regresar nuevamente al cliente durante la serialización. |

---

# 🌉 ¿Por qué ambas anotaciones tienen el mismo nombre?

```java
@JsonManagedReference("cliente-pedidos")
```

```java
@JsonBackReference("cliente-pedidos")
```

Ese texto funciona como un **identificador de la relación**.

Puedes imaginarlo como el nombre de un puente que conecta ambas entidades.

Jackson utiliza ese nombre para saber qué `@JsonBackReference` pertenece a qué `@JsonManagedReference`.

Si una entidad tiene varias relaciones bidireccionales, cada una debe tener un nombre diferente.

Por ejemplo:

```java
@JsonManagedReference("cliente-pedidos")
@JsonManagedReference("cliente-direcciones")
@JsonManagedReference("cliente-facturas")
```

Y cada relación tendrá su correspondiente `@JsonBackReference` con el mismo identificador.

---

# 🌟 Resultado Final

Cuando consultamos un cliente:

```http
GET /clientes/1
```

Obtendremos un JSON limpio:

```json
{
  "id": 1,
  "nombre": "Carlos",
  "pedidos": [
    {
      "id": 101,
      "consolaAsignada": "PlayStation 5"
    },
    {
      "id": 102,
      "consolaAsignada": "Nintendo Switch"
    }
  ]
}
```

Observa que dentro de cada pedido **ya no aparece nuevamente el cliente**, evitando así el ciclo infinito.

---

# ⚠️ ¿Siempre debo usar estas anotaciones?

No necesariamente.

En proyectos pequeños funcionan muy bien y son fáciles de entender.

Sin embargo, en aplicaciones grandes es muy común utilizar **DTOs (Data Transfer Objects)** para separar las entidades de la información que se envía al cliente.

Los DTOs ofrecen mayor flexibilidad, mejor rendimiento y evitan exponer información sensible de las entidades.

Por esta razón, muchos proyectos profesionales utilizan DTOs junto con herramientas como **MapStruct** para realizar la conversión entre entidades y objetos de respuesta.

---

# 🎒 Resumen para llevar en tu mochila de Senior

| Concepto                                 | Descripción                                                                     |
| ---------------------------------------- | ------------------------------------------------------------------------------- |
| `@JsonManagedReference`                  | Marca el lado de la relación que sí debe serializarse.                          |
| `@JsonBackReference`                     | Marca el lado donde Jackson debe detener la serialización.                      |
| Ambos deben tener el mismo identificador | Permite que Jackson relacione correctamente ambas anotaciones.                  |
| Problema que resuelven                   | Evitan el `StackOverflowError` causado por relaciones bidireccionales.          |
| Alternativa profesional                  | Utilizar DTOs para controlar exactamente qué información se expone en las APIs. |

---

# 🏆 Conclusión

Las relaciones bidireccionales son una característica muy poderosa de JPA, pero también introducen uno de los errores más frecuentes cuando trabajamos con APIs REST.

Las anotaciones `@JsonManagedReference` y `@JsonBackReference` permiten indicarle a Jackson cuál es el camino correcto para recorrer las entidades sin caer en una recursión infinita.

Una vez comprendas este concepto, te resultará mucho más sencillo trabajar con relaciones como:

* Cliente → Pedidos
* Usuario → Roles
* Categoría → Productos
* Orden → Detalles
* Empresa → Empleados

Y, más adelante, descubrirás que en aplicaciones profesionales este problema suele resolverse mediante el uso de **DTOs**, una práctica ampliamente utilizada para construir APIs más seguras, mantenibles y escalables.

---

## 🔙 [***Volver***](./entidades.md)