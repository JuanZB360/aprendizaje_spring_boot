## 🔙 [***Volver***](./implementacionalidacion.md)

---

# 🛡️ Validaciones Propias en Spring Boot

# Construyendo un Sistema de Validación Profesional

Hasta ahora hemos aprendido a crear entidades, relaciones entre tablas y servicios que interactúan con la base de datos.

Sin embargo, todavía existe un problema muy común que aparece en prácticamente todas las aplicaciones.

> **¿Cómo evitamos guardar información incorrecta en nuestra base de datos?**

Por ejemplo:

* Un usuario sin nombre.
* Un producto con precio negativo.
* Un correo electrónico con un formato inválido.
* Un videojuego cuyo nombre ya existe.
* Un cliente que intenta registrarse dos veces con el mismo documento.

Todas estas situaciones deben ser validadas antes de guardar la información.

En este capítulo aprenderemos a construir un sistema de validaciones profesional utilizando:

* Interfaces.
* Clases implementadoras.
* Inyección de dependencias.
* Desacoplamiento.
* `ResponseStatusException`.

Al finalizar comprenderás cómo diseñar validadores reutilizables siguiendo buenas prácticas de arquitectura utilizadas en proyectos reales de Spring Boot.

---

# 🎮 Nuestro ejemplo

Como en los capítulos anteriores, utilizaremos el mismo ejemplo para que todo sea más fácil de entender.

Imagina que estamos desarrollando el sistema para un **Local de Videojuegos**.

Nuestro sistema permitirá registrar nuevos videojuegos.

Cada videojuego tendrá la siguiente información:

* Nombre.
* Consola.
* Precio.

Pero antes de guardar un juego queremos verificar que cumpla varias reglas.

Por ejemplo:

* El nombre no puede estar vacío.
* El precio debe ser mayor que cero.
* La consola debe existir.
* No puede existir otro juego con el mismo nombre.

Todas estas reglas forman parte de las **validaciones de negocio**.

---

# 🤔 El problema

Cuando comenzamos a aprender Spring Boot normalmente escribimos todas las validaciones directamente dentro del servicio.

Algo parecido a esto.

```java
@Service
public class JuegoService {

    public Juego guardar(Juego juego){

        if(juego.getNombre() == null || juego.getNombre().isBlank()){
            throw new ResponseStatusException(
                HttpStatus.BAD_REQUEST,
                "El nombre es obligatorio."
            );
        }

        if(juego.getPrecio() <= 0){
            throw new ResponseStatusException(
                HttpStatus.BAD_REQUEST,
                "El precio debe ser mayor que cero."
            );
        }

        if(juego.getConsola() == null){
            throw new ResponseStatusException(
                HttpStatus.BAD_REQUEST,
                "Debe seleccionar una consola."
            );
        }

        return repository.save(juego);

    }

}
```

A primera vista parece una buena solución.

De hecho...

¡Funciona!

Si el usuario envía información incorrecta, el sistema lanzará una excepción y evitará guardar los datos.

Entonces...

¿Dónde está el problema?

---

# 😕 Imaginemos que el proyecto crece...

Supongamos que nuestro sistema ahora tiene muchos servicios.

```text
JuegoService

ProductoService

ClienteService

EmpleadoService

ProveedorService

FacturaService

PedidoService
```

Todos necesitan validar información.

Muy pronto cada servicio comenzará a llenarse de condiciones `if`.

```text
JuegoService

↓

if (...)

↓

if (...)

↓

if (...)

↓

if (...)

↓

repository.save(...)
```

Lo mismo ocurrirá con los demás servicios.

Con el tiempo tendrás cientos de líneas de validaciones mezcladas con la lógica de negocio.

---

# ❌ ¿Qué problemas aparecen?

Aunque el código funciona, comienzan a surgir varios inconvenientes.

## 📌 Código difícil de leer

Ahora el servicio tiene dos responsabilidades distintas:

* validar datos;
* realizar operaciones sobre la base de datos.

Esto hace que el código sea más largo y más difícil de entender.

---

## 📌 Código repetido

Imagina que necesitas validar que el nombre de un videojuego no esté vacío en cinco lugares diferentes.

Probablemente terminarás copiando el mismo bloque de código una y otra vez.

Si algún día la regla cambia, tendrás que modificarla en todos esos lugares.

---

## 📌 Difícil de mantener

Cada nueva regla hace crecer el servicio.

Lo que antes ocupaba unas pocas líneas ahora puede convertirse en cientos de líneas difíciles de mantener.

---

## 📌 Poco reutilizable

Si otro servicio necesita las mismas validaciones, no puede reutilizarlas fácilmente.

La única opción será copiar y pegar código.

Y copiar código casi siempre termina generando errores.

---

# 💡 Necesitamos una mejor solución

Como ya aprendimos en capítulos anteriores, una clase debería tener una única responsabilidad.

Entonces surge una pregunta.

> **¿Y si existiera una clase cuya única misión fuera validar?**

Una clase que:

* no guarde datos;
* no consulte la base de datos;
* no genere reportes;
* no envíe correos.

Que simplemente reciba información...

La revise...

Y diga si cumple o no las reglas del negocio.

Eso es exactamente lo que construiremos en este capítulo.

---

# 👮 La Analogía del Guardia de Seguridad

Imagina nuevamente nuestro Local de Videojuegos.

Cuando llega un cliente nuevo para registrarse, no entra directamente.

Primero debe pasar por un guardia de seguridad.

El guardia revisa varias cosas.

✅ Que tenga documento.

✅ Que la información esté completa.

✅ Que no exista otro cliente con el mismo documento.

Si todo está correcto...

El cliente puede ingresar.

Pero si alguna regla falla...

El guardia lo detiene inmediatamente y no le permite continuar.

Lo importante es que el guardia **no registra clientes**.

Tampoco vende videojuegos.

No administra consolas.

No cobra dinero.

Su única responsabilidad es validar.

---

# 🧠 Relacionándolo con Spring Boot

En nuestra aplicación ocurre exactamente lo mismo.

El Servicio será quien coordina el proceso.

Pero antes de guardar cualquier información, enviará los datos al **Validador**.

El Validador revisará todas las reglas del negocio.

Si encuentra un problema, lanzará una excepción y el proceso terminará.

Si todo está correcto, devolverá el control al Servicio para que continúe guardando la información.

En los próximos apartados construiremos paso a paso ese sistema de validación profesional utilizando interfaces, clases implementadoras e inyección de dependencias.

---

## 🔙 [***Volver***](./implementacionalidacion.md)