## 🔙 [***Volver***](./implementacionalidacion.md)

---

# 🚨 Entendiendo `ResponseStatusException`

# ¿Qué ocurre realmente cuando lanzamos una excepción?

Hasta este momento ya construimos la estructura de nuestro validador.

```java
@Component
@RequiredArgsConstructor
public class JuegoValidadorImpl implements IJuegoValidador {

    @Override
    public void validar(Juego juego){

        validarTitulo(juego);

        validarPrecio(juego);

        validarConsola(juego);

        validarJuegoDuplicado(juego);

    }

}
```

Ahora debemos responder una pregunta muy importante.

> 🤔 **¿Qué debe hacer nuestro programa cuando encuentra un dato incorrecto?**

Imaginemos que el usuario intenta registrar este juego.

```text
Título:
Precio: 35.000
Consola: PC
```

El título está vacío.

Claramente ese juego **no debería guardarse**.

Entonces...

¿Qué hacemos?

---

# 🤔 Las posibles soluciones

Podríamos pensar en varias opciones.

## Opción 1

Devolver un `boolean`.

```java
public boolean validar(Juego juego){
    ...
}
```

---

## Opción 2

Devolver `null`.

```java
return null;
```

---

## Opción 3

Imprimir un mensaje por consola.

```java
System.out.println("Título inválido");
```

---

Aunque todas estas opciones parecen funcionar...

Ninguna detiene realmente la ejecución del programa.

Y eso representa un problema.

---

# 🎮 Volvamos al Local de Videojuegos

Imagina que un cliente entrega un formulario para registrar un nuevo juego.

El empleado comienza a revisarlo.

```text
☑ Tiene precio

☑ Tiene consola

❌ El título está vacío
```

¿Qué debería hacer el empleado?

¿Continuar registrándolo?

Claro que no.

En ese mismo instante debe detener todo el proceso.

Debe decirle al cliente.

> "No puedo registrar este juego porque el título es obligatorio."

Eso mismo hacen las excepciones.

---

# 💥 ¿Qué es una excepción?

Una excepción es una forma de decirle al programa:

> **"Ha ocurrido un problema. No puedes continuar por este camino."**

Cuando una excepción aparece...

La ejecución normal del programa se interrumpe inmediatamente.

Podemos imaginarlo así.

```text
Inicio

↓

Validar título

↓

Validar precio

↓

Guardar juego

↓

Fin
```

Todo parece normal.

Pero ahora aparece un error.

```text
Inicio

↓

Validar título

↓

❌ Error

↓

💥 Se detiene todo

↓

No se guarda el juego
```

Observa algo muy importante.

Después del error...

Nada más continúa ejecutándose.

---

# 🔥 La palabra `throw`

En Java existe una palabra reservada muy especial.

```java
throw
```

Su significado es literalmente:

> **"Lanzar una excepción."**

Cuando Java encuentra un `throw`, ocurre algo parecido a esto.

```text
Programa

↓

Encuentra un problema

↓

throw

↓

Se interrumpe el flujo

↓

El error comienza a subir por la aplicación
```

---

# 🧠 ¿Qué significa "subir"?

Imagina una llamada entre métodos.

```text
Controller

↓

Service

↓

Validador
```

El controlador llama al servicio.

El servicio llama al validador.

Ahora el validador encuentra un error.

```text
Controller

↓

Service

↓

Validador

↓

💥 throw
```

La excepción no se queda allí.

Empieza a regresar por el mismo camino.

```text
Validador

↑

Service

↑

Controller

↑

Spring Boot
```

Por eso decimos que una excepción **se propaga**.

Va subiendo por la pila de llamadas hasta que alguien la captura.

---

# 🤔 ¿Quién captura la excepción?

Aquí ocurre una de las grandes ventajas de Spring Boot.

Nosotros no necesitamos escribir esto.

```java
try{

}
catch(Exception e){

}
```

En la mayoría de los casos.

