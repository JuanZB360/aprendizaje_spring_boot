## 🔙 [***Volver***](./entidades.md)

---

# 🚀 Lombok: Eliminando Código Repetitivo en Java

## 📖 ¿Qué es Lombok?

**Lombok** es una librería que ayuda a reducir la cantidad de código repetitivo que escribimos en Java.

Normalmente, cuando creamos una clase, debemos escribir manualmente:

* Getters (`getNombre()`)
* Setters (`setNombre()`)
* Constructores
* Métodos `toString()`
* Métodos `equals()` y `hashCode()`

Todo esto puede ocupar decenas de líneas que realmente no aportan lógica de negocio.

Lombok resuelve este problema generando automáticamente ese código durante la compilación.

> ⚠️ Importante: Lombok no cambia cómo funciona Java. Simplemente genera código que normalmente escribirías a mano.

---

# 🎯 Las Etiquetas de Datos (Adiós a los Getters, Setters y Constructores aburridos)

Imagina que estás creando una clase para representar los juegos de una tienda.

En Java tradicional, una clase sencilla puede terminar teniendo más código de infraestructura que datos reales.

## 👶 Explicación para niños

Imagina que vas a construir una caja para guardar juguetes.

Antes tenías que fabricar manualmente:

* Una ventana para mirar dentro → **Getter**
* Una compuerta para meter cosas → **Setter**
* Un letrero que describa el contenido → **toString()**
* Un sistema para crear cajas nuevas → **Constructores**

Con Lombok simplemente pegas una **calcomanía mágica** sobre la caja y todo aparece automáticamente.

---

# 🛠️ @Getter y @Setter

Estas anotaciones generan automáticamente los métodos para leer y modificar atributos.

```java
@Getter
@Setter
public class Juego {

    private Long id;
    private String titulo;
    private Double precio;

}
```

Lombok genera automáticamente:

```java
public Long getId() {
    return id;
}

public void setId(Long id) {
    this.id = id;
}
```

Y lo mismo para todos los demás atributos.

---

# 🛠️ @ToString

Genera automáticamente el método `toString()`.

```java
@ToString
public class Juego {

    private String titulo;
    private Double precio;

}
```

Ahora puedes hacer:

```java
System.out.println(juego);
```

Resultado:

```text
Juego(titulo=Minecraft, precio=29.99)
```

Sin necesidad de escribir el método manualmente.

---

# 🛠️ @NoArgsConstructor

Genera un constructor vacío.

```java
@NoArgsConstructor
public class Juego {

    private String titulo;
    private Double precio;

}
```

Lombok genera:

```java
public Juego() {
}
```

## ¿Por qué es importante?

Frameworks como:

* Spring
* Hibernate
* JPA

Necesitan un constructor vacío para poder crear objetos internamente.

---

# 🛠️ @AllArgsConstructor

Genera un constructor con todos los atributos.

```java
@AllArgsConstructor
public class Juego {

    private String titulo;
    private Double precio;

}
```

Lombok genera:

```java
public Juego(String titulo, Double precio) {
    this.titulo = titulo;
    this.precio = precio;
}
```

Uso:

```java
Juego juego = new Juego("Minecraft", 29.99);
```

---

# 🎯 Ejemplo Completo

```java
package com.montoapp.entities;

import lombok.*;

@Getter
@Setter
@ToString
@NoArgsConstructor
@AllArgsConstructor
public class Juego {

    private Long id;
    private String titulo;
    private Double precio;

}
```

Con solo unas pocas líneas obtenemos todo el código que normalmente escribiríamos a mano.

---

# 🧠 El Atajo de Oro: @Data

Lombok ofrece una anotación llamada `@Data`.

Esta es una combinación de varias anotaciones:

```java
@Data
public class Juego {

    private String titulo;
    private Double precio;

}
```

Internamente incluye:

```java
@Getter
@Setter
@ToString
@EqualsAndHashCode
@RequiredArgsConstructor
```

---

## ¿Cuándo usar @Data?

Es ideal para:

* DTOs
* Objetos de respuesta
* Objetos de petición
* Clases simples de transferencia de datos

Ejemplo:

```java
@Data
public class JuegoDTO {

    private String titulo;
    private Double precio;

}
```

---

## ⚠️ Advertencia para Entidades JPA

Aunque `@Data` es muy popular, generalmente **NO se recomienda utilizarlo en entidades JPA/Hibernate**.

Ejemplo:

```java
@Entity
@Data
public class Juego {
}
```

### ¿Por qué?

Porque Lombok genera automáticamente:

```java
@EqualsAndHashCode
```

Y esto puede provocar:

* Problemas de rendimiento.
* Comparaciones incorrectas.
* Bucles infinitos en relaciones bidireccionales.
* Errores difíciles de detectar.

### Recomendación profesional

Para entidades JPA es mejor usar:

```java
@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
```

Y controlar manualmente `equals()` y `hashCode()` cuando sea necesario.

---

# 🏗️ El Súper Arquitecto: @Builder

Esta es una de las anotaciones más apreciadas por los desarrolladores profesionales.

Permite construir objetos de manera limpia, legible y segura.

---

## 👶 Explicación para niños

