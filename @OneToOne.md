## 🔙 [***Volver***](./entidades.md)

---

# 🏗️ Construyendo una relación `@OneToOne`
## El Local de Videojuegos y su Gerente

En este capítulo aprenderemos una de las relaciones más importantes de **JPA/Hibernate**: la relación **Uno a Uno (`@OneToOne`)**.

A lo largo del capítulo construiremos una relación real entre dos entidades utilizando:

- `@OneToOne`
- `@JoinColumn`
- `mappedBy`
- `cascade`
- `fetch`
- `orphanRemoval`

Además aprenderás uno de los conceptos más importantes de Hibernate:

- ¿Qué es el **lado propietario (Owning Side)**?
- ¿Qué es el **lado inverso (Inverse Side)**?
- ¿Por qué solo una entidad crea la llave foránea?
- ¿Por qué existe `mappedBy`?
- ¿Cómo funciona realmente una relación bidireccional?

Cuando termines este capítulo entenderás cómo funcionan prácticamente todas las relaciones de JPA, ya que estos mismos conceptos también se utilizan en:

- `@OneToMany`
- `@ManyToOne`
- `@ManyToMany`

---

# 🎮 Nuestra historia

Durante todo el capítulo utilizaremos un ejemplo muy sencillo.

Tenemos un **Local de Videojuegos**.

Y ese local tiene un único gerente.

```text
🎮 Local GameZone

        │

        ▼

👨‍💼 Gerente Juan
```

Las reglas del negocio son:

- Un Local solo tiene un Gerente.
- Un Gerente solo administra un Local.

Esta es exactamente una relación **Uno a Uno**.

---

# 📦 Dependencia necesaria

Para trabajar con entidades y relaciones en Spring Boot necesitamos el starter de JPA.

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
```

---

# 🚗 Primera etapa
# Relación Unidireccional

Comenzaremos con la forma más sencilla.

Solo una entidad conoce a la otra.

En este caso:

```text
🎮 Local
     │
     ▼
👨‍💼 Gerente
```

El Local conoce al Gerente.

Pero el Gerente no conoce al Local.

Es exactamente igual que una calle de un solo sentido.

Puedes avanzar en una dirección, pero no regresar por el mismo camino.

---

# 👨‍💼 Entidad `Gerente`

Comencemos creando la entidad más simple.

```java
package com.montoapp.entities;

import jakarta.persistence.*;

@Entity
@Table(name = "gerentes")
public class Gerente {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String nombre;

}
```

---

## 📖 ¿Qué representa esta entidad?

Esta clase representa a un gerente del local.

Por ahora únicamente almacena:

- Su identificador (`id`)
- Su nombre

Todavía no existe ninguna relación con otra entidad.

El gerente puede existir completamente solo.

```text
👨‍💼 Juan
```

Nada más.

---

# 🎮 Entidad `Local`

Ahora construiremos la entidad que sí conoce al gerente.

```java
package com.montoapp.entities;

import jakarta.persistence.*;

@Entity
@Table(name = "locales")
public class Local {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String nombreLocal;

    @OneToOne
    @JoinColumn(name = "gerente_id")
    private Gerente gerente;

}
```

---

# 🔗 ¿Qué hace `@OneToOne`?

Esta anotación le dice a Hibernate:

> "Cada Local tendrá asociado un único Gerente."

Visualmente:

```text
🎮 Local
      │
      ▼
👨‍💼 Gerente
```

No significa que todavía exista una columna en la base de datos.

Simplemente indica el tipo de relación entre ambos objetos.

---

# 🔑 ¿Qué hace `@JoinColumn`?

Ahora sí aparece la parte importante.

```java
@JoinColumn(name = "gerente_id")
```

Esta anotación le dice a Hibernate:

> "Necesito una columna física en la base de datos para guardar qué gerente pertenece a este local."

Por eso Hibernate crea una llave foránea llamada:

```text
gerente_id
```

dentro de la tabla **locales**.

---

# 🗄️ ¿Cómo queda la base de datos?

Tabla **gerentes**

| id | nombre |
|----|---------|
| 1 | Juan |

Tabla **locales**

| id | nombre_local | gerente_id |
|----|---------------|------------|
| 1 | GameZone | 1 |

Observa que únicamente la tabla **locales** guarda la referencia.

No existe ninguna columna llamada:

```text
local_id
```

dentro de la tabla `gerentes`.

---

# 💡 ¿Qué significa esto?

Si Hibernate carga un Local, puede obtener inmediatamente su Gerente.

```java
Local local = ...

local.getGerente();
```

Pero ocurre algo diferente si comenzamos desde el Gerente.

```java
Gerente gerente = ...

