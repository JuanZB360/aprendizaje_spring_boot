## 🔙 [***Volver***](./Repositorio.md)
---

# 🎯 Sección 3: El Intérprete Personalizado

## JPQL (Java Persistence Query Language)

A veces, las palabras mágicas de los Metodos de Consulta se quedan cortas.

Imagina que quieres buscar un juego basado en el nombre del dueño del local, ordenado por el precio de los dulces que vende...

¡El nombre del método sería de tres kilómetros de largo!

Para evitar nombres de métodos monstruosos, usamos **JPQL**.

---

# 🧠 ¿Qué es JPQL?

JPQL es un idioma especial muy parecido al SQL que ya conoces, pero con un secreto de alta tecnología:

No le habla a las tablas aburridas de la base de datos.

¡Le habla directamente a tus clases y objetos de Java!

---

## 👶 Explicación para niños

Imagina que nuestro Recepcionista ya no solo escucha palabras sueltas.

Ahora tú te sientas con él en el mostrador, sacas una hoja de papel en blanco y le escribes una **Carta de Instrucciones Especiales** (una receta personalizada).

En la carta no hablas de cemento ni de ladrillos (tablas SQL).

Hablas de los juguetes mismos (las clases Java).

Le dices:

> "Querido Recepcionista, ve al estante de los objetos Juego, busca el juguete cuyo botón titulo sea igual al nombre que te dejaré anotado en este papel."

El recepcionista lee tu carta, la traduce en su mente y va directo al grano.

---

# 🛠️ ¿Cómo se escribe una consulta JPQL?

Para escribir estas cartas de instrucciones usamos una anotación llamada:

`@Query`

La colocamos encima de los métodos de nuestro repositorio.

---

## 📦 Ejemplo completo

```java
@Repository
public interface JuegoRepository extends JpaRepository<Juego, Long> {

        // Nivel 1: La carta básica (Buscar por un atributo)
        @Query("SELECT j FROM Juego j WHERE j.titulo = :tituloRecibido")
        List<Juego> buscarPorSuTituloPersonalizado(@Param("tituloRecibido") String titulo);


        // Nivel 2: Cruzando puentes (Buscar un juego usando datos del Local)
        @Query("SELECT j FROM Juego j WHERE j.local.nombreLocal = :nombreDelLocal")
        List<Juego> buscarJuegosPorNombreDeSuLocal(@Param("nombreDelLocal") String nombre);


        // Nivel 3: El reporte Senior (Traer solo los nombres de los juegos caros)
        @Query("SELECT j.titulo FROM Juego j WHERE j.local.id = :localId AND j.titulo LIKE %:palabra%")
        List<String> buscarNombresDeJuegosPorLocalYPalabra(
        @Param("localId") Long id,
        @Param("palabra") String palabra
        );

}
```

---

# 🎮 Nivel 1: La Carta Básica

Buscar por un atributo específico.

```java
@Query("SELECT j FROM Juego j WHERE j.titulo = :tituloRecibido")
List<Juego> buscarPorSuTituloPersonalizado(
        @Param("tituloRecibido") String titulo
);
```

### 👶 Mente de niño

Le entregas al recepcionista un papel que dice:

> "Tráeme el juego cuyo título sea Mario Kart."

Y él va exactamente por ese juego.

---

# 🌉 Nivel 2: Cruzando Puentes

Buscar usando información de una relación.

```java
@Query("SELECT j FROM Juego j WHERE j.local.nombreLocal = :nombreDelLocal")
List<Juego> buscarJuegosPorNombreDeSuLocal(
        @Param("nombreDelLocal") String nombre
);
```

### 👶 Mente de niño

Ahora el recepcionista puede mirar más allá del juego.

Puede ver en qué local está guardado.

Le dices:

> "Muéstrame todos los juegos que pertenezcan al Local Centro."

Y él busca usando información del Local.

---

# 🚀 Nivel 3: El Reporte Senior

Traer únicamente los datos que realmente necesitas.

```java
@Query("SELECT j.titulo FROM Juego j WHERE j.local.id = :localId AND j.titulo LIKE %:palabra%")
List<String> buscarNombresDeJuegosPorLocalYPalabra(
        @Param("localId") Long id,
        @Param("palabra") String palabra
);
```

### 👶 Mente de niño

Aquí no pedimos el juguete completo.

Solo queremos leer la etiqueta con su nombre.

Es más rápido y consume menos recursos.

---

# 🔍 Desarmando el Juguete

Vamos a analizar esta consulta:

```java
SELECT j FROM Juego j WHERE j.titulo = :tituloRecibido
```

---

## FROM Juego j

### La clase, no la tabla

```sql
FROM Juego j
```

⚠️ Atención aquí.

No escribimos el nombre de la tabla SQL.

No escribimos:

```sql
FROM juegos
```

Escribimos:

```sql
FROM Juego
```

Porque JPQL trabaja sobre la clase Java.

La letra `j` es simplemente un apodo (alias) para no escribir la palabra completa todo el tiempo.

---

## SELECT j

### Tráeme el objeto completo

```sql
SELECT j
```

Le estamos diciendo:

> "Tráeme el objeto Juego completo."

Incluyendo:

* id
* título
* precio
* relaciones
* y demás atributos

---

## j.titulo

### Propiedades Java

```sql
j.titulo
```

No estamos apuntando a una columna SQL.

Estamos apuntando al atributo:

```java
private String titulo;
```

de nuestra clase Java.

---

## :tituloRecibido

### El parámetro dinámico

```java
:tituloRecibido
```

Los dos puntos (`:`) indican que el valor será reemplazado en tiempo de ejecución.

Spring tomará el valor recibido en el método y lo insertará en la consulta.

Esto funciona gracias a:

```java
@Param("tituloRecibido")
```

---

# 🧩 El Traductor @Param

JPQL necesita saber qué variable Java corresponde a cada parámetro dentro de la consulta.

Por eso usamos:

```java
@Param("tituloRecibido")
```

### 👶 Mente de niño

Es como poner una etiqueta en un sobre.

JPQL ve:

```java
:tituloRecibido
```

y pregunta:

> "¿Quién tiene este nombre?"

Entonces `@Param` levanta la mano y dice:

> "Yo tengo ese valor."

---

# 👨‍💻 ¿Por qué un desarrollador Senior ama JPQL?

---

## 🛡️ Es independiente de la base de datos

Hoy puedes usar:

* MySQL
* PostgreSQL
* Oracle
* SQL Server

Y tu consulta seguirá funcionando.

Spring Data JPA se encarga de traducirla automáticamente.

---

## ⚡ Tiene autocompletado

Como trabajas con clases Java reales:

* IntelliJ
* Spring Tool Suite
* VS Code

pueden ayudarte mientras escribes.

Si te equivocas en un atributo, el IDE puede detectarlo antes de ejecutar la aplicación.

---

## 🧹 Es más fácil de mantener

Las consultas quedan relacionadas directamente con tus entidades.

Cuando lees el código entiendes inmediatamente qué objetos participan en la búsqueda.

---
## 🔙 [***Volver***](./Repositorio.md)