Imagina que estás en una heladería.

En lugar de recibir un helado ya armado, eliges:

* Chocolate
* Chispitas
* Galleta
* Salsa

Y cuando terminas dices:

> "¡Listo, construir!"

Eso es exactamente lo que hace `@Builder`.

---

# 🛠️ Cómo usar @Builder

## Paso 1: Agregar la anotación

```java
@Getter
@Setter
@Builder
@AllArgsConstructor
@NoArgsConstructor
public class Juego {

    private String titulo;
    private Double precio;
    private String consola;

}
```

---

## Paso 2: Construir el objeto

```java
Juego miJuegoFavorito = Juego.builder()
        .titulo("Minecraft")
        .precio(29.99)
        .consola("PC")
        .build();
```

Resultado:

```java
Juego(
    titulo="Minecraft",
    precio=29.99,
    consola="PC"
)
```

---

## Ventajas del Builder

### ✅ Más legible

```java
Juego.builder()
      .titulo("Minecraft")
      .precio(29.99)
      .build();
```

vs

```java
new Juego(
    "Minecraft",
    29.99,
    "PC"
);
```

---

### ✅ Evita errores de orden

Con constructores es fácil equivocarse:

```java
new Juego(
    "PC",
    29.99,
    "Minecraft"
);
```

Con Builder es imposible confundir los atributos porque cada uno tiene su nombre.

---

### ✅ Permite campos opcionales

Si no quieres asignar un atributo:

```java
Juego.builder()
      .titulo("Minecraft")
      .build();
```

No ocurre ningún error.

---

# 🔌 Inyección de Dependencias con @RequiredArgsConstructor

## ¿Qué hace?

Genera automáticamente un constructor para todos los atributos marcados como `final`.

---

## 👶 Explicación para niños

Imagina que tu televisor necesita:

* Un cable de energía.
* Un cable HDMI.

Como son obligatorios, deben conectarse desde el momento en que se fabrica.

`@RequiredArgsConstructor` conecta automáticamente todos esos cables obligatorios.

---

## Ejemplo

```java
@Service
@RequiredArgsConstructor
public class JuegoService {

    private final JuegoRepository juegoRepository;

}
```

Lombok genera:

```java
public JuegoService(JuegoRepository juegoRepository) {
    this.juegoRepository = juegoRepository;
}
```

---

## ¿Por qué es importante en Spring?

Spring puede usar ese constructor para realizar la **inyección de dependencias por constructor**, que es la forma recomendada actualmente.

---

## ❌ Forma antigua

```java
@Autowired
private JuegoRepository juegoRepository;
```

---

## ✅ Forma moderna

```java
@RequiredArgsConstructor
private final JuegoRepository juegoRepository;
```

Más segura, más limpia y más fácil de probar.

---

# 🔒 @Value: El Candado de la Inmutabilidad

La anotación `@Value` convierte una clase en **inmutable**.

Una vez creado el objeto, sus valores ya no pueden cambiar.

---

## 👶 Explicación para niños

Imagina que grabas algo en piedra.

Puedes leerlo todas las veces que quieras.

Pero nadie puede borrarlo o modificarlo.

Eso es un objeto inmutable.

---

## Ejemplo

```java
@Value
public class JuegoDTO {

    String titulo;
    Double precio;

}
```

---

## Lombok genera automáticamente

```java
private final String titulo;
private final Double precio;

public String getTitulo() {
    return titulo;
}

public Double getPrecio() {
    return precio;
}
```

Y además:

* Constructor obligatorio.
* `equals()`
* `hashCode()`
* `toString()`

---

## Lo que NO genera

```java
setTitulo(...)
setPrecio(...)
```

Porque los datos no pueden modificarse.

---

# 🎯 ¿Cuándo usar @Value?

Es especialmente útil para:

* DTOs de respuesta
* Objetos de configuración
* Objetos de solo lectura
* Programación funcional
* Sistemas concurrentes

---

# 🎒 Resumen para llevar en tu mochila de Senior

| Anotación                  | Función                             |
| -------------------------- | ----------------------------------- |
| `@Getter`                  | Genera getters                      |
| `@Setter`                  | Genera setters                      |
| `@ToString`                | Genera el método `toString()`       |
| `@NoArgsConstructor`       | Constructor vacío                   |
| `@AllArgsConstructor`      | Constructor con todos los atributos |
| `@RequiredArgsConstructor` | Constructor con atributos `final`   |
| `@Data`                    | Combina varias anotaciones comunes  |
| `@Builder`                 | Construcción elegante de objetos    |
| `@Value`                   | Clase completamente inmutable       |

---

# 🏆 Conclusión

Lombok no reemplaza Java ni modifica su comportamiento.

Lo que hace es eliminar el código repetitivo para que el desarrollador pueda concentrarse en la lógica de negocio.

Cuando el proyecto se compila:

1. Lombok genera automáticamente los métodos necesarios.
2. El compilador de Java los incorpora al bytecode.
3. La JVM ejecuta una clase completamente normal.

Por eso se suele decir:

> **"Lombok no hace magia, simplemente escribe por ti el código aburrido."**
|

---

## 🔙 [***Volver***](./entidades.md)