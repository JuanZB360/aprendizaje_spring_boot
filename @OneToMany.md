## 🔙 [***Volver***](./entidades.md)

---

# 🔗 Construyendo una relación `@ManyToOne` y `@OneToMany`

## 🎮 El Local de Videojuegos y sus Juegos

En este capítulo aprenderemos cómo construir una relación entre dos entidades utilizando:

* `@ManyToOne`
* `@OneToMany`
* `@JoinColumn`
* `mappedBy`
* `cascade`
* `fetch`
* `orphanRemoval`

Usaremos la siguiente historia.

```text
🎮 Local de Videojuegos

↓

🎮 Juegos
```

Un Local puede tener muchos Juegos.

Pero cada Juego solamente puede pertenecer a un único Local.

---

# 📦 Starter necesario

Para trabajar con entidades y relaciones en Spring Boot necesitas incluir el siguiente starter.

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
```

---

# 🛣️ Primera etapa

# Relación Unidireccional

## El camino de una sola vía

Comenzaremos construyendo una relación sencilla.

En este escenario:

* el Juego conoce al Local;
* pero el Local no conoce a sus Juegos.

Visualmente sería algo así.

```text
🎮 Juego
      │
      ▼
🏢 Local
```

---

# 👶 La Analogía del Sticker

Imagina una tienda de videojuegos.

Cada juego tiene un pequeño sticker pegado en la caja que dice:

```text
"Este juego pertenece al Local del Centro."
```

Entonces ocurre algo muy interesante.

Si tomas un juego cualquiera puedes leer el sticker y descubrir inmediatamente a qué Local pertenece.

Pero...

Si vas al Local y preguntas:

> "¿Qué juegos tienes?"

El Local responderá:

> "No tengo ninguna lista."

Porque nunca le enseñamos cuáles eran sus juegos.

---

# 🧠 La idea importante

En una relación `@ManyToOne`, el objeto que guarda la referencia es el Juego.

Por eso será él quien almacene la llave foránea dentro de la base de datos.

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

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "local_id")
    private Local local;

}
```

---

# 🔍 Analizando el código

## `@ManyToOne`

```java
@ManyToOne
```

Le dice a Hibernate:

> "Muchos Juegos pueden pertenecer al mismo Local."

Visualmente:

```text
🎮 Juego

↓

🏢 Local
```

---

## `@JoinColumn`

```java
@JoinColumn(name = "local_id")
```

Esta anotación tiene dos responsabilidades.

### 1️⃣ Crear la llave foránea

Hibernate agregará una columna llamada:

```text
local_id
```

Dentro de la tabla:

```text
juegos
```

---

### 2️⃣ Convertir al Juego en el dueño de la relación

Como la llave foránea vive dentro de la tabla `juegos`, Hibernate entiende que será el objeto `Juego` quien administrará esa relación.

Por eso decimos que `Juego` es el **Owning Side** (lado propietario).

---

# 🗄️ Resultado en la base de datos

Tabla **locales**

| id | nombre   |
| -- | -------- |
| 1  | GameZone |

Tabla **juegos**

| id | titulo     | local_id |
| -- | ---------- | -------- |
| 1  | FIFA       | 1        |
| 2  | Minecraft  | 1        |
| 3  | Mario Kart | 1        |

Observa que la llave foránea únicamente existe dentro de la tabla `juegos`.

---

# ⚠️ ¿Qué puede hacer el Juego?

Como conoce al Local, podemos escribir:

```java
Juego juego = ...

Local local = juego.getLocal();
```

Pero ocurre un problema.

---

# 🤔 El problema de la relación unidireccional

Supongamos que ahora recuperamos un Local.

```java
Local local = ...
```

Y queremos conocer todos sus Juegos.

Naturalmente intentaríamos hacer esto.

```java
local.getJuegos();
```

Pero aparece un error.

Ese método no existe.

¿Por qué?

Porque el Local nunca aprendió cuáles Juegos le pertenecen.

Solo los Juegos conocen al Local.

---

# 🚗 Una calle de un único sentido

La relación funciona exactamente igual que una calle de una sola vía.

```text
🎮 Juego

↓

🏢 Local
```

Puedes viajar desde el Juego hacia el Local.

Pero nunca desde el Local hacia los Juegos.

---

# 🎯 La solución

Queremos que el Local también conozca todos sus Juegos.

Visualmente la relación quedará así.

```text
          🏢 Local
             ▲
             │
     List<Juego>
             │
             ▼
🎮 Juego ─────────► Local
```

Ahora podremos navegar en ambos sentidos.

```java
juego.getLocal();
```

Y también.

```java
local.getJuegos();
```

Pero aquí aparece una nueva duda.

---

# 🤯 La gran pregunta

Si ahora ambos objetos se conocen...

¿La base de datos guardará dos relaciones?

La respuesta es:

**No.**

Y aquí entra nuevamente el concepto de **Owning Side** y **Inverse Side**.

---

# 🗄️ Java y la Base de Datos son mundos diferentes

En memoria tendremos esto.

```text
JAVA

        Local
           ▲
           │
     List<Juego>
           │
           ▼
Juego ─────────► Local
```

Pero en la base de datos solamente existirá esto.

Tabla **locales**

```text
id
nombre
```

Tabla **juegos**

```text
id
titulo
local_id
```

Observa algo muy importante.

La colección:

```java
List<Juego>
```

No existe en la base de datos.

Solo existe dentro de Java.

Hibernate la construirá automáticamente cuando sea necesaria.

---

# 👑 El Lado Propietario

La llave foránea vive en:

