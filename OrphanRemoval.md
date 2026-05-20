## 🔙 [***Volver***](./persistencia.md)

---

# 👻 El Superpoder de `orphanRemoval`
## El detector de olvidados

Este superpoder sirve para detectar:

# 🗑️ Datos abandonados

en la base de datos.

---

# 🎮 La historia del Local y Juan

Imagina que:

🎮 el Local de videojuegos

tiene un gerente llamado:

👨‍💼 Juan.

Pero un día el Local decide despedir a Juan y contratar a:

👨‍💼 Luis.

Entonces el Local:

- borra a Juan,
- colocando a Luis como nuevo gerente.

---

# 🤔 La gran pregunta

¿Qué hacemos con Juan?

Ahí entra:

`orphanRemoval`

---

# 🚫 `orphanRemoval = false`
## El olvidado sigue existiendo

Si usas:

`orphanRemoval = false`

cuando el Local elimina a Juan de la relación:

❌ Juan NO se borra de la base de datos.

---

# 👻 ¿Qué ocurre entonces?

Juan queda:

- solo,
- abandonado,
- sin Local,
- pero todavía ocupando espacio.

Se convierte en:

# 👻 Un dato huérfano

---

# ⚠️ Problema real

Con el tiempo la base de datos puede llenarse de:

- datos inútiles,
- relaciones rotas,
- y registros olvidados.

---

# ✅ `orphanRemoval = true`
## El limpiador automático

Ahora imagina que activas:

`orphanRemoval = true`

---

# 💡 ¿Qué hace Spring?

Cuando el Local elimina a Juan de la relación:

Spring detecta que:

👨‍💼 Juan quedó solo.

Entonces automáticamente:

# 🗑️ lo elimina de la base de datos

---

# 🧠 Idea fácil

Si el padre:

🎮 Local

abandona al hijo:

👨‍💼 Juan

Spring lo limpia automáticamente.

---

# 📌 ¿Dónde se coloca `orphanRemoval`?

## ✅ SOLO en la entidad fuerte

En este caso:

# 🎮 `Local`

---

# ⚠️ ¿Por qué?

Porque el Local es quien controla la relación.

El Local decide:

- contratar,
- cambiar,
- o despedir gerentes.

---

# ❌ ¿Por qué NO va en `Gerente`?

Porque el Gerente:

- no controla el Local,
- ni puede abandonar al edificio.

No tendría sentido que:

👨‍💼 Juan

decidiera borrar automáticamente:

🎮 todo el Local.

---

# 💻 Ejemplo conceptual

## 🎮 Entidad principal

```java
@OneToOne(orphanRemoval = true)
private Gerente gerente;
```

---

## 👨‍💼 Entidad secundaria

```java
@OneToOne(mappedBy = "gerente")
private Local local;
```

---

# ⚡ ¿Cuándo se usa mucho?

`orphanRemoval`
es muy útil cuando:

- los datos dependen completamente del padre,
- y no deben existir solos.

---

# 💡 Ejemplos comunes

| Padre | Hijo |
|---|---|
| Usuario | Perfil |
| Pedido | Factura |
| Local | Gerente |

---

# 🎯 Resumen fácil

| Configuración | Resultado |
|---|---|
| `false` | El huérfano sigue existiendo |
| `true` | Spring lo elimina automáticamente |

---

# 💡 Idea final

`orphanRemoval`
funciona como:

# 🧹 Un limpiador automático

que elimina datos olvidados para mantener limpia la base de datos.

---

## 🔙 [***Volver***](./persistencia.md)