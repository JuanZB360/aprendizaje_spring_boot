## 🔙 [***Volver***](./definir.md)

---

# 3. `@Service`
## 🧠 El cerebro de la operación

Aquí es donde vive la verdadera “magia” de la aplicación y donde se definen las reglas del negocio.

---

# ❓ ¿Qué hace?

La anotación `@Service` le indica a Spring que esta clase contiene la lógica principal del sistema.

Es el lugar donde ocurren:

- 🧮 cálculos,
- ✅ validaciones complejas,
- 🧠 decisiones importantes,
- ⚙️ procesos internos,
- 🔄 coordinación entre componentes.

---

# 🕒 ¿Cuándo usarla?

Se utiliza cuando un proceso requiere varios pasos o reglas específicas del negocio.

## 📌 Ejemplo

```text
Registrar una venta:
1. Calcular el IVA
2. Descontar el stock
3. Generar el recibo
4. Guardar la venta
```

Todo este flujo normalmente vive dentro de un @Service.

---

## 🎭 Personalidad

Es el Director de la operación.

- No sabe cómo hablar con el cliente (@RestController).
- Tampoco sabe cómo guardar datos (@Repository).

Su única responsabilidad es:

```text
“Saber cómo debe funcionar el negocio”.
```

---

## 🔄 Relación con otras capas

El @Service normalmente se encuentra en el centro del flujo de la aplicación.

```text
Cliente
   ↓
@RestController
   ↓
@Service
   ↓
@Repository
   ↓
Base de Datos
```

---

## ⚙️ Ejemplo conceptual

```java
@Service
public class VentaService {

}
```
Spring detecta esta clase automáticamente y la registra como un Bean especializado en lógica de negocio.

---

## 🧠 Idea clave

El @Service conecta:

- reglas del sistema,
- procesos internos,
- cálculos,
- y acceso a datos.

Es la capa donde realmente “piensa” la aplicación.

---

## 📌 Resumen rápido

| Concepto          | Significado                       |
| ----------------- | --------------------------------- |
| `@Service`        | Capa de lógica de negocio         |
| Función principal | Procesar reglas y operaciones     |
| Trabaja junto a   | `@RestController` y `@Repository` |
| Contiene          | Validaciones, cálculos y procesos |
| Personalidad      | El Director del sistema           |


---

## 🔙 [***Volver***](./definir.md)