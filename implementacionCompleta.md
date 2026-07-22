## 🔙 [***Volver***](./implementacionalidacion.md)

---

# 🎯 Nuestra primera regla de negocio

# Validando que el título del juego sea obligatorio

Ya tenemos nuestro método principal.

```java
@Override
public void validar(Juego juego){

    validarTitulo(juego);

    validarPrecio(juego);

    validarConsola(juego);

    validarJuegoDuplicado(juego);

}
```

Por ahora ninguno de estos métodos hace nada.

Hoy construiremos el primero.

```java
validarTitulo(juego);
```

Puede parecer una validación muy sencilla.

Pero aprenderemos un patrón que después podremos reutilizar para validar cualquier dato de nuestra aplicación.

---

# 🎮 Volvamos al Local de Videojuegos

Un cliente llega al mostrador y dice:

> "Quiero registrar este nuevo juego."

El empleado recibe el formulario.

```text
Título: Minecraft

Precio: 80.000

Consola: PlayStation 5
```

Lo primero que hace es mirar el título.

¿Por qué?

Porque un juego no puede existir sin un nombre.

Sería imposible:

* buscarlo,
* venderlo,
* alquilarlo,
* o mostrarlo en un catálogo.

Por eso la primera regla del negocio será:

> **Todo juego debe tener un título.**

---

# 🧠 Antes de escribir código debemos definir la regla

Uno de los errores más comunes es comenzar a programar sin pensar primero en la regla de negocio.

Antes de escribir una sola línea debemos responder una pregunta.

## ¿Qué queremos impedir?

En nuestro caso queremos impedir que ocurra cualquiera de estas situaciones.

```text
Título = null
```

o

```text
Título = ""
```

o incluso.

```text
Título = "        "
```

Aunque el usuario haya escrito espacios...

Para el negocio sigue siendo un título vacío.

Por lo tanto.

Todos esos casos son inválidos.

---

# 🤔 ¿Cómo comprobamos eso en Java?

Podríamos escribir algo como esto.

```java
if(juego.getTitulo() == null){

}
```

Pero esta condición solo detecta un problema.

¿Qué ocurre si el usuario envía una cadena vacía?

```text
""
```

La condición anterior no la detectará.

Ahora podríamos agregar otra.

```java
if(juego.getTitulo().isEmpty()){

}
```

Mejor.

Pero todavía existe otro caso.

```text
"        "
```

Es una cadena formada únicamente por espacios.

Visualmente parece vacía.

Pero técnicamente contiene caracteres.

---

# ⭐ La solución: `isBlank()`

Java nos ofrece un método mucho más completo.

```java
isBlank()
```

Este método devuelve `true` cuando la cadena:

* está vacía;
* contiene únicamente espacios;
* o está formada por tabulaciones y otros caracteres en blanco.

Por ejemplo.

```java
"".isBlank();
```

Devuelve.

```text
true
```

---

```java
"     ".isBlank();
```

También devuelve.

```text
true
```

---

```java
"Minecraft".isBlank();
```

Devuelve.

```text
false
```

Porque sí contiene texto.

---

# ⚠️ Primero debemos comprobar si es `null`

Existe un pequeño detalle muy importante.

Si escribimos directamente esto.

```java
juego.getTitulo().isBlank();
```

Y el título vale `null`...

Nuestra aplicación fallará inmediatamente con un:

```text
NullPointerException
```

¿Por qué?

Porque `null` no representa un objeto.

Y solamente los objetos poseen métodos.

Por eso primero debemos comprobar si el título existe.

---

# 💻 Construyendo nuestra validación

Ahora sí podemos escribir nuestro método.

```java
private void validarTitulo(Juego juego){

    if(juego.getTitulo() == null ||
       juego.getTitulo().isBlank()){

        throw new ResponseStatusException(
                HttpStatus.BAD_REQUEST,
                "El título del juego es obligatorio."
        );

    }

}
```

