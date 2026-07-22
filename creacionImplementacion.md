## 🔙 [***Volver***](./implementacionalidacion.md)

---

# 🏗️ Construyendo la Clase Implementadora

# Donde el contrato cobra vida

En la sección anterior construimos nuestra primera interfaz.

```java
public interface IJuegoValidador {

    void validar(Juego juego);

}
```

Pero si intentáramos ejecutar la aplicación en este momento, descubriríamos un pequeño problema.

La interfaz todavía **no hace absolutamente nada**.

Solo define un contrato.

Es como el contrato del guardia de seguridad.

El documento dice qué obligaciones debe cumplir.

Pero todavía no existe ninguna persona que las realice.

Necesitamos contratar al guardia.

En Java ocurre exactamente lo mismo.

Necesitamos una clase que acepte ese contrato y escriba la lógica real de las validaciones.

---

# 🎭 Del contrato a la realidad

Recordemos la analogía.

Tenemos este documento.

```text
Todo guardia deberá:

✔ Revisar documentos.

✔ Verificar la reserva.

✔ Impedir el ingreso si encuentra un problema.
```

Ese documento no trabaja.

No habla.

No toma decisiones.

Simplemente existe.

Ahora llega Juan y firma el contrato.

Desde ese momento Juan se convierte en el guardia que realmente realiza todas esas tareas.

En Java ocurre exactamente lo mismo.

La interfaz representa el contrato.

La clase implementadora representa al trabajador que acepta cumplirlo.

---

# 🤔 ¿Qué significa implementar una interfaz?

Cuando una clase implementa una interfaz está diciendo algo muy sencillo.

> **"Acepto cumplir todas las reglas definidas en este contrato."**

Eso se hace mediante la palabra clave:

```java
implements
```

---

# 🏗️ Nuestra primera implementación

```java
package com.localvideojuegos.validation;

import org.springframework.stereotype.Component;

@Component
public class JuegoValidadorImpl implements IJuegoValidador {

    @Override
    public void validar(Juego juego) {

        // Aquí escribiremos todas las reglas
        // de validación.

    }

}
```

A simple vista parece una clase muy pequeña.

Pero contiene varios conceptos fundamentales de Spring Boot.

Vamos a analizarlos uno por uno.

---

# 🔍 Analizando el código

## `public class`

```java
public class JuegoValidadorImpl
```

Ahora sí estamos creando una clase real.

A diferencia de una interfaz, esta clase:

* puede crear objetos;
* puede tener atributos;
* puede ejecutar código;
* puede contener lógica de negocio.

Aquí es donde realmente ocurrirán las validaciones.

---

## `implements`

```java
implements IJuegoValidador
```

Esta palabra es una promesa.

Le dice al compilador de Java:

> "Esta clase cumplirá todo lo que exige la interfaz."

Eso significa que obligatoriamente debemos implementar este método.

```java
void validar(Juego juego);
```

Si olvidamos escribirlo, Java mostrará un error de compilación.

¿Por qué?

Porque firmamos un contrato y ahora debemos cumplirlo.

---

# 👶 La Analogía de Firmar un Contrato

Imagina que una empresa publica esta oferta de trabajo.

```text
Se necesita Guardía de Seguridad.

Funciones obligatorias:

✔ Revisar documentos.

✔ Controlar el ingreso.

✔ Reportar incidentes.
```

Juan firma el contrato.

Desde ese momento está obligado a realizar esas tres funciones.

No puede decidir hacer solo una.

Debe cumplirlas todas.

Eso mismo ocurre cuando una clase utiliza:

```java
implements
```

Java obliga a implementar todos los métodos definidos por la interfaz.

---

# 🔍 ¿Qué hace `@Override`?

Encima del método aparece otra anotación.

```java
@Override
```

Muchos principiantes creen que esta anotación hace que el método funcione.

En realidad, no.

El método funcionaría exactamente igual sin ella.

Entonces...

¿Por qué existe?

Porque ayuda al compilador a verificar que realmente estamos sobrescribiendo un método definido en la interfaz.

Observa este ejemplo.

```java
@Override
public void validar(Juego juego){

}
```

Todo está correcto.

Ahora imaginemos que escribimos mal el nombre.

```java
@Override
public void validarJuego(Juego juego){

}
```

Gracias a `@Override`, Java detectará inmediatamente el error.

Nos dirá que ese método no existe en la interfaz.

Sin `@Override`, ese error podría pasar desapercibido y generar problemas mucho más difíciles de encontrar.

Por eso es una excelente práctica utilizarla siempre que implementemos una interfaz o sobrescribamos un método heredado.

---

# 🧠 ¿Qué hace realmente el método `validar()`?

Por ahora nuestro método está vacío.

```java
@Override
public void validar(Juego juego){

}
```

Eso significa que todavía no estamos validando nada.

Pero muy pronto comenzaremos a agregar reglas.

Por ejemplo.

```text
validar(juego)

│

├── validarNombre()

├── validarPrecio()

├── validarConsola()

├── validarJuegoDuplicado()

└── validarStock()
```

Observa algo interesante.

El método `validar()` no necesariamente contendrá toda la lógica.

Su trabajo será coordinar las distintas validaciones.

Cada regla podrá vivir en un método independiente.

Esto hará que el código sea mucho más limpio y fácil de mantener.

---

# 🤔 ¿Por qué terminar el nombre con `Impl`?

Nuestra clase se llama:

```text
JuegoValidadorImpl
```

Muchos desarrolladores se preguntan qué significa ese sufijo.

`Impl` es una abreviatura de:

> **Implementation** (Implementación).

Su función es indicar rápidamente que esta clase contiene la implementación concreta de una interfaz.

Por ejemplo.

```text
IJuegoValidador
```

es el contrato.

Mientras que.

```text
JuegoValidadorImpl
```

es quien cumple ese contrato.

No es obligatorio utilizar este nombre.

Pero es una convención ampliamente utilizada en proyectos Java.

---

# 🎒 Resumen

| Concepto             | Función                                                                  |
| -------------------- | ------------------------------------------------------------------------ |
| `implements`         | Indica que la clase acepta cumplir el contrato definido por la interfaz. |
| Clase implementadora | Contiene la lógica real de las validaciones.                             |
| `@Override`          | Verifica que realmente estamos implementando un método del contrato.     |
| `validar()`          | Será el punto de entrada que coordinará todas las reglas de validación.  |
| `Impl`               | Convención utilizada para identificar la implementación de una interfaz. |

---

## 🔙 [***Volver***](./implementacionalidacion.md)
