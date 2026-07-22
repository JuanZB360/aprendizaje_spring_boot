## 🔙 [***Volver***](./implementacionalidacion.md)

---

# 📋 Construyendo nuestra primera Interfaz de Validación

En la sección anterior aprendimos que nuestro `JuegoService` no debería depender directamente de una clase como `JuegoValidadorImpl`.

En su lugar, debe depender de una **interfaz**, ya que esto hace que el código sea más flexible y desacoplado.

Pero ahora surge una nueva pregunta.

> 🤔 ¿Qué es exactamente una interfaz?

Y, sobre todo...

> 🤔 ¿Qué debemos escribir dentro de ella?

Vamos a descubrirlo paso a paso.

---

# 🤔 ¿Qué es una interfaz?

Una **interfaz** es un contrato.

No contiene la lógica de la aplicación.

No guarda datos.

No consulta la base de datos.

No realiza validaciones.

Simplemente define **qué operaciones deben existir**.

Una forma sencilla de entenderlo es pensar que una interfaz es una lista de reglas obligatorias.

Por ejemplo.

> "Todo validador de videojuegos debe ser capaz de validar un videojuego."

No importa cómo lo haga.

Lo único importante es que exista ese comportamiento.

---

# 👶 La Analogía del Contrato de Trabajo

Imagina que el dueño del Local de Videojuegos quiere contratar un guardia de seguridad.

Antes de contratarlo redacta un documento.

Ese documento dice.

```text
Todo guardia deberá:

✔ Revisar la identificación.

✔ Verificar que el cliente tenga una reserva.

✔ Impedir el ingreso si encuentra un problema.
```

Observa algo importante.

Ese documento **no explica cómo revisar una identificación**.

Tampoco explica **cómo verificar una reserva**.

Solo establece las obligaciones que debe cumplir cualquier guardia.

Eso es exactamente una interfaz.

Una interfaz no dice **cómo** hacer las cosas.

Solo dice **qué cosas deben hacerse**.

---

# 🧠 Aplicándolo a Spring Boot

Nuestro sistema necesita un validador.

No nos interesa cómo está construido.

Solo queremos estar seguros de que cualquier validador pueda ejecutar una operación llamada:

```java
validar(juego);
```

Por eso crearemos una interfaz.

---

# 🏗️ Creando la interfaz

```java
package com.localvideojuegos.validation;

public interface IJuegoValidador {

    void validar(Juego juego);

}
```

A simple vista parece una clase muy extraña.

No tiene atributos.

No tiene lógica.

No tiene instrucciones.

Solo tiene un método.

Entonces...

¿Para qué sirve?

---

# 🔍 Analizando el código

## `public interface`

```java
public interface IJuegoValidador
```

La palabra `interface` le dice a Java que estamos creando un contrato.

No una clase.

Una interfaz no puede crear objetos.

Por ejemplo, esto es incorrecto.

```java
IJuegoValidador validador = new IJuegoValidador();
```

Java mostrará un error.

¿Por qué?

Porque las interfaces no tienen implementación.

Solo describen comportamientos.

---

## ¿Por qué comienza con la letra **I**?

Muchos proyectos utilizan una convención para identificar rápidamente las interfaces.

Por ejemplo.

```text
IJuegoValidador
```

La letra **I** significa:

> **Interface**

No es una obligación del lenguaje Java.

Es simplemente una convención utilizada por muchos equipos de desarrollo.

Otros proyectos prefieren nombres como:

```text
JuegoValidador
```

o

```text
JuegoValidationService
```

Lo importante no es el nombre.

Lo importante es entender que representa una abstracción.

---

## El método `validar()`

Observa el único método de la interfaz.

```java
void validar(Juego juego);
```

Aquí también hay varias cosas importantes.

---

### `void`

Significa que el método no devolverá ningún valor.

¿Por qué?

Porque nuestro objetivo no es obtener información.

Nuestro objetivo es verificar si el objeto cumple las reglas del negocio.

Si todo está correcto...

El método simplemente terminará.

Si encuentra un error...

Lanzará una excepción.

---

### `Juego juego`

Este será el objeto que deseamos validar.

Por ejemplo.

```java
Juego juego = new Juego();

juego.setNombre("Minecraft");

juego.setPrecio(120000.0);

juego.setConsola("PlayStation 5");
```

Ese objeto será enviado al validador.

```java
validador.validar(juego);
```

El validador revisará todos sus datos antes de permitir que continúe el proceso.

---

# 🤔 ¿Por qué solo existe un método?

Muchos principiantes imaginan que la interfaz debería verse así.

```java
public interface IJuegoValidador {

    void validarNombre(String nombre);

    void validarPrecio(Double precio);

    void validarConsola(String consola);

}
```

Aunque esto podría funcionar, no suele ser la mejor opción.

Nuestro servicio tendría que recordar ejecutar cada validación manualmente.

```java
validador.validarNombre(...);

validador.validarPrecio(...);

validador.validarConsola(...);
```

Si olvidamos una llamada...

Esa regla nunca se ejecutará.

---

# ✅ Una única puerta de entrada

Por eso normalmente diseñamos una única operación principal.

```java
void validar(Juego juego);
```

Esta operación actuará como el director de una orquesta.

Cuando alguien la invoque.

```java
validador.validar(juego);
```

Internamente ejecutará todas las reglas necesarias.

Por ejemplo.

```text
validar(juego)

│

├── validarNombre()

├── validarPrecio()

├── validarConsola()

└── validarJuegoDuplicado()
```

De esta manera el resto de la aplicación solamente necesita recordar una única llamada.

Es mucho más limpio y reduce la posibilidad de errores.

---

# 📌 ¿Quién implementará este contrato?

Hasta este momento solamente hemos definido las reglas.

Pero todavía nadie las está cumpliendo.

Es exactamente igual que el contrato del guardia de seguridad.

El documento dice qué debe hacerse.

Pero todavía no hemos contratado al guardia.

En Java ocurre lo mismo.

La interfaz únicamente describe el comportamiento esperado.

Necesitamos una clase que acepte ese contrato y escriba el código real.

Esa será nuestra clase implementadora.

La construiremos en la siguiente sección.

---

# 🎒 Resumen

| Concepto                    | Función                                                            |
| --------------------------- | ------------------------------------------------------------------ |
| `interface`                 | Define un contrato que otras clases deberán cumplir.               |
| No contiene lógica          | Solo describe las operaciones disponibles.                         |
| `void validar(Juego juego)` | Define la operación principal de validación.                       |
| Una única puerta de entrada | Permite centralizar todas las reglas del negocio en un solo punto. |
| Clase implementadora        | Será la encargada de escribir la lógica real de las validaciones.  |

---

## 🔙 [***Volver***](./implementacionalidacion.md)