Observemos el código paso a paso.

---

# 🔍 Analizando la condición

```java
juego.getTitulo() == null
```

Comprueba que el título exista.

Si no existe.

No podemos continuar.

---

```java
juego.getTitulo().isBlank()
```

Comprueba que el usuario realmente haya escrito un texto.

No basta con enviar espacios.

---

```java
||
```

Este operador significa.

> **"O".**

Por lo tanto.

La condición completa puede leerse así.

> Si el título es `null` **o** está vacío, entonces el juego no es válido.

---

# 💥 ¿Qué ocurre cuando la condición se cumple?

Si el título es inválido.

Entramos al `if`.

```java
throw new ResponseStatusException(
        HttpStatus.BAD_REQUEST,
        "El título del juego es obligatorio."
);
```

En ese instante ocurre todo lo que aprendimos en el capítulo anterior.

```text
Validar título

↓

Se detecta un error

↓

throw

↓

Se detiene el método

↓

La excepción sube hasta Spring

↓

Spring responde:

400 Bad Request
```

Observa algo muy importante.

Después del `throw`...

No se ejecuta ninguna otra validación.

---

# 🚫 ¿Qué ocurre con las demás reglas?

Imaginemos este método.

```java
@Override
public void validar(Juego juego){

    validarTitulo(juego);

    validarPrecio(juego);

    validarConsola(juego);

    validarJuegoDuplicado(juego);

}
```

Si `validarTitulo()` lanza una excepción.

El flujo termina inmediatamente.

```text
validarTitulo()

↓

❌ Error

↓

💥 Fin del método
```

Los demás métodos nunca se ejecutan.

```text
❌ validarPrecio()

❌ validarConsola()

❌ validarJuegoDuplicado()
```

Y eso es exactamente lo que queremos.

No tiene sentido seguir validando un objeto que ya sabemos que es inválido.

---

# 🎮 La Analogía del Inspector

Imagina que un inspector revisa un nuevo Local de Videojuegos.

La primera comprobación consiste en verificar si existe una salida de emergencia.

No existe.

¿Tiene sentido seguir revisando la iluminación?

¿O los extintores?

Claro que no.

La inspección termina inmediatamente.

Primero debe solucionarse ese problema.

Nuestro validador trabaja exactamente igual.

Cuando encuentra la primera regla incumplida.

Detiene toda la revisión.

---

# 💡 Una buena práctica

Cada método de validación debería comprobar una única regla.

Por ejemplo.

```java
validarTitulo();
```

Solo valida el título.

---

```java
validarPrecio();
```

Solo valida el precio.

---

```java
validarConsola();
```

Solo valida la consola.

---

```java
validarJuegoDuplicado();
```

Solo comprueba si ya existe un juego con el mismo nombre.

Este principio hace que el código sea mucho más limpio, reutilizable y fácil de mantener.

---

# 🎒 Resumen

| Concepto             | Explicación                                                |   |                    |
| -------------------- | ---------------------------------------------------------- | - | ------------------ |
| Primera regla        | Todo juego debe tener un título.                           |   |                    |
| `null`               | El título no existe.                                       |   |                    |
| `isBlank()`          | Detecta cadenas vacías o formadas únicamente por espacios. |   |                    |
| `                    |                                                            | ` | Significa **"o"**. |
| `throw`              | Detiene inmediatamente la ejecución del método.            |   |                    |
| Una regla por método | Facilita el mantenimiento y la reutilización del código.   |   |                    |

---

# 🏆 Conclusión

Ya construimos nuestra primera regla de negocio completa.

Aprendimos a detectar un dato inválido, a lanzar una `ResponseStatusException` y a detener el flujo de la aplicación cuando una condición del negocio no se cumple.

Lo más importante es que esta estructura no sirve únicamente para validar el título de un juego.

---

## 🔙 [***Volver***](./implementacionalidacion.md)