```text
juegos.local_id
```

Por lo tanto el dueño de la relación es:

```text
🎮 Juego
```

Porque es quien puede modificar esa columna.

---

# 🔄 El Lado Inverso

El Local no crea ninguna llave foránea.

Simplemente tendrá una lista para poder navegar hacia sus Juegos.

Por eso utilizaremos:

```java
mappedBy
```

---

# 🏢 Construyendo la entidad `Local`

```java
@Entity
@Table(name = "locales")
public class Local {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String nombre;

    @OneToMany(
            mappedBy = "local",
            cascade = CascadeType.ALL,
            fetch = FetchType.LAZY,
            orphanRemoval = true
    )
    private List<Juego> juegos = new ArrayList<>();

}
```

---

# 🔍 Analizando el código

## `@OneToMany`

```java
@OneToMany
```

Le dice a Hibernate:

> "Este Local puede tener muchos Juegos."

---

## `mappedBy = "local"`

Esta es la parte más importante.

Muchos creen que `"local"` hace referencia a la tabla.

No es así.

Hace referencia al atributo:

```java
private Local local;
```

Que existe dentro de la entidad `Juego`.

Hibernate interpreta esta instrucción así:

> "No crees una nueva relación."

> "La relación ya existe dentro del atributo `local` de la entidad Juego."

Gracias a esto:

* no se crea otra llave foránea;
* no se crea otra tabla;
* no se duplican las relaciones.

---

# 🤖 ¿Cómo construye Hibernate la lista?

Supongamos que haces esto.

```java
Local local = localRepository.findById(1L).get();
```

Y luego.

```java
local.getJuegos();
```

Hibernate ejecutará internamente una consulta muy parecida a esta.

```sql
SELECT *
FROM juegos
WHERE local_id = 1;
```

Obtendrá todos los Juegos cuyo `local_id` sea `1`.

Con ese resultado construirá automáticamente:

```java
List<Juego>
```

Es decir, la lista no estaba almacenada en la base de datos.

Hibernate la creó leyendo todos los registros relacionados.

---

# 🎯 Navegando por la relación

Ahora podemos movernos en ambas direcciones.

Desde un Juego.

```java
Juego juego = ...

Local local = juego.getLocal();
```

Y también desde un Local.

```java
Local local = ...

List<Juego> juegos = local.getJuegos();
```

Todo gracias a la relación bidireccional.

---

# ⚡ Configuraciones adicionales

La relación también incluye tres configuraciones importantes.

## `cascade`

Permite propagar automáticamente operaciones como:

* guardar;
* actualizar;
* eliminar.

Hacia todos los Juegos relacionados.

---

## `fetch`

Controla cuándo Hibernate debe cargar la colección de Juegos.

Con `FetchType.LAZY` los Juegos solamente se consultarán cuando realmente los necesites.

---

## `orphanRemoval`

Si un Juego deja de pertenecer al Local y ya no tiene otro dueño, Hibernate podrá eliminarlo automáticamente.

---

# 🧠 Comparación entre Unidireccional y Bidireccional

## 🛣️ Relación Unidireccional

```text
🎮 Juego

↓

🏢 Local
```

* El Juego conoce al Local.
* El Local no conoce a sus Juegos.
* Más sencilla.
* Consume menos memoria.

---

## 🔄 Relación Bidireccional

```text
          🏢 Local
             ▲
             │
     List<Juego>
             │
             ▼
🎮 Juego ─────────► Local
```

* El Juego conoce al Local.
* El Local conoce todos sus Juegos.
* Permite navegar en ambos sentidos.
* Requiere `mappedBy`.

---

# 📊 Comparación con `@OneToOne`

| Relación                  | ¿Dónde vive la llave foránea?         | Lado Propietario          | Lado Inverso           |
| ------------------------- | ------------------------------------- | ------------------------- | ---------------------- |
| `@OneToOne`               | En la entidad que tiene `@JoinColumn` | Entidad con `@JoinColumn` | Entidad con `mappedBy` |
| `@ManyToOne / @OneToMany` | En la tabla del lado `Many`           | `@ManyToOne`              | `@OneToMany`           |

---

# 🎒 Resumen

| Concepto        | Función                                                                   |
| --------------- | ------------------------------------------------------------------------- |
| `@ManyToOne`    | Muchos registros pertenecen a una sola entidad.                           |
| `@OneToMany`    | Una entidad puede tener muchos registros relacionados.                    |
| `@JoinColumn`   | Crea la llave foránea y convierte a la entidad en el lado propietario.    |
| `mappedBy`      | Indica que la relación ya existe y evita crear una segunda llave foránea. |
| `cascade`       | Propaga operaciones hacia las entidades relacionadas.                     |
| `fetch`         | Decide cuándo cargar la colección.                                        |
| `orphanRemoval` | Elimina automáticamente entidades huérfanas.                              |

---

# 🏆 Conclusión

Una relación `@ManyToOne` / `@OneToMany` sigue exactamente la misma filosofía que aprendimos con `@OneToOne`.

Siempre existe:

* un **lado propietario**, que contiene la llave foránea y administra la relación;
* un **lado inverso**, que simplemente permite navegar en sentido contrario utilizando `mappedBy`.

La diferencia es que ahora un único Local puede estar relacionado con muchos Juegos, mientras que cada Juego solamente puede pertenecer a un único Local.

Una vez comprendas esta estructura, habrás entendido el funcionamiento interno de la mayoría de las relaciones utilizadas en JPA y Hibernate.

---

## 🔙 [***Volver***](./entidades.md)
