## 🔙 [***Volver***](./implementacionalidacion.md)

---

# 🌱 ¿Qué hace realmente `@Component`?

# Descubriendo los Beans y el Contenedor de Spring

Hasta este momento ya tenemos nuestra clase implementadora.

```java
@Component
public class JuegoValidadorImpl implements IJuegoValidador {

    @Override
    public void validar(Juego juego){

    }

}
```

Probablemente ya has visto cientos de clases parecidas.

Incluso puede que tú mismo hayas escrito muchas anotaciones como estas.

```java
@Component

@Service

@Repository

@Controller
```

Pero...

¿Alguna vez te has preguntado qué ocurre cuando Spring Boot encuentra una de estas anotaciones?

¿Por qué simplemente escribimos un `@Component` y después podemos utilizar esa clase en cualquier parte del proyecto?

Para responder esa pregunta primero debemos conocer uno de los conceptos más importantes de Spring Boot.

Los **Beans**.

---

# 🤔 El problema antes de Spring

Imagina que estamos programando únicamente con Java.

Queremos utilizar nuestro validador.

Entonces debemos crear el objeto manualmente.

```java
public class JuegoService {

    private JuegoValidadorImpl validador =
            new JuegoValidadorImpl();

}
```

Parece sencillo.

Pero ahora imagina que el validador necesita un repositorio.

```java
public class JuegoValidadorImpl {

    private JuegoRepository repository =
            new JuegoRepository();

}
```

Y ese repositorio necesita otra clase.

```java
private Conexion conexion =
        new Conexion();
```

Y la conexión necesita configuración.

Y la configuración necesita otras dependencias.

Poco a poco nosotros mismos terminamos construyendo toda la aplicación utilizando `new`.

```text
JuegoService

↓

new JuegoValidadorImpl()

↓

new JuegoRepository()

↓

new Conexion()

↓

new Driver()

↓

Base de datos
```

Mientras más crece el proyecto...

Más objetos debemos crear manualmente.

Más dependencias aparecen.

Y más difícil se vuelve mantener el código.

---

# 💡 La solución de Spring

Spring decidió cambiar completamente esta forma de trabajar.

En lugar de que el programador cree todos los objetos...

Spring los crea automáticamente.

Nosotros solamente debemos decirle cuáles clases queremos que administre.

Y eso se logra con anotaciones como:

```java
@Component
```

---

# 👶 La Analogía del Local de Videojuegos

Volvamos a nuestro Local de Videojuegos.

Imagina que el dueño necesita contratar empleados.

Tiene varias opciones.

## Opción 1

Cada vez que necesita un empleado...

Sale a buscar uno.

```text
Necesito un cajero.

↓

Lo contrato.

↓

Necesito otro.

↓

Lo vuelvo a contratar.

↓

Necesito otro.

↓

Lo vuelvo a contratar.
```

Muchísimo trabajo.

---

## Opción 2

Ahora el Local tiene una oficina de Recursos Humanos.

Todos los empleados trabajan para esa oficina.

Cuando alguien necesita un cajero simplemente dice:

> "Necesito un cajero."

Recursos Humanos responde.

> "Aquí tienes uno."

El empleado ya estaba contratado.

No fue necesario volver a buscarlo.

Eso mismo hace Spring.

---

# 🧠 El Contenedor de Spring

Spring posee una enorme oficina llamada:

# 📦 IoC Container

También conocido como:

* Spring Container
* Application Context
* Contenedor de Beans

Su trabajo consiste en administrar todos los objetos importantes de la aplicación.

Podemos imaginarlo así.

```text
                Spring Boot

                     │

                     ▼

          ┌────────────────────┐
          │   IoC Container     │
          ├────────────────────┤
          │                    │
          │ JuegoService       │
          │ JuegoRepository    │
          │ JuegoValidadorImpl │
          │ EmailService       │
          │ JwtService         │
          │                    │
          └────────────────────┘
```

Todos esos objetos viven dentro del contenedor.

Spring sabe dónde están.

Cómo crearlos.

Y cuándo entregarlos.

---

# 🤔 ¿Qué es un Bean?

Un **Bean** es simplemente un objeto cuya vida está siendo administrada por Spring.

Dicho de otra manera.

Un Bean es una instancia creada automáticamente por el contenedor.

Por ejemplo.

```java
@Component
public class JuegoValidadorImpl{

}
```

Cuando Spring encuentra esta clase...

Hace algo parecido a esto.

```java
JuegoValidadorImpl bean =
        new JuegoValidadorImpl();
```

Pero lo hace internamente.

Después guarda ese objeto dentro de su contenedor.

A partir de ese momento podrá reutilizarlo todas las veces que sea necesario.

---

# 🔍 ¿Qué ocurre cuando inicia Spring Boot?

Cuando ejecutamos nuestra aplicación ocurre una secuencia muy interesante.

## Paso 1

Spring inicia.

```text
Spring Boot

↓

Comienza el arranque
```

---

## Paso 2

Empieza a recorrer todo nuestro proyecto.

```text
📂 controllers

📂 services

📂 repositories

📂 validations

📂 configurations
```

A este proceso se le conoce como:

> **Component Scanning**

---

## Paso 3

Mientras recorre las clases busca anotaciones especiales.

Como estas.

```java
@Component

@Service

@Repository

@Controller
```

Cada vez que encuentra una...

La registra como un Bean.

---

## Paso 4

Después crea automáticamente el objeto.

```java
JuegoValidadorImpl bean =
        new JuegoValidadorImpl();
```

---

## Paso 5

Finalmente lo guarda dentro del IoC Container.

```text
          IoC Container

             │

             ├── JuegoService

             ├── JuegoRepository

             ├── JuegoValidadorImpl

             ├── JwtService

             └── EmailService
```

A partir de ese momento cualquier otra clase podrá utilizar ese Bean sin necesidad de escribir `new`.

---

# 🎯 Entonces...

Cuando escribimos esto.

```java
@Component
public class JuegoValidadorImpl{

}
```

En realidad le estamos diciendo a Spring.

> "Esta clase es importante. Créala por mí y guárdala dentro del contenedor."

Eso es todo.

No hace magia.

No valida nada.

No modifica el comportamiento de la clase.

Simplemente le pide a Spring que la administre.

---

# 🤔 ¿Por qué esto es tan importante?

Porque muy pronto podremos hacer algo como esto.

```java
@Service
@RequiredArgsConstructor
public class JuegoService {

    private final IJuegoValidador validador;

}
```

Y Spring responderá.

> "No te preocupes."

> "Ya tengo un Bean que implementa esa interfaz."

> "Aquí lo tienes."

Eso se llama **Inyección de Dependencias**, y será el siguiente paso de nuestro recorrido.

---

# 🎒 Resumen

| Concepto           | Función                                                       |
| ------------------ | ------------------------------------------------------------- |
| `@Component`       | Registra una clase para que Spring la administre.             |
| Bean               | Objeto creado y administrado por Spring.                      |
| IoC Container      | Contenedor donde Spring almacena todos los Beans.             |
| Component Scanning | Proceso mediante el cual Spring busca clases anotadas.        |
| Sin `new`          | Spring crea y entrega automáticamente los objetos necesarios. |

---

## 🔙 [***Volver***](./implementacionalidacion.md)