gerente.getLocal(); // ❌ No existe
```

¿Por qué?

Porque el objeto **Gerente** todavía no conoce al objeto **Local**.

La relación solamente funciona en una dirección.

Por eso recibe el nombre de **relación unidireccional**.

---

# 🤔 Entonces...

Aquí aparece una pregunta muy importante.

¿Qué ocurre si ya tengo un Gerente y quiero saber en qué Local trabaja?

No puedo hacerlo.

Y precisamente ese será el problema que resolveremos en la siguiente sección cuando construyamos una **relación bidireccional**.

---

# 🔄 Segunda etapa
# Relación Bidireccional
## El camino de doble vía

En la sección anterior construimos una relación **unidireccional**.

Visualmente quedó así:

```text
🎮 Local
     │
     ▼
👨‍💼 Gerente
```

El Local conoce al Gerente.

Pero el Gerente no conoce al Local.

---

# 🤔 El problema de la relación unidireccional

Imagina que ya recuperaste un gerente desde la base de datos.

```java
Gerente gerente = ...
```

Ahora quieres saber:

> **¿En qué Local trabaja este gerente?**

Naturalmente intentarías hacer algo como esto:

```java
gerente.getLocal();
```

Pero aparece un problema.

Ese método no existe.

¿Por qué?

Porque nunca le enseñamos al objeto `Gerente` quién era su Local.

El único que conoce la relación es el objeto `Local`.

---

# 🚗 Una calle de una sola vía

Una relación unidireccional funciona exactamente igual que una calle de un único sentido.

```text
🎮 Local
     │
     ▼
👨‍💼 Gerente
```

Puedes viajar desde el Local hacia el Gerente.

Pero jamás podrás regresar.

---

# 🎯 La solución

Queremos que ambos objetos puedan conocerse entre sí.

Es decir:

- que el Local conozca al Gerente;
- y que el Gerente conozca al Local.

Visualmente quedaría así:

```text
🎮 Local ◄────────────► 👨‍💼 Gerente
```

Ahora podemos navegar desde cualquiera de los dos lados.

```java
local.getGerente();
```

o

```java
gerente.getLocal();
```

Mucho más cómodo.

Pero aquí aparece una de las dudas más grandes de quienes comienzan con JPA.

---

# 🤯 La gran pregunta

Si ahora ambos objetos se conocen...

¿La base de datos también guardará dos conexiones?

Muchos creen que sí.

La realidad es que **NO**.

Y aquí comienza uno de los conceptos más importantes de Hibernate.

---

# 🗄️ Java y la Base de Datos son mundos diferentes

Es muy importante entender que:

> **Los objetos de Java no son la base de datos.**

En Java puedes tener muchos objetos apuntándose entre sí.

```text
JAVA

        Local
          │
          ▼
      Gerente
          ▲
          │
          └──────────────
```

Pero en una base de datos relacional no es necesario guardar dos conexiones.

Solo hace falta una llave foránea.

```text
BASE DE DATOS

Tabla: locales

id
nombre_local
gerente_id
```

Observa algo muy importante.

Aunque en Java ambos objetos se conocen...

En SQL solamente existe una columna.

```text
gerente_id
```

Eso significa que alguien debe ser el responsable de administrar esa columna.

---

# 👑 El Lado Propietario (Owning Side)

Hibernate necesita saber quién será el encargado de guardar y actualizar la llave foránea.

A esa entidad se le conoce como:

## 👉 Lado Propietario
(**Owning Side**)

Es el único que tiene permiso para modificar la relación.

En nuestro ejemplo será:

```text
🎮 Local
```

Porque es la entidad que tiene:

```java
@JoinColumn(name = "gerente_id")
```

Podemos imaginar que el Local le dice a Hibernate:

> "Yo soy el responsable de guardar qué gerente pertenece a este local."

---

# 🔑 ¿Qué hace realmente `@JoinColumn`?

Muchos piensan que solamente crea una columna.

En realidad hace mucho más.

También le dice a Hibernate:

> "Yo soy quien controla esta relación."

Por eso decimos que el `Local` es el **lado propietario**.

Es la única entidad que puede:

- crear la llave foránea;
- actualizarla;
- modificarla;
- eliminarla.

---

# 🙋 ¿Y el Gerente?

El Gerente también quiere conocer al Local.

Pero no quiere administrar la relación.

Solo quiere poder navegar hacia ella.

Por eso agregaremos un nuevo atributo.

```java
private Local local;
```

Sin embargo...

Todavía falta decirle a Hibernate que esta propiedad **NO** crea una nueva relación.

Aquí aparece `mappedBy`.

---

# 🧭 ¿Qué hace realmente `mappedBy`?

Supongamos que escribimos esto:

```java
@OneToOne
private Local local;
```

Hibernate pensará:

> "Perfecto."

> "Aquí hay otra relación OneToOne."

Entonces intentará crear otra llave foránea.

Y terminaríamos con algo parecido a esto.

Tabla **locales**

```text
gerente_id
```

Tabla **gerentes**

```text
local_id
```

Acabamos de crear dos relaciones diferentes.

Y eso no era lo que queríamos.

---

# 🚫 Aquí entra `mappedBy`

Cuando escribimos:

```java
mappedBy = "gerente"
```

Le estamos diciendo a Hibernate:

> "No crees otra relación."

> "La relación ya existe."

> "Simplemente utiliza la que administra la variable `gerente` de la clase `Local`."

En otras palabras:

El Gerente le dice a Hibernate:

> "Yo solamente quiero consultar la relación."

> "Quien realmente manda es el Local."

---

# 🧠 Una analogía muy sencilla

Imagina una carpeta compartida en una empresa.

Solo una persona tiene permiso para modificarla.

La otra únicamente puede verla.

```text
📁 Relación

