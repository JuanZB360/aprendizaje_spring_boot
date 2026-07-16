## 🔙 [***Volver***](./servicios.md)

---

# 🛡️ La Etiqueta @Transactional (El Escudo Mágico de la Base de Datos)

Después de aprender a crear servicios e inyectar dependencias, llega una de las herramientas más importantes de Spring Boot para proteger la información de nuestro sistema: **@Transactional**.

Vamos a destripar esta etiqueta en varias partes utilizando nuestro **Local de Videojuegos y Dulces**, para que entiendas no solamente cómo funciona, sino también cuándo utilizarla en proyectos reales.

---

# 🎯 ¿Qué es y para qué sirve @Transactional?

En el mundo de las bases de datos, una **Transacción** es un conjunto de acciones que deben ejecutarse como una sola operación.

La regla es simple:

> **O se ejecutan todas correctamente o no se ejecuta ninguna.**

No existe un punto intermedio.

---

## 👶 Explicación para niños

Imagina que un niño llega al mostrador del local para comprar un videojuego que cuesta **$50**.

El Administrador (Servicio) debe realizar dos tareas obligatorias:

### Paso 1
Quitarle los **$50** de su tarjeta.

### Paso 2
Restar **1 unidad** del inventario del juego.

Ahora imagina que ocurre esto:

✅ Se descuentan los $50.

❌ Se va la luz antes de descontar el juego del inventario.

Resultado:

El niño perdió su dinero.

Y además sigue apareciendo el juego en la estantería.

¡Un desastre!

---

## ✨ Aquí aparece @Transactional

Cuando colocamos esta etiqueta sobre un método, Spring Boot activa un escudo mágico.

Si alguno de los pasos falla:

- Devuelve el dinero.
- Cancela los cambios.
- Deja todo exactamente como estaba.

Como si nada hubiera ocurrido.

A este proceso se le conoce como:

### 🔄 Rollback (Volver atrás)

---

# ⚙️ ¿Cómo funciona por debajo? (El Truco del Proxy)

Cuando escribimos:

```java
@Transactional
public void comprarJuego() {
    // lógica
}
```

Spring no ejecuta ese método directamente.

Primero crea un ayudante invisible llamado:

### Proxy

Este asistente se encarga de vigilar todo lo que ocurre dentro del método.

---

## 🧩 El flujo completo

### 1️⃣ El asistente abre la transacción

Antes de ejecutar tu código:

```text
"Hola Base de Datos,
voy a iniciar una transacción."
```

---

### 2️⃣ Se ejecuta tu código

Aquí ocurren operaciones como:

- INSERT
- UPDATE
- DELETE

---

### 3️⃣ El asistente revisa el resultado

Si todo salió bien:

### ✅ Commit

```text
"Todo perfecto.
Guarda los cambios para siempre."
```

La base de datos almacena la información de manera definitiva.

---

Si algo falla:

### ❌ Rollback

```text
"¡Alerta!
Borra todo lo que hicimos."
```

La base de datos elimina todos los cambios realizados durante esa transacción.

---

# 🎮 Ejemplo Real del Local

Imagina este proceso:

```java
@Transactional
public void comprarJuego() {

    descontarDinero();

    actualizarInventario();

    generarFactura();
}
```

Si falla cualquiera de estos métodos:

```java
actualizarInventario();
```

entonces:

```java
descontarDinero();
```

también será revertido.

---

## Resultado

Antes:

```text
Dinero = $100
Inventario = 10
```

Durante:

```text
Dinero = $50
Inventario = ERROR
```

Después del Rollback:

```text
Dinero = $100
Inventario = 10
```

Todo vuelve a su estado original.

---

# 🚀 Todos los atributos importantes de @Transactional

La anotación puede recibir varios parámetros que modifican el comportamiento de la transacción.

---

# A. readOnly (Modo Lectura)

## ¿Qué hace?

Le indica a la base de datos que únicamente vamos a leer información.

No vamos a modificar nada.

---

## Uso

```java
@Transactional(readOnly = true)
```

---

## 👶 Mente de niño

Es como entrar al local únicamente para mirar los videojuegos en las vitrinas.

No vas a comprar.

No vas a mover nada.

Solo observar.

---

## Beneficios

La base de datos:

- Consume menos recursos.
- Evita bloqueos innecesarios.
- Responde más rápido.

---

## Ejemplo

```java
@Transactional(readOnly = true)
public List<Juego> listarJuegos() {
    return juegoRepository.findAll();
}
```

---

## 💡 Regla de Senior

Siempre que un método únicamente consulte datos:

- findAll()
- findById()
- búsquedas
- reportes

utiliza:

```java
@Transactional(readOnly = true)
```

---

# B. rollbackFor y noRollbackFor

## ¿Por qué existen?

Por defecto Spring solo hace Rollback cuando ocurre una:

```java
RuntimeException
```

o una excepción no controlada.

---

## Problema

Si ocurre una excepción normal:

```java
Exception
```

Spring podría guardar cambios parcialmente.

---

## Solución

Obligar a Spring a hacer Rollback ante cualquier error.

---

## Uso recomendado

