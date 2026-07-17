## 🔙 [***Volver***](./entidades.md)

---

# ⚡ `fetch`: ¿Cuándo debe Hibernate traer los datos?

Hasta ahora ya sabemos crear una relación entre entidades.

Nuestra clase `Local` quedó así:

```java
@OneToOne(
    fetch = FetchType.LAZY
)
@JoinColumn(name = "gerente_id")
private Gerente gerente;
```

Ahora aparece una nueva pregunta.

> 🤔 ¿Qué significa `fetch`?

La respuesta corta es:

> **Le indica a Hibernate cuándo debe cargar una entidad relacionada desde la base de datos.**

Pero para comprenderlo realmente, primero debemos entender cómo trabaja Hibernate internamente.

---

# 🎮 La Analogía del Local de Videojuegos

Imagina que tienes un sistema para administrar una cadena de locales de videojuegos.

Cada Local tiene:

* un Gerente,
* un Inventario,
* una Lista de Empleados,
* un Historial de Ventas,
* una Lista de Consolas,
* una Lista de Clientes.

Visualmente:

```text
🎮 Local

├── 👨‍💼 Gerente
├── 👨‍🔧 Empleados
├── 🎮 Consolas
├── 📦 Inventario
├── 💰 Ventas
└── 👥 Clientes
```

Ahora imagina que solamente quieres mostrar esto:

```text
Nombre del Local

GameZone
```

¿Tiene sentido que Hibernate cargue también:

* todos los empleados,
* todas las ventas,
* todos los clientes,
* todo el inventario,
* todas las consolas?

Claramente no.

Sería muchísimo trabajo innecesario.

Aquí es donde aparece `fetch`.

---

# 📦 ¿Qué significa "cargar"?

Cuando hablamos de cargar una entidad queremos decir que Hibernate realiza una consulta SQL para obtener esa información desde la base de datos.

Por ejemplo.

```java
Local local = localRepository.findById(1L).get();
```

Hibernate consulta:

```sql
SELECT *
FROM locales
WHERE id = 1;
```

Hasta aquí solamente conocemos el Local.

La pregunta es:

> ¿También debe traer automáticamente el Gerente?

Existen dos respuestas posibles.

---

# 🦥 Primera opción: `FetchType.LAZY`

```java
@OneToOne(fetch = FetchType.LAZY)
```

La palabra **Lazy** significa:

> Perezoso.

Hibernate interpreta esta configuración así:

> "No cargues el Gerente todavía."

> "Espera hasta que realmente alguien lo necesite."

Por eso primero consulta únicamente el Local.

```sql
SELECT *
FROM locales
WHERE id = 1;
```

Y se detiene.

---

# 🧠 ¿Qué ocurre si nunca utilizo el gerente?

Supongamos este código.

```java
Local local = localRepository.findById(1L).get();

System.out.println(local.getNombreLocal());
```

Observa algo interesante.

Nunca hicimos esto.

```java
local.getGerente();
```

Entonces Hibernate piensa:

> "Perfecto."

> "Nunca necesitaron el gerente."

Por lo tanto **jamás realiza otra consulta**.

Solo ejecutó una.

```sql
SELECT *
FROM locales;
```

Nada más.

---

# 🔍 ¿Y cuándo consulta el gerente?

En el momento exacto en que lo necesites.

```java
Local local = localRepository.findById(1L).get();

Gerente gerente = local.getGerente();
```

Ahora Hibernate detecta que realmente quieres acceder al gerente.

Entonces ejecuta automáticamente una segunda consulta.

```sql
SELECT *
FROM gerentes
WHERE id = ?;
```

Eso ocurre únicamente cuando el dato es utilizado.

Por eso se llama:

> **Carga Perezosa (Lazy Loading).**

---

# 🚀 Segunda opción: `FetchType.EAGER`

Ahora cambiemos la configuración.

```java
@OneToOne(fetch = FetchType.EAGER)
```

La palabra **Eager** significa:

> Ansioso.

Hibernate interpreta esta configuración así:

> "Trae inmediatamente toda la información relacionada."

Cuando consultas un Local:

```java
Local local = localRepository.findById(1L).get();
```

Hibernate no espera.

Inmediatamente obtiene también el Gerente.

```text
Consulta Local

↓

Consulta Gerente
```

