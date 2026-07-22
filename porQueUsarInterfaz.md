## 🔙 [***Volver***](./implementacionalidacion.md)

---

# 🔌 ¿Por qué usar una Interfaz?

# El secreto del desacoplamiento en Spring Boot

En la sección anterior descubrimos que las validaciones no deberían vivir dentro del `Service`.

Por eso creamos una nueva idea:

```text
Servicio → Validador → Base de datos
```

Ahora aparece una nueva pregunta.

> 🤔 ¿Por qué crear una **interfaz** si podríamos hacer esto directamente?

```java
private JuegoValidadorImpl validador;
```

Y listo.

Parece más corto y funciona perfectamente.

Entonces…

¿Para qué complicarnos con interfaces?

La respuesta está en una palabra clave de la arquitectura de software.

---

# 🧠 Desacoplamiento

Desacoplar significa:

> **Que una clase no dependa de una implementación específica.**

Suena abstracto, así que usemos una analogía.

---

# 🔌 La Analogía del Enchufe Universal

Imagina la pared de tu casa.

Tiene un enchufe.

```text
🔌 Pared
```

Ahora conectamos un cargador Samsung.

```text
🔌 Pared → 📱 Samsung
```

Mañana lo desconectas y conectas un iPhone.

```text
🔌 Pared → 🍎 iPhone
```

Después conectas una laptop.

```text
🔌 Pared → 💻 Laptop
```

La pared nunca sabe qué aparato usarás.

Solo conoce una regla:

> "Todo lo que se conecte debe tener esta forma de enchufe."

Ese **contrato** es exactamente lo que representa una **interfaz** en Java.

---

# 🎮 Aplicándolo a nuestro Local de Videojuegos

Tenemos un servicio que registra videojuegos.

El servicio necesita validar información antes de guardar.

Podríamos conectarlo directamente a una clase concreta.

```text
JuegoService → JuegoValidadorImpl
```

Pero ahora el servicio queda amarrado a esa implementación para siempre.

Si algún día queremos cambiar la forma de validar, tendremos que modificar el servicio.

Eso es **acoplamiento**.

---

# ❌ La mala práctica: depender de una implementación

Veamos el código.

```java
@Service
public class JuegoService {

    private final JuegoValidadorImpl validador;

    public JuegoService(JuegoValidadorImpl validador) {
        this.validador = validador;
    }

}
```

A primera vista parece correcto.

Pero observa la dependencia.

```text
JuegoService
      ↓
JuegoValidadorImpl
```

El servicio conoce el nombre exacto de la clase.

Está completamente acoplado a ella.

---

# 🤯 ¿Qué pasa si mañana cambiamos la validación?

Imagina que ahora las validaciones deben venir desde otro sistema.

Creamos una nueva clase.

```java
public class JuegoValidadorExterno {
    // validaciones desde API externa
}
```

El servicio ya no funciona.

Debemos modificarlo.

```java
private final JuegoValidadorExterno validador;
```

Cada cambio obliga a tocar el servicio.

Eso viola un principio muy importante.

---

# 📚 Principio de Inversión de Dependencias

Uno de los principios SOLID dice:

> **Las clases de alto nivel no deben depender de clases concretas.**

> **Deben depender de abstracciones.**

En nuestro caso:

* `JuegoService` = clase de alto nivel.
* `JuegoValidadorImpl` = clase concreta.
* `IJuegoValidador` = abstracción.

La solución es hacer que el servicio dependa de la interfaz.

---

# ✅ La solución profesional

Creamos una interfaz.

```java
public interface IJuegoValidador {

    void validar(Juego juego);

}
```

Y ahora el servicio depende de esa interfaz.

```java
@Service
public class JuegoService {

    private final IJuegoValidador validador;

    public JuegoService(IJuegoValidador validador) {
        this.validador = validador;
    }

}
```

Observa el cambio.

Antes:

```text
JuegoService → JuegoValidadorImpl
```

Ahora:

```text
JuegoService → IJuegoValidador
```

El servicio ya no sabe qué clase valida.

Solo sabe que existe alguien capaz de ejecutar:

```java
validar(juego)
```

---

# 🏗️ La arquitectura completa

Así queda nuestra aplicación.

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
                   │
                   ▼
          JuegoValidadorImpl
                   │
                   ▼
            JuegoRepository
                   │
                   ▼
              Base de datos
```

Lo importante es que `JuegoService` **solo conoce la interfaz**.

No conoce la implementación.

---

# 🤔 ¿Y quién conecta todo?

Buena pregunta.

Si el servicio solo conoce la interfaz…

¿cómo obtiene la implementación real?

Aquí entra la magia de Spring Boot.

---

# ⚙️ Spring Boot busca la implementación automáticamente

Creamos la clase implementadora.

```java
@Component
public class JuegoValidadorImpl implements IJuegoValidador {

    @Override
    public void validar(Juego juego) {
        // reglas de validación
    }

}
```

Cuando la aplicación inicia, Spring hace algo parecido a esto.

```text
🔍 Escaneando proyecto...

Encontrado: JuegoValidadorImpl

Implementa: IJuegoValidador

✔ Registrado como Bean
```

Luego ve el constructor del servicio.

```java
public JuegoService(IJuegoValidador validador)
```

Y piensa:

> "Necesitan un `IJuegoValidador`."

> "Tengo una clase que lo implementa: `JuegoValidadorImpl`."

> "La inyectaré automáticamente."

Nosotros nunca escribimos:

```java
new JuegoValidadorImpl()
```

Spring lo hace por nosotros.

---

# 🧪 La prueba definitiva: cambiar la implementación

Supongamos que mañana queremos validar usando una API externa.

Creamos otra implementación.

```java
@Component
public class JuegoValidadorExternoImpl
        implements IJuegoValidador {

    @Override
    public void validar(Juego juego) {
        // validaciones externas
    }

}
```

¿Tuvimos que modificar `JuegoService`?

❌ No.

El servicio sigue funcionando porque depende de la interfaz.

Eso es desacoplamiento.

---

# 🧠 La idea que debes recordar para siempre

Cuando una clase depende de una **implementación**:

```text
Service → ClaseConcreta
```

El código queda rígido.

Cuando depende de una **interfaz**:

```text
Service → Interfaz
            ↑
   Implementación A
   Implementación B
   Implementación C
```

El código queda flexible.

Podemos cambiar implementaciones sin tocar el servicio.

---

# 🎒 Resumen

| Concepto            | Significado                                                         |
| ------------------- | ------------------------------------------------------------------- |
| **Acoplamiento**    | Una clase depende de una implementación concreta.                   |
| **Desacoplamiento** | Una clase depende de una abstracción (interfaz).                    |
| **Interfaz**        | Define el contrato de validación.                                   |
| **Implementación**  | Contiene la lógica real de las validaciones.                        |
| **Spring Boot**     | Busca automáticamente la implementación e inyecta el Bean correcto. |

---

## 🔙 [***Volver***](./implementacionalidacion.md)
