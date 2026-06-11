## 🔙 [***Volver***](./Repositorio.md)

---

# 🎩 Sección 2: El Recepcionista Inteligente

## Query Methods (Consultas por nombre de método)

Aquí ocurre uno de los trucos más espectaculares de Spring Data JPA.

Podemos crear consultas nuevas sin escribir SQL.

Solo debemos darle el nombre correcto al método y Spring Boot se encargará de construir la consulta automáticamente.

---

## 👶 Explicación para niños

Imagina que nuestro recepcionista tiene un superpoder especial.

Él entiende órdenes habladas.

No necesitas explicarle paso a paso cómo buscar algo.

Simplemente le dices:

* "Busca un juego por título."
* "Cuenta cuántos juegos hay."
* "Dime si existe."
* "Borra los juegos dañados."

Y él sabe exactamente qué hacer.

---

# 🛠️ El Catálogo Completo de Superpoderes

Dentro de nuestro `JuegoRepository` podemos declarar todos los poderes que tendrá el recepcionista.

`java
@Repository
public interface JuegoRepository extends JpaRepository<Juego, Long> {

```
// ==========================================
// 1. BUSCAR
// ==========================================

Optional<Juego> findByTitulo(String titulo);

List<Juego> findByTituloContaining(String palabraClave);

List<Juego> findByTituloAndLocalId(String titulo, Long localId);


// ==========================================
// 2. CONTAR
// ==========================================

long countByConsola(String consola);


// ==========================================
// 3. VERIFICAR EXISTENCIA
// ==========================================

boolean existsByTitulo(String titulo);


// ==========================================
// 4. BORRAR
// ==========================================

@Transactional
void deleteByDisponibleFalse();
```

}
`

---

# 🔍 Los Verbos Mágicos de Spring Data JPA

Spring reconoce ciertas palabras al inicio del método.

Cada palabra representa una acción diferente.

---

## 🔎 findBy

### El buscador

Sirve para traer registros desde la base de datos.

### Ejemplos

`java
Optional<Juego> findByTitulo(String titulo);

List<Juego> findByTituloContaining(String palabraClave);
`

### 👶 Mente de niño

Le dices al recepcionista:

"Busca el juego llamado Mario Kart."

o

"Busca todos los juegos que tengan la palabra Craft."

Y él corre al estante y te los trae.

---

## 🔢 countBy

### El contador

Sirve para contar registros sin traerlos completos.

### Ejemplo

`java
long countByConsola(String consola);
`

### 👶 Mente de niño

Le preguntas:

"¿Cuántos juegos de PS5 tenemos?"

El recepcionista no carga todas las cajas.

Solo las cuenta y responde:

"Tenemos 14."

---

## 🕵️ existsBy

### El detective

Sirve para verificar si algo existe.

### Ejemplo

`java
boolean existsByTitulo(String titulo);
`

### 👶 Mente de niño

Le preguntas:

"¿Existe el juego FIFA 2026?"

El recepcionista mira rápidamente el estante.

* Si lo encuentra → `true`
* Si no lo encuentra → `false`

No necesita traer el juego completo.

---

## 🗑️ deleteBy

### El borrador

Sirve para eliminar registros.

### Ejemplo

`java
@Transactional
void deleteByDisponibleFalse();
`

### 👶 Mente de niño

El recepcionista se pone los guantes de limpieza.

Busca todos los juegos dañados o no disponibles.

Y los elimina de las vitrinas.

---

## ⚠️ ¿Por qué usamos `@Transactional`?

Porque borrar información es una operación delicada.

Spring necesita abrir una transacción para garantizar que todo se elimine correctamente.

### 👶 Mente de niño

Es como darle una autorización especial al recepcionista para modificar el inventario del local.

---

# 🧩 Combinando Palabras Mágicas

Spring permite construir consultas complejas combinando palabras.

---

## Containing

Busca coincidencias parciales.

### Ejemplo

`java
findByTituloContaining(String palabraClave);
`

### 👶 Mente de niño

Si buscas:

`Craft`

Spring puede encontrar:

* Minecraft
* World of Warcraft

---

## And

Permite combinar condiciones.

### Ejemplo

`java
findByTituloAndLocalId(String titulo, Long localId);
`

### 👶 Mente de niño

"Busca Mario Kart que esté guardado en el Local 3."

Deben cumplirse ambas condiciones.

---

# 📚 Diccionario de Filtros Avanzados

Puedes combinar cualquiera de los verbos anteriores con filtros especiales.

---

## LessThan

Menor que.

### Ejemplo

`java
findByPrecioLessThan(Double precio);
`

### 👶 Mente de niño

"Muéstrame juegos más baratos de $20."

---

## GreaterThan

Mayor que.

### Ejemplo

`java
findByPrecioGreaterThan(Double precio);
`

### 👶 Mente de niño

"Muéstrame juegos más caros de $50."

---

## Between

Entre dos valores.

### Ejemplo

`java
findByPrecioBetween(Double minimo, Double maximo);
`

### 👶 Mente de niño

"Quiero juegos que cuesten entre $10 y $50."

---

## True

Solo valores verdaderos.

### Ejemplo

`java
findByDisponibleTrue();
`

### 👶 Mente de niño

"Muéstrame únicamente los juegos disponibles."

---

## False

Solo valores falsos.

### Ejemplo

`java
findByDisponibleFalse();
`

### 👶 Mente de niño

"Muéstrame únicamente los juegos agotados."

---

## OrderBy Asc

Orden ascendente.

### Ejemplo

`java
findByOrderByTituloAsc();
`

### 👶 Mente de niño

Ordena los juegos:

A → Z

---

## OrderBy Desc

Orden descendente.

### Ejemplo

`java
findByOrderByTituloDesc();
`

### 👶 Mente de niño

Ordena los juegos:

Z → A

---

# 🎒 Resumen para tu mochila de Senior

Spring Data JPA permite crear consultas simplemente escribiendo nombres de métodos.

No necesitas SQL para las consultas más comunes.

Solo debes combinar:

* `findBy` → Buscar
* `countBy` → Contar
* `existsBy` → Verificar
* `deleteBy` → Borrar

Y después agregar filtros como:

* `Containing`
* `And`
* `LessThan`
* `GreaterThan`
* `Between`
* `True`
* `False`
* `OrderBy`

Spring Boot leerá el nombre, entenderá la intención y construirá automáticamente la consulta SQL por ti.


---

## 🔙 [***Volver***](./Repositorio.md)