🎮 Local
    ✅ Puede modificar

👨‍💼 Gerente
    👀 Solo consulta
```

Eso es exactamente lo que ocurre con:

- `@JoinColumn`
- `mappedBy`

---

# 🎯 Regla de Oro

En una relación bidireccional siempre existen dos lados.

## 👑 Lado propietario

Es quien tiene:

```java
@JoinColumn(...)
```

Es el responsable de guardar la llave foránea.

---

## 🔄 Lado inverso

Es quien tiene:

```java
mappedBy = "..."
```

No crea columnas.

No crea llaves foráneas.

No administra la relación.

Simplemente permite navegar hacia el otro objeto.

---

# 📌 Visualizando toda la relación

## En Java

```text
                 Java

      🎮 Local ◄────────────► 👨‍💼 Gerente
```

Ambos objetos se conocen.

---

## En la Base de Datos

```text
Tabla locales

id
nombre_local
gerente_id
```

Solo existe una llave foránea.

Nunca dos.

---

# 💡 La idea más importante de todo el capítulo

Una relación bidireccional **no significa que existan dos relaciones en la base de datos**.

Significa que existen **dos referencias en memoria (Java)** para poder navegar cómodamente entre los objetos.

La base de datos sigue almacenando una única llave foránea.

Por eso siempre existe:

- un **lado propietario**, encargado de administrar la relación;
- y un **lado inverso**, encargado únicamente de navegar por ella.

Una vez comprendas este concepto, entenderás cómo funcionan prácticamente todas las relaciones de JPA (`@OneToMany`, `@ManyToOne` y `@ManyToMany`), ya que todas siguen exactamente esta misma filosofía.

---

# 💻 Construyendo la Relación Bidireccional

Ahora que ya entendemos la diferencia entre el **lado propietario** y el **lado inverso**, es momento de construir ambas entidades.

Nuestro objetivo es que:

- El **Local** conozca a su **Gerente**.
- El **Gerente** conozca al **Local**.
- Solo exista **una llave foránea** en la base de datos.

Al finalizar tendremos esta estructura:

```text
                 JAVA

        🎮 Local ◄────────────► 👨‍💼 Gerente
```

Mientras que en la base de datos únicamente existirá una relación:

```text
BASE DE DATOS

Tabla: locales

id
nombre_local
gerente_id
```

---

# 👑 Paso 1: Crear el Lado Propietario

Comenzaremos por la entidad que administrará la relación.

En nuestro caso será **Local**.

```java
package com.montoapp.entities;

import jakarta.persistence.*;

@Entity
@Table(name = "locales")
public class Local {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String nombreLocal;

    @OneToOne(
            cascade = CascadeType.ALL,
            fetch = FetchType.LAZY,
            orphanRemoval = true
    )
    @JoinColumn(name = "gerente_id")
    private Gerente gerente;

}
```

---

# 🔍 Analizando el código

Veamos qué hace cada parte.

---

## 🏷️ `@OneToOne`

```java
@OneToOne(...)
```

Le indica a Hibernate:

> "Esta entidad tiene exactamente un único Gerente."

Y, al mismo tiempo, un Gerente solamente podrá estar asociado a un único Local.

Visualmente:

```text
🎮 Local ─────────► 👨‍💼 Gerente
```

---

## 🔑 `@JoinColumn`

```java
@JoinColumn(name = "gerente_id")
```

Esta anotación cumple dos funciones muy importantes.

### 1️⃣ Crear la llave foránea

Hibernate agregará una columna llamada:

```text
gerente_id
```

Dentro de la tabla:

```text
locales
```

---

### 2️⃣ Indicar quién administra la relación

Al tener `@JoinColumn`, Hibernate entiende que el **Local** será el responsable de:

- guardar la relación;
- modificarla;
- actualizarla;
- eliminarla.

Por eso decimos que **Local** es el **Owning Side** (lado propietario).

---

# 🗄️ Así queda la tabla

```text
locales
```

| id | nombre_local | gerente_id |
|----|--------------|------------|
| 1 | GameZone | 3 |

Mientras tanto, la tabla:

```text
gerentes
```

No necesita ninguna columna adicional.

| id | nombre |
|----|---------|
| 3 | Juan |

---

# 🔄 Paso 2: Crear el Lado Inverso

Ahora construiremos la entidad `Gerente`.

Su objetivo no será administrar la relación.

Únicamente permitirá navegar desde el gerente hacia el local.

```java
package com.montoapp.entities;

