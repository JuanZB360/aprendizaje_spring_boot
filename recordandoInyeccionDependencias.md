## 🔙 [***Volver***](./implementacionalidacion.md)

---

# 💉 Inyección de Dependencias

# ¿Cómo sabe Spring qué objeto debe entregar?

En la sección anterior aprendimos que Spring crea automáticamente nuestros objetos y los guarda dentro del **IoC Container**.

Por ejemplo, cuando encuentra esta clase:

```java
@Component
public class JuegoValidadorImpl implements IJuegoValidador {

}
```

Spring hace algo parecido a esto de forma interna.

```java
JuegoValidadorImpl bean = new JuegoValidadorImpl();
```

Después guarda ese objeto dentro del contenedor.

Hasta aquí todo parece sencillo.

Pero ahora aparece una gran pregunta.

> 🤔 **Si nuestro `Service` solo conoce la interfaz... ¿cómo sabe Spring qué implementación debe utilizar?**

Vamos a descubrirlo paso a paso.

---

# 🤔 El problema

Recordemos nuestro servicio.

```java
@Service
@RequiredArgsConstructor
public class JuegoService {

    private final IJuegoValidador validador;

}
```

Observa con atención.

La variable es de tipo:

```java
IJuegoValidador
```

No es:

```java
JuegoValidadorImpl
```

Entonces...

¿Cómo termina llegando un objeto de `JuegoValidadorImpl` a esa variable?

---

# 👶 La Analogía del Local de Videojuegos

Volvamos una vez más a nuestro Local de Videojuegos.

Imagina que el gerente necesita un guardia de seguridad.

No dice:

> "Quiero a Juan."

Dice simplemente:

> "Necesito un guardia."

En Recursos Humanos revisan todos los empleados.

Encuentran que Juan es guardia.

Entonces responden.

> "Perfecto."

> "Aquí tienes a Juan."

Observa algo muy importante.

El gerente nunca pidió específicamente a Juan.

Solo pidió alguien que cumpliera el trabajo de guardia.

Eso mismo ocurre con las interfaces.

---

# 🧠 Traduciéndolo a Spring Boot

Nuestro servicio no pide una clase específica.

Solo dice:

```java
private final IJuegoValidador validador;
```

Es decir.

> "Necesito cualquier objeto que sea capaz de comportarse como un `IJuegoValidador`."

Spring revisa todos los Beans registrados.

Encuentra uno que cumple ese contrato.

```java
@Component
public class JuegoValidadorImpl
        implements IJuegoValidador {

}
```

Entonces lo entrega automáticamente.

---

# 🔍 Veamos el proceso completo

Cuando inicia la aplicación ocurre algo parecido a esto.

```text
               Spring Boot

                     │

                     ▼

      Escanea todas las clases del proyecto

                     │

                     ▼

 Encuentra JuegoValidadorImpl anotado con @Component

                     │

                     ▼

 Crea el Bean

                     │

                     ▼

 Detecta que implementa IJuegoValidador

                     │

                     ▼

 Registra esa relación en el contenedor
```

Ahora el contenedor sabe que:

```text
IJuegoValidador

        │

        ▼

JuegoValidadorImpl
```

Más adelante, cuando alguna clase solicite un `IJuegoValidador`, Spring ya sabe exactamente qué objeto debe entregar.

---

# 🏗️ El papel de `@RequiredArgsConstructor`

Ahora observemos nuevamente nuestro servicio.

```java
@Service
@RequiredArgsConstructor
public class JuegoService {

    private final IJuegoValidador validador;

}
```

Aquí no vemos ningún constructor.

Sin embargo, la clase funciona perfectamente.

¿Por qué?

Porque Lombok genera automáticamente este constructor.

```java
public JuegoService(IJuegoValidador validador){

    this.validador = validador;

}
```

Cuando Spring crea el `JuegoService`, utiliza ese constructor para inyectar todas las dependencias marcadas como `final`.

Pero todavía falta una pieza.

¿Quién envía ese objeto al constructor?

La respuesta es:

**el IoC Container.**

