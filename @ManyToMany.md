## 🔙 [***Volver***](./entidades.md)

---

# 🔗 Construyendo una relación `@ManyToMany`

## 🎮 Los Clientes y los Juegos

Hasta ahora hemos aprendido dos tipos de relaciones:

* `@OneToOne`
* `@OneToMany` / `@ManyToOne`

En ambas siempre existía una **llave foránea (`Foreign Key`)** dentro de una de las tablas.

Pero ahora aparece un nuevo escenario.

Imagina un Local de Videojuegos donde:

* muchos Clientes pueden jugar muchos Juegos;
* y un mismo Juego puede ser jugado por muchos Clientes.

Visualmente la relación sería así.

```text
Clientes

Juan
María
Luis
Ana

        ↕️

Minecraft
FIFA
Mario Kart
Zelda

Juegos
```

Ningún Cliente está limitado a jugar un solo Juego.

Y ningún Juego está limitado a ser utilizado por un solo Cliente.

---

# 🤔 El problema

Hasta ahora las relaciones eran sencillas.

En `@ManyToOne`, por ejemplo, bastaba con guardar un `local_id` dentro de la tabla `juegos`.

```text
Tabla juegos

id
titulo
local_id
```

Pero ahora...

¿Cómo guardaríamos que:

* Juan juega Minecraft;
* Juan juega FIFA;
* María juega Minecraft;
* Luis juega Mario Kart?

Si intentáramos agregar un solo `cliente_id` dentro de la tabla `juegos`, solamente podríamos guardar un Cliente por Juego.

Y si agregáramos un solo `juego_id` dentro de la tabla `clientes`, solamente podríamos guardar un Juego por Cliente.

Ninguna de las dos soluciones funciona.

---

# 🌉 La solución: Una Tabla Puente

Las bases de datos relacionales resuelven este problema utilizando una **tabla intermedia**.

También se conoce como:

* Tabla puente.
* Tabla de unión.
* Tabla intermedia.
* Join Table.

Esta tabla no almacena información adicional.

Su única responsabilidad es conectar ambas entidades.

Visualmente:

```text
Clientes

Juan
María
Luis

        │
        ▼

Tabla Puente

cliente_id | juego_id

        ▲
        │

Juegos

Minecraft
FIFA
Mario Kart
```

Ahora sí podemos registrar todas las combinaciones posibles.

---

# 👶 La Analogía del Libro de Registro

Imagina que el Local tiene un cuaderno en la recepción.

Cada vez que un Cliente juega un Juego, el encargado escribe una línea.

```text
Juan  → Minecraft

Juan  → FIFA

María → Minecraft

Luis  → Mario Kart
```

Ese cuaderno es exactamente la tabla puente.

No guarda información del Cliente.

No guarda información del Juego.

Solo registra quién jugó qué.

---

# 🗄️ ¿Cómo se vería en SQL?

Tabla **clientes**

| id | nombre |
| -- | ------ |
| 1  | Juan   |
| 2  | María  |

Tabla **juegos**

| id | titulo    |
| -- | --------- |
| 10 | Minecraft |
| 20 | FIFA      |

Tabla **cliente_juego**

| cliente_id | juego_id |
| ---------- | -------- |
| 1          | 10       |
| 1          | 20       |
| 2          | 10       |

Observa algo importante.

No existe ninguna llave foránea en las tablas `clientes` o `juegos`.

Las dos llaves viven dentro de la tabla puente.

---

# 🛣️ Primera etapa

# Relación Unidireccional

Comenzaremos permitiendo que únicamente el Juego conozca a sus Clientes.

Visualmente:

```text
🎮 Juego

↓

👤 Clientes
```

Pero los Clientes todavía no conocerán sus Juegos.

---

# 👶 La Analogía de la Lista de Jugadores

Imagina que detrás de cada Juego hay una hoja pegada.

Por ejemplo.

```text
Minecraft

Jugadores

✔ Juan

✔ María

✔ Luis
```

El Juego sabe quién lo jugó.

Pero si tomamos a Juan y preguntamos:

> "¿Qué Juegos has jugado?"

Juan responderá:

> "No tengo ninguna lista."

---

# 🎮 Construyendo la entidad `Juego`

```java
@Entity
@Table(name = "juegos")
public class Juego {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String titulo;

    @ManyToMany
    @JoinTable(
        name = "cliente_juego",
        joinColumns = @JoinColumn(name = "juego_id"),
        inverseJoinColumns = @JoinColumn(name = "cliente_id")
    )
    private List<Cliente> clientes = new ArrayList<>();

}
```

---

# 🔍 Analizando el código

## `@ManyToMany`

Indica que muchos Juegos pueden relacionarse con muchos Clientes.

---

## `@JoinTable`

```java
@JoinTable(...)
```

Le dice a Hibernate:

> "La relación no utilizará una llave foránea."

> "Utilizará una tabla puente."

---

## `name`

```java
name = "cliente_juego"
```

Es el nombre que tendrá la tabla intermedia.

---

## `joinColumns`

```java
joinColumns = @JoinColumn(name = "juego_id")
```

Representa la llave foránea que apunta al Juego.

---

## `inverseJoinColumns`

```java
inverseJoinColumns = @JoinColumn(name = "cliente_id")
```

Representa la llave foránea que apunta al Cliente.

---

# ⚠️ ¿Quién es el dueño de la relación?

Igual que en los capítulos anteriores, aquí también existe un **Owning Side**.

En este caso será la entidad que contiene:

