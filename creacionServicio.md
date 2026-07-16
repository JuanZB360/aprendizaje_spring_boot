## 🔙 [***Volver***](./servicios.md)

---

# 🧠 ¿Qué es un Servicio (`@Service`)?

## 👶 Explicación para niños

Imagina que en nuestro local de videojuegos tenemos una regla especial:

> "Si un niño alquila un juego el día de su cumpleaños, el sistema le regala un dulce y le hace un descuento del 50%."

El **Recepcionista** (Repositorio) no sabe tomar esa decisión.

Él solamente sabe:

- guardar juegos,
- buscarlos,
- listarlos,
- o borrarlos.

No sabe calcular descuentos ni aplicar reglas especiales.

Para eso necesitamos al **Administrador del Local**.

El Administrador revisa:

- si el niño cumple años,
- cuánto cuesta el juego,
- cuánto descuento debe aplicar,
- y qué regalos debe entregar.

Cuando termina de pensar, le dice al recepcionista:

> "Guarda esta información en la base de datos."

---

## 🎮 Traducción al mundo de Spring Boot

En Spring Boot, ese Administrador se llama:

```java
@Service
```

Un Servicio es la capa donde escribimos las reglas de negocio de nuestra aplicación.

Aquí es donde la aplicación:

- toma decisiones,
- valida información,
- calcula resultados,
- coordina procesos,
- y se comunica con los repositorios.

---

## 🛠️ ¿Cómo se ve en el código?

```java
package com.montoapp.services;

import org.springframework.stereotype.Service;

@Service
public class JuegoService {

    // Aquí programaremos toda la lógica inteligente
}
```

---

## 🔍 ¿Qué significa `@Service`?

```java
@Service
```

Es una etiqueta que le dice a Spring Boot:

> "Oye Spring, esta clase contiene la lógica del negocio. Mantén una instancia lista en memoria para que pueda ser utilizada cuando alguien la necesite."

Gracias a esto Spring puede administrar automáticamente el objeto y permitir que otras clases lo utilicen mediante Inyección de Dependencias.

---

# 🏢 ¿Qué responsabilidades tiene un Servicio?

Un Servicio normalmente se encarga de:

### ✅ Validar información

```java
if(precio < 0){
    throw new RuntimeException("El precio no puede ser negativo");
}
```

---

### ✅ Aplicar reglas de negocio

```java
if(cliente.esCumpleaños()){
    precioFinal = precio * 0.5;
}
```

---

### ✅ Coordinar varios repositorios

```java
clienteRepository.save(cliente);

pedidoRepository.save(pedido);

facturaRepository.save(factura);
```

---

### ✅ Preparar información antes de guardarla

```java
juego.setFechaRegistro(LocalDate.now());

juegoRepository.save(juego);
```

---

# 🔄 Flujo Completo de una Solicitud

Cuando un usuario realiza una acción, normalmente ocurre este recorrido:

```text
Cliente
   ↓
Controller
   ↓
Service
   ↓
Repository
   ↓
Base de Datos
```

---

## 👶 Mente de niño

```text
Cliente
   ↓
Empleado de Ventanilla
   ↓
Administrador del Local
   ↓
Recepcionista
   ↓
Cuaderno de Registros
```

### ¿Quién hace qué?

**Controller**
👉 Recibe la solicitud.

**Service**
👉 Piensa y toma decisiones.

**Repository**
👉 Habla con la base de datos.

**Database**
👉 Guarda la información.

---

# 💡 Idea Clave

El Servicio es el cerebro del sistema.

Los Repositorios saben guardar y buscar datos.

Los Controladores saben recibir solicitudes.

Pero el Servicio es quien entiende las reglas del negocio y decide qué debe ocurrir antes de hablar con la base de datos.

---

## 🎒 Resumen

- `@Service` representa la capa de lógica de negocio.
- Aquí se programan las reglas y decisiones de la aplicación.
- Se comunica con los Repositorios.
- No guarda datos directamente; delega esa tarea al Repository.
- Actúa como el Administrador del Local que organiza todo antes de que la información llegue a la base de datos.

---

## 🔙 [***Volver***](./servicios.md)