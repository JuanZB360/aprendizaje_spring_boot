## 🔙 [***Volver***](./servicios.md)

---

# 🔌 ¿Qué es la Inyección de Dependencias?

## 👶 Explicación para niños

Imagina que el **Administrador del Local** (`Service`) necesita hablar constantemente con el **Recepcionista** (`Repository`).

Sin un Recepcionista, el Administrador no puede:

* guardar juegos,
* buscar información,
* actualizar registros,
* ni consultar la base de datos.

La pregunta es:

> ¿Cómo le entregamos el Recepcionista al Administrador?

A ese proceso se le conoce como:

**Inyección de Dependencias (Dependency Injection).**

Spring Boot se encarga de conectar automáticamente los componentes que necesitan trabajar juntos.

---

# 🕰️ El Pasado: ¿Cómo funcionaba `@Autowired`?

## 📌 Inyección por Campo (Field Injection)

Antes, para conectar al Administrador con el Recepcionista, hacíamos esto:

```java
@Service
public class JuegoService {

    @Autowired
    private JuegoRepository juegoRepository;

}
```

---

## 👶 Explicación para niños

Imagina que el Administrador tiene en su oficina un radio de comunicación llamado:

```java
juegoRepository
```

Cuando le colocamos la etiqueta:

```java
@Autowired
```

Le estamos diciendo a Spring Boot:

> "Oye Spring, cuando abras el local, entra en secreto a la oficina, conecta el radio por detrás y déjalo funcionando."

El Administrador nunca recibe el radio directamente.

Spring entra después y realiza la conexión usando un mecanismo interno llamado **Reflexión (Reflection)**.

---

# ❌ ¿Por qué dejó de recomendarse?

Aunque funciona, tiene varios problemas.

---

## ⚠️ Problema 1: El Teléfono Desconectado

Imagina que quieres probar al Administrador sin abrir todo el local.

```java
JuegoService service = new JuegoService();
```

El problema es que nadie conectó el radio.

Entonces:

```java
service.guardarJuego();
```

produce un error porque:

```java
juegoRepository == null
```

Spring no estuvo presente para hacer la conexión.

---

## ⚠️ Problema 2: Dependencias Ocultas

Cuando miras la clase rápidamente:

```java
@Service
public class JuegoService {
    @Autowired
    private JuegoRepository juegoRepository;
}
```

No es evidente qué necesita realmente para funcionar.

Parece que puede existir sola, pero en realidad depende completamente del repositorio.

---

## ⚠️ Problema 3: No permite usar `final`

Como Spring conecta el repositorio después de crear el objeto, la variable no puede declararse como:

```java
private final JuegoRepository juegoRepository;
```

Esto significa que alguien podría modificar la referencia accidentalmente durante la ejecución.

---

# 🚀 El Presente: Inyección por Constructor

La comunidad Java concluyó que la mejor forma de conectar objetos era hacerlo desde el momento de su creación.

Por eso apareció la Inyección por Constructor.

---

## 🛠️ Ejemplo

```java
@Service
public class JuegoService {

    private final JuegoRepository juegoRepository;

    public JuegoService(JuegoRepository juegoRepository) {
        this.juegoRepository = juegoRepository;
    }
}
```

---

## 👶 Explicación para niños

Ahora el Administrador es más exigente.

Antes decía:

> "Entren después a conectarme el radio."

Ahora dice:

> "Yo no abro mi oficina si no me entregan el radio en la puerta."

El constructor se convierte en la entrada obligatoria.

Si no recibe el repositorio, simplemente no puede existir.

---

# 🔍 ¿Qué ventajas tiene?

## ✅ Dependencias visibles

Al ver el constructor:

```java
public JuegoService(JuegoRepository juegoRepository)
```

Es evidente que necesita un repositorio para funcionar.

No hay magia oculta.

---

## ✅ Permite usar `final`

```java
private final JuegoRepository juegoRepository;
```

Una vez conectado, nadie puede reemplazarlo.

Queda congelado para siempre.

---

## ✅ Facilita las pruebas

Ahora puedes crear servicios manualmente:

```java
JuegoRepository repoFalso = new JuegoRepositoryFake();

JuegoService service =
        new JuegoService(repoFalso);
```

No necesitas arrancar Spring Boot completo para probar tu lógica.

---

# ✨ La Magia Moderna: `@RequiredArgsConstructor`

Escribir constructores a mano funciona.

Pero si tienes muchos repositorios:

```java
private final JuegoRepository juegoRepository;
private final LocalRepository localRepository;
private final ClienteRepository clienteRepository;
private final FacturaRepository facturaRepository;
```

el constructor empieza a crecer demasiado.

Aquí es donde entra Lombok.

---

## 🛠️ Ejemplo Moderno

```java
@Service
@RequiredArgsConstructor
public class JuegoService {

    private final JuegoRepository juegoRepository;
    private final LocalRepository localRepository;

}
```

---

## 🔍 ¿Qué hace `@RequiredArgsConstructor`?

Lombok revisa la clase.

Cuando encuentra variables marcadas como:

```java
final
```

genera automáticamente el constructor por detrás.

Es como si escribiera esto por ti:

```java
public JuegoService(
        JuegoRepository juegoRepository,
        LocalRepository localRepository) {

    this.juegoRepository = juegoRepository;
    this.localRepository = localRepository;
}
```

Pero sin que tengas que escribirlo.

---

## 👶 Explicación para niños

Imagina que Lombok es un asistente invisible.

Cada vez que ve variables importantes marcadas con:

```java
final
```

dice:

> "No te preocupes jefe, yo construyo la puerta de entrada automáticamente."

Y genera el constructor mientras compilas el proyecto.

---

# 🏆 ¿Por qué `@RequiredArgsConstructor` es la mejor opción?

| Característica            | `@Autowired`           | `@RequiredArgsConstructor` |
| ------------------------- | ---------------------- | -------------------------- |
| Seguridad (`final`)       | ❌ No                   | ✅ Sí                       |
| Testing                   | ❌ Difícil              | ✅ Muy fácil                |
| Transparencia             | ❌ Dependencias ocultas | ✅ Dependencias visibles    |
| Mantenimiento             | ❌ Más complejo         | ✅ Más limpio               |
| Buenas prácticas actuales | ❌ Antiguo              | ✅ Recomendado              |

---

# 💡 Idea Clave

La Inyección de Dependencias consiste en entregar automáticamente los componentes que una clase necesita para funcionar.

Antes se hacía con:

```java
@Autowired
```

Hoy la forma recomendada es:

```java
@RequiredArgsConstructor
```

porque genera código más seguro, más limpio, más fácil de probar y más fácil de mantener.

---

## 🎒 Resumen para tu mochila de Senior

* Un Servicio necesita otros componentes para trabajar.
* Spring Boot puede conectarlos automáticamente.
* `@Autowired` fue la forma tradicional.
* La Inyección por Constructor es la práctica moderna.
* `@RequiredArgsConstructor` genera automáticamente el constructor necesario.
* Las dependencias se declaran como `final` para hacer el código más seguro.
* Es la forma recomendada actualmente en proyectos profesionales de Spring Boot.

---

## 🔙 [***Volver***](./servicios.md)