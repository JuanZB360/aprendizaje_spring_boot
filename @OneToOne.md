## 🔙 [***Volver***](./entidades.md)
---

# 🏗️ Construyendo una relación `@OneToOne`
## El Local de Videojuegos y su Gerente

Vamos a construir paso a paso una relación real en Spring Boot usando:

- `@OneToOne`
- `@JoinColumn`
- `mappedBy`
- `cascade`
- `fetch`
- `orphanRemoval`

Usaremos esta historia:

# 🎮 Local de Videojuegos
y
# 👨‍💼 Gerente Juan

---

# 📦 Starter necesario

Para trabajar con relaciones y entidades necesitas:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
```

---

# 🔹 Parte 1
# Relación Unidireccional
## El camino de una sola vía

En este caso:

# 🎮 El Local
sí conoce al:
# 👨‍💼 Gerente

Pero el Gerente:

❌ NO sabe en qué Local trabaja.

---

# 👨‍💼 Entidad `Gerente`
## La pieza independiente

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

# 🔍 ¿Qué hace esta entidad?

Define:

- el ID del gerente,
- y su nombre.

---

# ⚠️ Importante

Aquí NO existe ninguna referencia al Local.

El Gerente vive “solo”.

---

# 🎮 Entidad `Local`
## El dueño de la relación

Aquí sí ocurre la conexión.

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
    @JoinColumn(name = "gerente_id", referencedColumnName = "id")
    private Gerente gerente;
}
```

---

# 🔗 ¿Qué hace `@OneToOne`?

Es el cable de conexión.

Le dice a Spring:

> “Este Local tendrá un solo Gerente”.

---

# 🔑 ¿Qué hace `@JoinColumn`?

```java
@JoinColumn(name = "gerente_id")
```

Le dice a Spring:

> “Crea una columna física llamada gerente_id”.

---

# 🗄️ ¿Dónde se crea esa columna?

En la tabla:

# 🎮 `locales`

---

# 📌 Resultado en la base de datos

| id | nombre_local | gerente_id |
|---|---|---|
| 1 | GameZone | 3 |

---

# 💡 ¿Qué significa?

El Local:

🎮 GameZone

está conectado con:

👨‍💼 el Gerente número 3.

---

# 🔹 Parte 2
# Relación Bidireccional
## El camino de doble vía

Ahora mejoraremos la relación.

Queremos que:

- el Local conozca al Gerente,
- y el Gerente también conozca al Local.

---

# 🎮 Entidad fuerte
## `Local`

El Local seguirá siendo:

# 👑 El dueño de la relación

---

# 💻 Código

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
    @JoinColumn(name = "gerente_id", referencedColumnName = "id")
    private Gerente gerente;
}
```

---

# ⚡ Explicación de los superpoderes

---

# 🔄 `cascade = CascadeType.ALL`

## El efecto dominó

Si el Local:

- se guarda,
- se actualiza,
- o se elimina,

Spring hará lo mismo automáticamente con el Gerente.

---

# 🦥 `fetch = FetchType.LAZY`

## El inteligente

Spring NO traerá automáticamente al Gerente.

Solo lo buscará cuando realmente lo necesites.

---

# 👻 `orphanRemoval = true`

## El limpiador automático

Si el Local cambia de Gerente:

- el gerente viejo queda abandonado.

Entonces Spring:

🗑️ lo elimina automáticamente.

---

# 👨‍💼 Entidad inversa
## `Gerente`

Ahora el Gerente también podrá conocer al Local.

---

# 💻 Código

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

    @OneToOne(
        mappedBy = "gerente",
        fetch = FetchType.LAZY
    )
    private Local local;
}
```

---

# 🔍 ¿Qué hace `mappedBy`?

```java
mappedBy = "gerente"
```

Le dice a Spring:

> “La relación real ya la controla la variable gerente dentro de Local”.

---

# ⚠️ ¿Por qué es importante?

Evita que Spring cree:

❌ otra columna innecesaria.

---

# 🧠 Idea fácil

| Entidad | Rol |
|---|---|
| `Local` | Dueño de la relación |
| `Gerente` | Lado inverso |

---

# 📌 Resultado final

En la base de datos:

✅ SOLO existe:

`gerente_id`

dentro de:

# 🎮 `locales`

---

# ⚡ Pero en Java…

Puedes navegar desde ambos lados.

---

# 💡 Ejemplo mental

Desde Local:

```java
local.getGerente();
```

---

Desde Gerente:

```java
gerente.getLocal();
```

---

# 🎯 Lección importante

La magia de una relación bidireccional es que:

- en SQL solo existe UNA conexión,
- pero en Java puedes moverte en ambas direcciones.

---

# 🧠 Resumen fácil

| Concepto | Función |
|---|---|
| `@OneToOne` | Conecta dos entidades |
| `@JoinColumn` | Crea la llave foránea |
| `mappedBy` | Indica quién controla la relación |
| `cascade` | Propaga acciones automáticamente |
| `fetch` | Decide cuándo cargar datos |
| `orphanRemoval` | Elimina datos abandonados |

---

## 🔙 [***Volver***](./entidades.md)