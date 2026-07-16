## 🔙 [***Volver***](./persistencia.md)

---

Continuando con la tematica del local de Video Juegos comenzaremso una nueva capa El Repositorio.

El repositorio es como el Recepcionista del Local de Videojuegos. Él está sentado en el mostrador, tiene las llaves de los estantes y sabe exactamente cómo buscar lo que le pidas.

- [***Creacion de Repositorio***](./creacionRepo.md)
- [***Consultas CRUD simples***](./consultasSimples.md)
- [***Metodos de Consulta***](./QueryMethods.md)
- [***JPQL***](./JPQL.md)

---

# 🎒 El Mapa Completo de los Repositorios

Después de todo lo aprendido, ya tienes tres niveles de poder dentro de Spring Data JPA.

---

## 🏭 Sección 1

### El Manual de Fábrica

Usas los métodos que ya vienen listos.

Ejemplos:

* `save()`
* `findById()`
* `findAll()`
* `deleteById()`

---

## 🎩 Sección 2

### Las Palabras Mágicas

Creas consultas simplemente nombrando métodos.

Ejemplos:

* `findByTitulo()`
* `findByTituloContaining()`
* `countByConsola()`
* `existsByTitulo()`

---

## 🚀 Sección 3

### Las Cartas Personalizadas

Tomas el control total usando:

```java
@Query(...)
```

Ejemplo:

```java
@Query("SELECT j FROM Juego j WHERE j.titulo = :tituloRecibido")
```

Ahora ya no dependes únicamente de las palabras mágicas.

Puedes escribir exactamente la consulta que necesites utilizando tus clases Java como referencia.

---

# 💡 Resumen para tu mochila

Cuando los métodos heredados no son suficientes y las palabras mágicas empiezan a volverse gigantes o difíciles de mantener, JPQL aparece como la solución ideal.

Con `@Query` puedes escribir consultas personalizadas utilizando el lenguaje de tus entidades Java, manteniendo el código limpio, portable y fácil de entender.

JPQL es el punto donde Spring Data JPA deja de ser magia y empieza a convertirse en una herramienta profesional de control total.


---

## 🔙 [***Volver***](./persistencia.md)