import jakarta.persistence.*;

@Entity
@Table(name = "gerentes")
public class Gerente {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String nombre;

    @OneToOne(mappedBy = "gerente")
    private Local local;

}
```

---

# 🔍 Analizando el código

La parte más importante es esta:

```java
mappedBy = "gerente"
```

Muchos desarrolladores creen que `"gerente"` hace referencia a la clase `Gerente`.

En realidad no.

Hace referencia al **nombre del atributo** que existe en la otra entidad.

Observa nuevamente la clase `Local`.

```java
private Gerente gerente;
```

El atributo se llama:

```text
gerente
```

Por eso escribimos:

```java
mappedBy = "gerente"
```

Hibernate interpreta esta instrucción como:

> "La relación que necesito ya existe."

> "Está siendo administrada por el atributo llamado `gerente` dentro de la entidad `Local`."

---

# ⚠️ Un error muy común

Algunas personas escriben:

```java
mappedBy = "Gerente"
```

O incluso:

```java
mappedBy = "GERENTE"
```

Eso es incorrecto.

`mappedBy` **no recibe el nombre de la clase**.

Recibe exactamente el nombre del atributo.

Debe coincidir carácter por carácter.

Si el atributo se llama:

```java
private Gerente gerentePrincipal;
```

Entonces deberá ser:

```java
mappedBy = "gerentePrincipal"
```

---

# 🧠 ¿Cómo encuentra Hibernate la relación?

Cuando inicia la aplicación, Hibernate hace algo parecido a esto:

1. Busca la entidad `Gerente`.
2. Encuentra:

```java
mappedBy = "gerente"
```

3. Va a la entidad `Local`.
4. Busca un atributo llamado:

```java
gerente
```

5. Encuentra:

```java
@JoinColumn(name = "gerente_id")
```

6. Comprende que esa ya es la relación principal.

7. No crea una segunda llave foránea.

Todo ocurre automáticamente durante el arranque de la aplicación.

---

# 🔍 ¿Qué ocurre si el nombre no coincide?

Supongamos que el atributo se llama:

```java
private Gerente gerentePrincipal;
```

Pero escribimos:

```java
mappedBy = "gerente"
```

Hibernate buscará un atributo llamado:

```text
gerente
```

Como no existe, lanzará una excepción durante el inicio de la aplicación.

Por eso siempre debes asegurarte de que ambos nombres coincidan exactamente.

---

# 🎯 Probando la navegación

Ahora ambos objetos pueden comunicarse entre sí.

Desde el Local:

```java
Local local = ...

Gerente gerente = local.getGerente();
```

Y también desde el Gerente:

```java
Gerente gerente = ...

Local local = gerente.getLocal();
```

Observa que ahora la navegación funciona en ambos sentidos.

```text
🎮 Local ◄────────────► 👨‍💼 Gerente
```

Sin embargo...

La base de datos continúa almacenando una única llave foránea.

```text
locales

id
nombre_local
gerente_id
```

No existe una columna `local_id` dentro de la tabla `gerentes`.

---

# 🎒 Lo que debes recordar

Hasta este momento ya conoces los tres pilares fundamentales de una relación bidireccional.

| Concepto | Función |
|----------|---------|
| `@OneToOne` | Indica que existe una relación Uno a Uno entre dos entidades. |
| `@JoinColumn` | Crea la llave foránea y convierte a la entidad en el lado propietario. |
| `mappedBy` | Indica que la relación ya existe y que esta entidad solo representa el lado inverso. |

Con estos tres elementos ya puedes construir correctamente una relación bidireccional en JPA.

En la siguiente sección aprenderemos tres configuraciones muy importantes que modifican el comportamiento de esa relación:

- `cascade`
- `fetch`
- `orphanRemoval`

Estas propiedades no crean la relación, pero sí controlan cómo se comporta durante el ciclo de vida de las entidades.

---

## 🔙 [***Volver***](./entidades.md)