```java
@JoinTable
```

Es decir:

```text
🎮 Juego
```

Hibernate entiende que será esta entidad quien administrará la tabla puente.

---

# 🤔 El problema de la relación unidireccional

Ahora podemos hacer esto.

```java
Juego juego = ...

List<Cliente> clientes = juego.getClientes();
```

Pero...

Si obtenemos un Cliente.

```java
Cliente cliente = ...
```

Y hacemos.

```java
cliente.getJuegos();
```

El método no existe.

¿Por qué?

Porque todavía no le enseñamos al Cliente cuáles Juegos ha jugado.

---

# 🔄 Segunda etapa

# Relación Bidireccional

Ahora queremos que ambos objetos puedan navegar entre sí.

Visualmente:

```text
           👤 Cliente
               ▲
               │
        List<Juego>
               │
               ▼
Juego ◄────────────► Cliente
```

Ahora podremos escribir.

```java
juego.getClientes();
```

Y también.

```java
cliente.getJuegos();
```

---

# 👤 Construyendo la entidad `Cliente`

```java
@Entity
@Table(name = "clientes")
public class Cliente {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String nombre;

    @ManyToMany(
        mappedBy = "clientes",
        fetch = FetchType.LAZY
    )
    private List<Juego> juegos = new ArrayList<>();

}
```

---

# 🔍 ¿Qué hace `mappedBy`?

Observa esta línea.

```java
mappedBy = "clientes"
```

Muchos creen que `"clientes"` hace referencia a la tabla.

No.

Hace referencia al atributo que existe dentro de la entidad `Juego`.

```java
private List<Cliente> clientes;
```

Hibernate interpreta esta instrucción así.

> "La relación ya existe."

> "Ya hay una tabla puente."

> "Simplemente utiliza esa relación para construir esta colección."

Gracias a `mappedBy`:

* no se crea una segunda tabla puente;
* no se duplican las relaciones;
* ambas entidades quedan sincronizadas.

---

# 🤖 ¿Cómo construye Hibernate la lista?

Supongamos que recuperamos un Cliente.

```java
Cliente cliente = clienteRepository.findById(1L).get();
```

Después hacemos.

```java
cliente.getJuegos();
```

Hibernate consulta automáticamente la tabla puente.

```sql
SELECT juego.*
FROM juegos juego
INNER JOIN cliente_juego cj
ON juego.id = cj.juego_id
WHERE cj.cliente_id = 1;
```

Con ese resultado construye:

```java
List<Juego>
```

La colección no estaba almacenada en memoria.

Hibernate la creó leyendo la tabla puente.

---

# ⚡ Buenas prácticas en `@ManyToMany`

## 🦥 Utiliza `FetchType.LAZY`

Las colecciones `ManyToMany` pueden crecer muchísimo.

Si utilizaras `FetchType.EAGER`, Hibernate cargaría todos los registros relacionados inmediatamente.

Eso puede generar:

* muchas consultas SQL;
* mayor consumo de memoria;
* peor rendimiento.

Por eso la recomendación general es utilizar:

```java
fetch = FetchType.LAZY
```

---

## 🚫 Evita `CascadeType.REMOVE`

Imagina que eliminas un Cliente.

```java
clienteRepository.delete(cliente);
```

Si utilizaras:

```java
CascadeType.REMOVE
```

Hibernate podría eliminar también los Juegos relacionados.

Sería un desastre, porque esos Juegos probablemente también pertenezcan a otros Clientes.

En una relación `@ManyToMany` normalmente solo se elimina el registro correspondiente de la tabla puente, no las entidades relacionadas.

Por esta razón, en la mayoría de los casos se evita utilizar:

* `CascadeType.REMOVE`
* `CascadeType.ALL`

---

# 📊 Comparación de las relaciones aprendidas

| Relación      | ¿Dónde vive la relación? | Lado propietario          |
| ------------- | ------------------------ | ------------------------- |
| `@OneToOne`   | Llave foránea            | Entidad con `@JoinColumn` |
| `@ManyToOne`  | Llave foránea            | Entidad `@ManyToOne`      |
| `@ManyToMany` | Tabla puente             | Entidad con `@JoinTable`  |

---

# 🎒 Resumen

| Concepto             | Función                                                                  |
| -------------------- | ------------------------------------------------------------------------ |
| `@ManyToMany`        | Permite que muchas entidades se relacionen con muchas otras.             |
| `@JoinTable`         | Crea la tabla puente que conecta ambas entidades.                        |
| `joinColumns`        | Define la llave foránea de la entidad propietaria.                       |
| `inverseJoinColumns` | Define la llave foránea de la entidad relacionada.                       |
| `mappedBy`           | Indica que la relación ya existe y evita crear una segunda tabla puente. |
| `FetchType.LAZY`     | Carga la colección únicamente cuando se necesita.                        |

---

# 🏆 Conclusión

La relación `@ManyToMany` es diferente a las anteriores porque **no utiliza una única llave foránea**.

En su lugar, Hibernate crea una **tabla puente** que almacena las conexiones entre ambas entidades.

Al igual que en las demás relaciones, siempre existe:

* un **lado propietario**, que administra la relación mediante `@JoinTable`;
* un **lado inverso**, que utiliza `mappedBy` para navegar sin crear una segunda relación.

Comprender este patrón es fundamental para dominar JPA, ya que todas las relaciones siguen la misma filosofía: siempre hay una entidad que administra la relación y otra que simplemente la representa.

---

## 🔙 [***Volver***](./entidades.md)
