## 🔙 [***Volver***](./implementacionalidacion.md)

---

# 🤔 ¿Qué ocurre cuando existen varias implementaciones?

Hasta ahora nuestro proyecto tiene una única implementación de la interfaz.

```java
public interface IJuegoValidador {

    void validar(Juego juego);

}
```

Y una única clase que cumple ese contrato.

```java
@Component
public class JuegoValidadorImpl implements IJuegoValidador {

    @Override
    public void validar(Juego juego) {

        // Lógica de validación

    }

}
```

Cuando nuestro `JuegoService` necesita un `IJuegoValidador`, Spring no tiene ninguna duda.

Solo existe una implementación.

Entonces simplemente la entrega.

```text
IJuegoValidador
        │
        ▼
JuegoValidadorImpl
```

Todo funciona perfectamente.

---

# 🤔 Pero... ¿qué pasa cuando el proyecto crece?

Imaginemos que después de unos meses nuestro sistema comienza a evolucionar.

Ahora necesitamos dos tipos de validadores.

* Un validador para los usuarios normales.
* Otro validador para los administradores.

Creamos dos clases diferentes.

```java
@Component
public class JuegoValidadorImpl implements IJuegoValidador {

    @Override
    public void validar(Juego juego) {

    }

}
```

Y también.

```java
@Component
public class JuegoValidadorPremium implements IJuegoValidador {

    @Override
    public void validar(Juego juego) {

    }

}
```

Ahora Spring encuentra esto.

```text
IJuegoValidador

├── JuegoValidadorImpl

└── JuegoValidadorPremium
```

Y aquí aparece un problema muy importante.

---

# 🤔 ¿Cuál de los dos Beans debe entregar Spring?

Nuestro servicio sigue siendo exactamente el mismo.

```java
@Service
@RequiredArgsConstructor
public class JuegoService {

    private final IJuegoValidador validador;

}
```

Observa algo interesante.

Nosotros no estamos pidiendo una clase concreta.

Solo estamos diciendo:

> "Necesito cualquier objeto que implemente `IJuegoValidador`."

Pero ahora Spring encuentra dos candidatos.

```text
✔ JuegoValidadorImpl

✔ JuegoValidadorPremium
```

Entonces Spring se pregunta.

> 🤔 ¿Cuál de los dos debo utilizar?

---

# ❌ Spring NO elige al azar

Muchas personas creen que Spring selecciona automáticamente la primera implementación que encuentra.

Eso es falso.

Spring nunca hace suposiciones.

Si existen varias implementaciones y ninguna tiene prioridad, la aplicación ni siquiera iniciará.

En su lugar lanzará una excepción similar a esta.

```text
No qualifying bean of type
'IJuegoValidador'

expected single matching bean
but found 2
```

Con este mensaje Spring nos está diciendo:

> "Encontré dos Beans que cumplen el contrato."

> "Pero tú nunca me dijiste cuál debo utilizar."

---

# 👶 La Analogía del Local de Videojuegos

Volvamos a nuestro Local de Videojuegos.

Imagina que el gerente necesita un guardia de seguridad.

Llama a Recursos Humanos y dice.

> "Necesito un guardia."

Pero Recursos Humanos responde.

```text
Tenemos dos guardias disponibles.

👮 Juan

👮 Pedro
```

El gerente nunca dijo cuál quería.

Entonces Recursos Humanos no puede decidir por él.

En lugar de enviar a cualquiera, responde.

> "Debes indicarme exactamente cuál necesitas."

Eso mismo hace Spring.

Si encuentra varias implementaciones del mismo contrato, espera que el desarrollador tome la decisión.

---

# ⭐ Solución 1: `@Primary`

La primera forma de resolver este problema consiste en indicar cuál será la implementación principal.

Para ello utilizamos la anotación:

```java
@Primary
```

Ejemplo.

```java
@Component
@Primary
public class JuegoValidadorImpl implements IJuegoValidador {

}
```

Mientras que la otra implementación permanece igual.

```java
@Component
public class JuegoValidadorPremium implements IJuegoValidador {

}
```

Ahora Spring entiende la siguiente jerarquía.

```text
IJuegoValidador

├── ⭐ JuegoValidadorImpl (@Primary)

└── JuegoValidadorPremium
```

Cada vez que alguien solicite un `IJuegoValidador`, Spring utilizará automáticamente la implementación marcada con `@Primary`.

---

# 🧠 ¿Cuándo utilizar `@Primary`?

`@Primary` es una excelente opción cuando:

* existen varias implementaciones;
* una de ellas será la más utilizada;
* y deseas que Spring la seleccione automáticamente.

Por ejemplo.

```text
IEmailService

├── ⭐ GmailService

├── OutlookService

└── YahooService
```

La mayoría de la aplicación utilizará `GmailService`.

Solo en casos especiales será necesario utilizar las demás implementaciones.

---

# ⭐ Solución 2: `@Qualifier`

A veces no queremos una implementación principal.

Queremos decidir manualmente cuál utilizar en cada clase.

Para eso existe:

```java
@Qualifier
```

Primero asignamos un nombre a cada Bean.

```java
@Component("basico")
public class JuegoValidadorImpl implements IJuegoValidador {

}
```

Y después.

```java
@Component("premium")
public class JuegoValidadorPremium implements IJuegoValidador {

}
```

Ahora podemos indicar exactamente cuál queremos inyectar.

```java
@Service
@RequiredArgsConstructor
public class JuegoService {

    @Qualifier("premium")
    private final IJuegoValidador validador;

}
```

En este caso Spring buscará el Bean llamado `"premium"` y lo inyectará en el servicio.

---

# 🧠 ¿Qué ocurre internamente?

Podemos imaginar el proceso así.

```text
JuegoService

↓

Solicita:

IJuegoValidador

↓

Spring revisa el IoC Container

↓

Encuentra dos Beans

↓

¿Existe @Qualifier?

        │

     Sí │ No

        ▼

Usa el Bean indicado

        │

        ▼

¿Existe @Primary?

        │

     Sí │ No

        ▼

Usa @Primary

        │

        ▼

Si no existe ninguno

↓

❌ Lanza una excepción
```

Este comportamiento hace que Spring sea muy seguro.

Nunca tomará una decisión que pueda producir resultados inesperados.

---

# 📌 ¿Cuál debería utilizar?

En la mayoría de proyectos encontrarás uno de estos tres escenarios.

## ✅ Solo existe una implementación

No necesitas hacer nada.

```text
Interfaz

↓

Implementación
```

Spring la encontrará automáticamente.

---

## ✅ Existen varias implementaciones, pero una será la principal

Utiliza:

```java
@Primary
```

Spring seleccionará esa implementación por defecto.

---

## ✅ Existen varias implementaciones y quieres decidir cuál utilizar

Utiliza:

```java
@Qualifier
```

Así podrás elegir explícitamente qué Bean deseas inyectar.

---

# 🎒 Resumen

| Concepto                       | Función                                                                                                                                  |
| ------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------- |
| Varias implementaciones        | Spring necesita saber cuál utilizar.                                                                                                     |
| Sin `@Primary` ni `@Qualifier` | Spring lanza una excepción porque encuentra varios Beans compatibles.                                                                    |
| `@Primary`                     | Define la implementación predeterminada.                                                                                                 |
| `@Qualifier`                   | Permite elegir explícitamente qué Bean debe inyectarse.                                                                                  |
| Buenas prácticas               | Utiliza `@Primary` cuando exista una implementación principal y `@Qualifier` cuando necesites seleccionar una implementación específica. |

---

## 🔙 [***Volver***](./implementacionalidacion.md)