Spring Boot ya posee un sistema interno encargado de capturar las excepciones.

Cuando la excepción llega hasta Spring...

Él sabe cómo convertirla en una respuesta HTTP.

---

# 🎯 Aquí aparece `ResponseStatusException`

Spring proporciona una excepción muy especial.

```java
ResponseStatusException
```

Esta excepción no solo indica que ocurrió un error.

También informa cuál debe ser el código HTTP que recibirá el cliente.

Por ejemplo.

```java
throw new ResponseStatusException(
    HttpStatus.BAD_REQUEST,
    "El título del juego es obligatorio."
);
```

Aquí estamos diciendo dos cosas al mismo tiempo.

1. Ocurrió un error.

2. Ese error corresponde a un **400 Bad Request**.

---

# 🔍 Analizando la instrucción paso a paso

Veamos nuevamente el código.

```java
throw new ResponseStatusException(
    HttpStatus.BAD_REQUEST,
    "El título del juego es obligatorio."
);
```

Analicemos cada parte.

---

## `throw`

Indica que vamos a lanzar una excepción.

En ese momento la ejecución del método se detiene.

---

## `new`

Crea un nuevo objeto de tipo `ResponseStatusException`.

Recuerda que una excepción también es un objeto en Java.

---

## `ResponseStatusException`

Es una excepción proporcionada por Spring Boot.

Su función consiste en transportar información sobre el error.

---

## `HttpStatus.BAD_REQUEST`

Indica el código HTTP que recibirá el cliente.

En este caso.

```text
400 Bad Request
```

Que significa:

> "El cliente envió información incorrecta."

---

## `"El título del juego es obligatorio"`

Es el mensaje que explica el motivo del error.

Será muy útil para quien esté consumiendo nuestra API.

---

# 🌐 ¿Qué recibe el cliente?

Supongamos que alguien intenta registrar un juego sin título.

La aplicación lanzará esta excepción.

```java
throw new ResponseStatusException(
    HttpStatus.BAD_REQUEST,
    "El título del juego es obligatorio."
);
```

Spring capturará automáticamente esa excepción y enviará una respuesta parecida a esta.

```json
{
    "timestamp": "2026-07-22T15:30:45.120",
    "status": 400,
    "error": "Bad Request",
    "message": "El título del juego es obligatorio.",
    "path": "/juegos"
}
```

Observa que nosotros nunca construimos este JSON.

Spring Boot lo hizo automáticamente.

---

# 🧠 Todo el recorrido de una excepción

Podemos resumir el proceso completo así.

```text
Cliente

↓

Controller

↓

Service

↓

JuegoValidador

↓

❌ Detecta un error

↓

throw ResponseStatusException

↓

Spring captura la excepción

↓

Spring construye la respuesta HTTP

↓

Cliente recibe:

400 Bad Request
```

---

# 🎒 Resumen

| Concepto                  | Explicación                                                                |
| ------------------------- | -------------------------------------------------------------------------- |
| Excepción                 | Indica que ocurrió un problema y que el flujo debe detenerse.              |
| `throw`                   | Lanza una excepción y detiene inmediatamente la ejecución del método.      |
| `ResponseStatusException` | Excepción proporcionada por Spring para devolver errores HTTP.             |
| `HttpStatus.BAD_REQUEST`  | Código HTTP **400**, utilizado cuando el cliente envía datos inválidos.    |
| Spring Boot               | Captura automáticamente la excepción y la convierte en una respuesta HTTP. |

---

# 🏆 Conclusión

Ahora ya entendemos qué ocurre realmente cuando utilizamos `ResponseStatusException`.

No es simplemente una línea de código.

Es un mecanismo mediante el cual detenemos la ejecución del programa, informamos que ocurrió un error y permitimos que Spring Boot genere automáticamente una respuesta HTTP clara para el cliente.

---

## 🔙 [***Volver***](./implementacionalidacion.md)