---

# 🔄 Lo que ocurre detrás de escena

Podemos imaginar el proceso así.

```text
JuegoService necesita:

↓

IJuegoValidador

↓

Spring busca un Bean compatible

↓

Encuentra:

JuegoValidadorImpl

↓

Lo entrega automáticamente

↓

JuegoService queda listo para trabajar
```

Todo este proceso ocurre sin que nosotros escribamos una sola línea de código para crear objetos.

---

# 🧠 ¿Y si existieran dos implementaciones?

Imaginemos que ahora tenemos dos clases.

```java
@Component
public class JuegoValidadorImpl
        implements IJuegoValidador{

}
```

y también.

```java
@Component
public class JuegoValidadorPremium
        implements IJuegoValidador{

}
```

Ahora Spring encuentra dos Beans que cumplen el mismo contrato.

```text
IJuegoValidador

├── JuegoValidadorImpl

└── JuegoValidadorPremium
```

Entonces aparece un problema.

Cuando el servicio solicite un `IJuegoValidador`...

Spring preguntará.

> 🤔 ¿Cuál de los dos debo entregar?

Como no puede decidir por sí solo, lanzará una excepción durante el inicio de la aplicación.

Este comportamiento evita que Spring haga suposiciones incorrectas.

Más adelante aprenderemos herramientas como:

* `@Primary`
* `@Qualifier`

que sirven precisamente para indicar qué implementación debe utilizarse cuando existen varias opciones.

---

# 💡 ¿Qué ventajas tiene este sistema?

Gracias a la Inyección de Dependencias obtenemos muchas ventajas.

## ✅ Bajo acoplamiento

El servicio no conoce ninguna implementación concreta.

Solo conoce el contrato.

---

## ✅ Fácil mantenimiento

Podemos reemplazar una implementación por otra sin modificar el servicio.

---

## ✅ Mayor reutilización

Varias clases pueden utilizar el mismo Bean.

No es necesario crear nuevos objetos constantemente.

---

## ✅ Código más limpio

Desaparecen los `new`.

Spring administra automáticamente el ciclo de vida de los objetos.

---

# 🎮 Nuestro flujo completo hasta ahora

Ya podemos visualizar toda la arquitectura que hemos construido.

```text
                Cliente

                   │

                   ▼

             JuegoController

                   │

                   ▼

              JuegoService

                   │

                   ▼

          IJuegoValidador
          (Contrato)

                   │

                   ▼

         JuegoValidadorImpl
          (@Component)

                   │

                   ▼

             IoC Container

                   │

                   ▼

           JuegoRepository

                   │

                   ▼

            Base de Datos
```

Observa que el `Service` nunca conoce directamente a `JuegoValidadorImpl`.

Toda la comunicación ocurre a través de la interfaz.

Ese es precisamente el objetivo del desacoplamiento.

---

# 🎒 Resumen

| Concepto                   | Función                                                                     |
| -------------------------- | --------------------------------------------------------------------------- |
| Inyección de Dependencias  | Spring entrega automáticamente los objetos que una clase necesita.          |
| `private final`            | Indica una dependencia obligatoria que será inyectada por el constructor.   |
| `@RequiredArgsConstructor` | Lombok genera automáticamente el constructor para las dependencias `final`. |
| IoC Container              | Busca el Bean que implementa la interfaz solicitada y lo inyecta.           |
| Interfaces                 | Permiten desacoplar el servicio de la implementación concreta.              |

---

# 🏆 Conclusión

Ahora ya entendemos uno de los conceptos más importantes de Spring Boot.

Cuando escribimos:

```java
private final IJuegoValidador validador;
```

No estamos creando ningún objeto.

Simplemente estamos declarando una dependencia.

Spring busca dentro del IoC Container un Bean que implemente esa interfaz y lo inyecta automáticamente utilizando el constructor generado por Lombok.

Gracias a este mecanismo podemos construir aplicaciones desacopladas, fáciles de mantener y preparadas para crecer.

---

## 🔙 [***Volver***](./implementacionalidacion.md)