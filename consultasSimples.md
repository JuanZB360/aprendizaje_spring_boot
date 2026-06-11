## 🔙 [***Volver***](./Repositorio.md)

---

# 🎮 Los Botones de Fábrica

## Métodos que ya vienen listos para usar

Por el solo hecho de escribir esta línea:

`public interface JuegoRepository extends JpaRepository<Juego, Long>`

Tu `JuegoRepository` ya aprendió a usar estos botones principales dentro de la capa de servicios.

---

## 💾 1. `save(objeto)`

### El botón de guardar

### 📌 ¿Qué hace?

Toma un juego que creaste en Java y lo guarda en la base de datos.

Si el juego ya existía, actualiza su información.

### 👶 Mente de niño

Es como meter un cassette nuevo en la vitrina del local.

Si el cassette ya estaba allí, simplemente cambias la etiqueta por una nueva.

---

## 🔎 2. `findById(id)`

### El botón de buscar por número

### 📌 ¿Qué hace?

Busca un juego usando su número de identificación.

Por ejemplo:

`findById(5)`

Intentará encontrar el juego que tenga el ID `5`.

Devuelve un `Optional`, que es una cajita que puede:

* venir con el juego adentro,
* o venir vacía si no existe.

### 👶 Mente de niño

"Busca el juego que está guardado en el casillero número 5".

---

## 📚 3. `findAll()`

### El botón de ver todo

### 📌 ¿Qué hace?

Trae una lista con todos los juegos registrados en la base de datos.

Devuelve:

`List<Juego>`

### 👶 Mente de niño

"Saca todos los juegos de las vitrinas y colócalos sobre una mesa para que pueda verlos".

---

## 🗑️ 4. `deleteById(id)`

### El botón de triturar

### 📌 ¿Qué hace?

Busca un juego por su ID y lo elimina completamente de la base de datos.

Por ejemplo:

`deleteById(5)`

Borrará el juego con ID `5`.

### 👶 Mente de niño

"Coge el juego del casillero número 5 y tíralo a la basura".

---

## 🎒 Resumen para tu mochila de Senior

Las consultas ya establecidas son métodos que tú no tienes que programar.

Spring Data JPA ya los creó por ti.

Con solo crear una interfaz que herede de `JpaRepository`, obtienes automáticamente la capacidad de:

* guardar,
* buscar,
* listar,
* actualizar,
* y borrar registros.

Todo esto sin escribir una sola consulta SQL.

---

## 🔙 [***Volver***](./Repositorio.md)
