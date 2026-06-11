## 🔙 [***Volver***](./Repositorio.md)

---

# 🗄️ El Recepcionista de Fábrica

## Consultas ya establecidas / Métodos Heredados

Cuando creamos un repositorio en Spring Boot, ocurre un truco de magia espectacular.

No tenemos que escribir código desde cero.

Usamos una interfaz que hereda (compra los superpoderes) de algo llamado `JpaRepository`.

---

## 👶 Explicación más sencilla

Imagina que contratamos a un nuevo Recepcionista para el local.

El primer día de trabajo, en lugar de enseñarle todo desde cero, le regalamos un **Manual de Operaciones Mágicas**.

Este manual ya viene con unos botones entrenados de fábrica.

Si el recepcionista presiona un botón, el manual hace el trabajo aburrido por él en la base de datos sin que tengamos que explicarle cómo hacerlo.

---

## 🛠️ ¿Cómo se configura en el código?

Mira lo sencillo que es crear al recepcionista para nuestra entidad `Juego`:

```java
package com.montoapp.repositories;

import com.montoapp.entities.Juego;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface JuegoRepository extends JpaRepository<Juego, Long> {
// ¡Mira mamá, está vacío! No hay código adentro.
}
```

---

## 🔍 ¿Qué significan los términos extraños?

### 📋 `interface`

No es una clase normal (`class`).

Es una interfaz.

Es como una lista de promesas o un contrato que dice qué sabe hacer nuestro recepcionista.

---

### 🧩 `extends JpaRepository<Juego, Long>`

Aquí ocurre la magia de Senior.

Le decimos a Spring:

> "Dale a este recepcionista el manual de fábrica para controlar la entidad `Juego`, y toma en cuenta que su llave primaria (`@Id`) es de tipo `Long` (un número grande)."

---

### 🏷️ `@Repository`

Es la etiqueta que le avisa a Spring Boot:

> "Oye, este muchacho es el encargado de hablar con el sótano (la base de datos), tenlo listo."

---

## 💡 Lo más importante

Gracias a `JpaRepository`, nuestro repositorio ya nace con muchos métodos listos para usar.

No tenemos que programarlos manualmente.

Spring Boot los hereda automáticamente.

---

## 🔙 [***Volver***](./Repositorio.md)