Aunque nunca llegues a utilizar el gerente.

---

# 📊 Comparando ambos comportamientos

## `FetchType.LAZY`

```text
Consultar Local

↓

Solo se obtiene el Local

↓

¿Necesitas el Gerente?

      │

      ├── Sí → Consulta Gerente

      └── No → Fin
```

---

## `FetchType.EAGER`

```text
Consultar Local

↓

Consulta Local

↓

Consulta Gerente

↓

Entrega ambos objetos
```

Siempre consulta ambas entidades.

---

# 🎯 ¿Cuál consume menos recursos?

Supongamos que tienes un Local con:

* 40 empleados,
* 600 ventas,
* 2.000 clientes,
* 300 consolas.

Si todas las relaciones fueran `EAGER`, cada vez que consultaras un Local Hibernate también cargaría miles de registros relacionados.

Aunque únicamente quisieras mostrar el nombre del Local.

Eso significa:

* Más consultas SQL.
* Más memoria.
* Más tiempo de respuesta.

Por eso en aplicaciones grandes se intenta utilizar `LAZY` siempre que sea posible.

---

# 🤖 ¿Cómo logra Hibernate hacer esto?

Aquí ocurre una de las cosas más interesantes de Hibernate.

Cuando usas `LAZY`, Hibernate **no coloca inmediatamente un objeto Gerente**.

En su lugar crea un objeto especial llamado **Proxy**.

Puedes imaginarlo como un sustituto temporal.

```text
Local

↓

Proxy de Gerente
```

Ese Proxy todavía no contiene los datos reales.

Solo sabe:

* quién es el gerente,
* cómo buscarlo,
* cuándo debe consultarlo.

Cuando haces:

```java
local.getGerente();
```

El Proxy despierta.

Entonces Hibernate consulta la base de datos y reemplaza el Proxy por el objeto real.

Todo ocurre automáticamente.

---

# ⚠️ El error más famoso: `LazyInitializationException`

Ahora imaginemos este escenario.

```java
Local local = localRepository.findById(1L).get();
```

La transacción termina.

Hibernate ya cerró la conexión con la base de datos.

Después haces esto.

```java
local.getGerente().getNombre();
```

Pero el gerente nunca había sido cargado.

Hibernate intenta consultarlo.

Sin embargo...

La conexión ya está cerrada.

Resultado:

```text
LazyInitializationException
```

Este es uno de los errores más comunes cuando se comienza a trabajar con JPA.

---

# 💡 ¿Cómo evitar este error?

Existen varias soluciones.

Las más utilizadas son:

* Acceder a la relación dentro de una transacción activa.
* Utilizar consultas con `JOIN FETCH`.
* Utilizar DTOs para cargar únicamente la información necesaria.
* Diseñar correctamente la capa de servicio.

En aplicaciones profesionales, la combinación de **DTOs + consultas optimizadas** suele ser la estrategia más utilizada.

---

# 🧠 ¿Entonces siempre debo usar `LAZY`?

No existe una respuesta única.

Depende del caso de uso.

Pero como regla general:

* Utiliza `LAZY` cuando la información relacionada no siempre sea necesaria.
* Utiliza `EAGER` únicamente cuando estés seguro de que esa relación siempre será utilizada.

Muchos proyectos modernos prefieren comenzar con `LAZY` y cargar explícitamente las relaciones cuando realmente las necesitan.

---

# 🎒 Resumen

| Tipo              | Comportamiento                                                   |
| ----------------- | ---------------------------------------------------------------- |
| `FetchType.LAZY`  | Carga la relación únicamente cuando se accede a ella.            |
| `FetchType.EAGER` | Carga la relación inmediatamente junto con la entidad principal. |

---

# 🏆 Conclusión

`fetch` no crea relaciones ni modifica la base de datos.

Su única responsabilidad es decidir **cuándo Hibernate debe consultar la información relacionada**.

Podemos resumirlo de esta forma:

```text
LAZY

"Espera hasta que realmente necesiten los datos."


EAGER

"Trae todo desde el principio."
```

Elegir correctamente entre `LAZY` y `EAGER` puede mejorar significativamente el rendimiento de una aplicación, reducir el consumo de memoria y evitar consultas innecesarias a la base de datos.

---

## 🔙 [***Volver***](./entidades.md)