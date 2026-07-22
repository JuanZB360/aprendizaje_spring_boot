## 🔙 [***Volver***](./implementacionalidacion.md)

---

# 🛠️ Construyendo nuestro primer método `validar()`

# La puerta de entrada de todas las reglas de negocio

Ya sabemos que nuestro objetivo es impedir que información incorrecta llegue a la base de datos.

Para lograrlo crearemos un único punto de entrada para todas las validaciones.

Ese punto de entrada será el método:

```java
void validar(Juego juego);
```

Aunque parece un método muy sencillo, desde aquí comenzará todo el proceso de validación.

---

# 🎮 Volvamos al Local de Videojuegos

Imagina nuevamente que un cliente llega al mostrador para registrar un juego.

El empleado no revisa toda la información al mismo tiempo.

Lo hace paso a paso.

Primero mira el título.

Después revisa el precio.

Luego verifica la consola.

Finalmente comprueba si el juego ya existe.

Podemos imaginar el proceso así.

```text
Cliente entrega la información

            │

            ▼

      Empleado recibe el formulario

            │

            ▼

 ¿El título es válido?

            │

            ▼

 ¿El precio es válido?

            │

            ▼

 ¿La consola es válida?

            │

            ▼

 ¿El juego ya existe?

            │

            ▼

 Todo correcto

            │

            ▼

 Se registra el juego
```

Nuestro método `validar()` hará exactamente ese trabajo.

---

# 🧠 Una validación no es una única condición

Cuando alguien comienza con Spring Boot suele escribir algo parecido a esto.

```java
@Override
public void validar(Juego juego){

    if(juego.getTitulo() == null){
        ...
    }

    if(juego.getPrecio() <= 0){
        ...
    }

    if(juego.getConsola() == null){
        ...
    }

    if(repository.existsByTitulo(juego.getTitulo())){
        ...
    }

}
```

Aunque este código funciona...

Conforme el proyecto crezca se volverá difícil de leer.

Imagina un sistema con veinte o treinta reglas de negocio.

Ese método terminaría teniendo cientos de líneas.

---

# 💡 Una mejor forma de organizar nuestras validaciones

En lugar de colocar todas las reglas dentro del mismo método, podemos dividir cada validación en un método independiente.

Así cada método tendrá una única responsabilidad.

Nuestro método principal quedaría así.

```java
@Override
public void validar(Juego juego){

    validarTitulo(juego);

    validarPrecio(juego);

    validarConsola(juego);

    validarJuegoDuplicado(juego);

}
```

Ahora el código es mucho más fácil de entender.

Incluso sin leer la implementación ya sabemos exactamente qué está validando.

---

# 👶 La Analogía del Inspector

Imagina que un inspector debe revisar un nuevo local antes de autorizar su apertura.

No intenta comprobar todo al mismo tiempo.

Tiene una lista de verificación.

```text
☐ Revisar electricidad

☐ Revisar agua

☐ Revisar extintores

☐ Revisar salidas de emergencia

☐ Revisar iluminación
```

Va marcando cada punto uno por uno.

Si alguno falla...

Detiene inmediatamente la inspección.

Nuestro método `validar()` funciona exactamente igual.

Cada método representa una revisión específica.

---

# 🏗️ Construyendo la estructura del validador

Nuestra implementación comenzará así.

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

Observa que todavía no hemos implementado ninguna regla.

Por ahora solo estamos definiendo el recorrido que seguirá la validación.

---

# 📍 ¿Por qué dividir las reglas?

Separar las validaciones tiene muchas ventajas.

## ✅ Código más limpio

Cada método realiza una única tarea.

---

## ✅ Fácil mantenimiento

Si mañana cambia la regla del precio, solo modificamos `validarPrecio()`.

No es necesario tocar las demás validaciones.

---

## ✅ Mayor reutilización

Una misma regla puede utilizarse desde diferentes partes de la aplicación.

---

## ✅ Más fácil de probar

Podemos crear pruebas unitarias para cada regla de forma independiente.

---

# 🔍 ¿Qué hace realmente `validar()`?

Es importante entender que el método `validar()` no valida directamente.

Su verdadera función es coordinar el proceso.

Podemos verlo como un director de orquesta.

```text
validar()

│

├── validarTitulo()

├── validarPrecio()

├── validarConsola()

└── validarJuegoDuplicado()
```

Cada método especializado hace su trabajo.

El método principal únicamente organiza el flujo.

---

# 🎯 Nuestro siguiente paso

Ya tenemos la estructura del validador.

Ahora debemos enseñar a cada método cómo detectar un error.

Por ejemplo.

```java
private void validarTitulo(Juego juego){

    // Aquí verificaremos que el título sea válido

}
```

Pero aparece una nueva pregunta.

> 🤔 ¿Qué debe hacer el sistema cuando encuentra un error?

¿Debe devolver `false`?

¿Debe devolver `null`?

¿Debe imprimir un mensaje por consola?

La respuesta es **no**.

Spring Boot utiliza un mecanismo mucho más elegante.

Las **excepciones**.

Y la excepción que utilizaremos durante toda nuestra aplicación será:

```java
ResponseStatusException
```

---

# 🎒 Resumen

| Concepto              | Explicación                                                                        |
| --------------------- | ---------------------------------------------------------------------------------- |
| `validar()`           | Es el punto de entrada de todas las validaciones.                                  |
| Una regla = un método | Cada validación debe tener una única responsabilidad.                              |
| Método principal      | Organiza el flujo de validación, no realiza todas las comprobaciones directamente. |
| Beneficio             | Código limpio, reutilizable, fácil de mantener y de probar.                        |

---

# 🏆 Conclusión

Ya construimos el esqueleto de nuestro validador.

Todavía no estamos comprobando ninguna regla de negocio, pero hemos definido una estructura profesional que podrá crecer sin convertirse en un método enorme y difícil de mantener.

---

## 🔙 [***Volver***](./implementacionalidacion.md)