```java
@Transactional(
    rollbackFor = Exception.class
)
```

---

## 👶 Mente de niño

Le dices al guardia:

```text
"Si escuchas cualquier tipo de alarma,
no importa cuál sea,
devuelve todo a como estaba."
```

---

# C. propagation (Reglas entre transacciones)

Controla qué ocurre cuando un método transaccional llama a otro método transaccional.

---

## 1️⃣ Propagation.REQUIRED

### Valor por defecto

```java
@Transactional(
    propagation = Propagation.REQUIRED
)
```

---

### ¿Qué hace?

Si ya existe una transacción:

👉 Se une a ella.

Si no existe:

👉 Crea una nueva.

---

### 👶 Mente de niño

Todos se suben al mismo autobús.

Si el autobús choca:

Todos vuelven atrás.

---

### Uso real

Es el comportamiento utilizado el 95% del tiempo.

---

## 2️⃣ Propagation.REQUIRES_NEW

```java
@Transactional(
    propagation = Propagation.REQUIRES_NEW
)
```

---

### ¿Qué hace?

Ignora la transacción actual.

Crea una completamente nueva.

---

### 👶 Mente de niño

El Administrador está cobrando un juego.

Pero además debe registrar una auditoría.

Entonces:

- La compra viaja en un autobús.
- La auditoría viaja en otro autobús.

Si la compra falla:

La auditoría puede seguir existiendo.

---

### Ejemplo típico

```text
Compra de producto
↓
Guardar Log de Auditoría
↓
Enviar Notificación
```

---

## 3️⃣ Propagation.MANDATORY

```java
@Transactional(
    propagation = Propagation.MANDATORY
)
```

---

### ¿Qué hace?

Exige que ya exista una transacción abierta.

Si no existe:

💥 Explota inmediatamente.

---

### 👶 Mente de niño

Es como un empleado que dice:

```text
"No trabajo solo.
Solo entro si ya hay un jefe trabajando."
```

---

# D. isolation (Nivel de aislamiento)

Controla cuánto puede ver una transacción de lo que hacen otras transacciones al mismo tiempo.

---

## ¿Por qué es importante?

Imagina que queda:

```text
1 sola copia de Mario Kart
```

Dos clientes intentan comprarla exactamente al mismo tiempo.

Sin aislamiento:

Ambos podrían comprar el mismo juego.

---

# Isolation.READ_COMMITTED

## Valor por defecto

```java
Isolation.READ_COMMITTED
```

---

### ¿Qué hace?

Solo permite leer datos que ya fueron confirmados mediante Commit.

---

### Beneficio

Evita las llamadas:

### Lecturas Sucias (Dirty Reads)

---

## 👶 Mente de niño

Solo puedes leer los apuntes que ya fueron escritos con tinta permanente.

No puedes leer borradores.

---

# Isolation.SERIALIZABLE

## El nivel máximo

```java
Isolation.SERIALIZABLE
```

---

### ¿Qué hace?

Obliga a que las transacciones se ejecuten una detrás de otra.

---

### 👶 Mente de niño

Una fila india perfecta.

```text
Cliente 1
↓
Cliente 2
↓
Cliente 3
```

Nadie puede adelantarse.

---

### Ventaja

Máxima seguridad.

---

### Desventaja

Puede volver lenta la aplicación.

---

# 🛠️ Ejemplo de un Servicio Senior

```java
@Service
@RequiredArgsConstructor
public class JuegoService {

    private final JuegoRepository juegoRepository;

    // Consulta optimizada
    @Transactional(readOnly = true)
    public List<Juego> listarJuegos() {
        return juegoRepository.findAll();
    }

    // Operación crítica
    @Transactional(
        rollbackFor = Exception.class
    )
    public Juego comprarJuego(
            Long juegoId,
            Long clienteId
    ) throws Exception {

        // Lógica de negocio
        // Descontar dinero
        // Actualizar inventario
        // Generar factura

        return juegoRepository.save(juego);
    }
}
```

---

# 🎒 Resumen para tu Mochila de Senior

### @Transactional

Convierte un conjunto de operaciones en una única unidad de trabajo.

> Todo se guarda o nada se guarda.

---

### readOnly = true

Tu mejor amigo para consultas.

Más rápido.

Más eficiente.

Menos consumo de recursos.

---

### rollbackFor = Exception.class

Tu escudo de protección.

Garantiza que cualquier error revierta todos los cambios realizados.

---

### propagation

Controla cómo se relacionan las transacciones entre sí.

- REQUIRED → Se une a la existente.
- REQUIRES_NEW → Crea una nueva.
- MANDATORY → Exige una existente.

---

### isolation

Controla qué pueden ver las transacciones concurrentes.

- READ_COMMITTED → Equilibrio ideal.
- SERIALIZABLE → Máxima seguridad.

---

## 💡 Regla de Oro

Piensa en **@Transactional** como el cinturón de seguridad de tu aplicación.

Mientras todo va bien parece innecesario.

Pero el día que ocurre un error en producción, será lo que evite que tu base de datos quede inconsistente, con dinero perdido, inventarios incorrectos o registros incompletos.

---

## 🔙 [***Volver***](./servicios.md)