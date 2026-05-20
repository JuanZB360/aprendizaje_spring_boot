## 🔙 [***Volver***](./persistencia.md)

---

# 🔄 El Superpoder de `cascade`
## El efecto dominó
### “Lo que me pasa a mí, te pasa a ti”

Imagina que:

# 🎮 El Local
y
# 👨‍💼 El Gerente Juan

son mejores amigos.

Están tan conectados que lo que le ocurra al Local también puede afectar automáticamente a Juan.

Eso es exactamente lo que hace:

```text
cascade
```

---

# 💡 ¿Qué controla `cascade`?

Decide si las acciones realizadas sobre una entidad también deben aplicarse automáticamente a la entidad relacionada.

---

# 🧠 Idea fácil

Si el:

🎮 Local

hace algo…

¿Juan también debe hacerlo?

```text
cascade
```

responde esa pregunta.

---

# 🏗️ `CascadeType.PERSIST`
## El poder de crear juntos

Imagina que construyes un Local nuevo y contratas a Juan como gerente.

Sin `cascade`:

❌ debes registrar:

- el Local,
- y Juan,
- por separado.

---

## ✅ Con `CascadeType.PERSIST`

Cuando guardas el Local:

🎮 ➜ 👨‍💼

Spring automáticamente también guarda a Juan.

---

# 💡 Idea rápida

Guardar el padre:

✅ guarda automáticamente al hijo.

---

# ❌ `CascadeType.REMOVE`
## El poder de desaparecer juntos

Ahora imagina que la empresa decide destruir el Local.

---

## ✅ Con `CascadeType.REMOVE`

Cuando eliminas el Local:

🎮 ❌ ➜ 👨‍💼 ❌

Juan también desaparece automáticamente de la base de datos.

---

# ⚠️ ¿Por qué tiene sentido?

Porque:

- el Local es la entidad principal,
- y Juan depende del Local.

No tendría sentido que:

❌ borrar al Gerente destruya todo el Local.

---

# 🔄 `CascadeType.MERGE`
## El poder de actualizar juntos

Imagina que:

- pintan el Local de verde,
- y Juan cambia su uniforme a verde.

---

## ✅ Con `CascadeType.MERGE`

Cuando actualizas el Local:

🎮 🔄 ➜ 👨‍💼 🔄

Spring también actualiza automáticamente a Juan.

---

# 🔥 `CascadeType.ALL`
## El paquete completo

Este es el superpoder máximo.

Activa TODOS los poderes automáticamente.

Incluye:

- guardar,
- actualizar,
- eliminar,
- y más.

---

# 💡 Idea rápida

Todo lo que le pase al Local:

también le pasa a Juan.

---

# 📌 ¿Dónde se coloca `cascade`?

Normalmente se coloca en:

# 🎮 La entidad fuerte o principal

En este caso:

## ✅ `Local`

Porque:

- el Local controla la relación,
- y el Gerente depende del Local.

---

# ⚠️ ¿Por qué NO suele ponerse en `Gerente`?

Porque no tendría sentido que:

👨‍💼 borrar al Gerente

también destruya:

🎮 todo el Local.

---

# 🧠 Regla fácil para recordar

| Entidad | Rol |
|---|---|
| `Local` | Padre / entidad fuerte |
| `Gerente` | Hijo / entidad dependiente |

---

# 💻 Ejemplo conceptual

## 🎮 Entidad principal

```java
@OneToOne(cascade = CascadeType.ALL)
private Gerente gerente;
```

---

## 👨‍💼 Entidad secundaria

```java
@OneToOne(mappedBy = "gerente")
private Local local;
```

---

# 🎯 Resumen fácil

| Tipo | ¿Qué hace? |
|---|---|
| `PERSIST` | Guarda automáticamente |
| `REMOVE` | Elimina automáticamente |
| `MERGE` | Actualiza automáticamente |
| `ALL` | Activa todos los poderes |

---

# 💡 Idea final

`cascade` es como un:

# ⚡ Efecto dominó

Si algo le pasa al padre:

🎮 ***Local***

también puede pasarle automáticamente al hijo:

👨‍💼 ***Gerente***

---

## 🔙 [***Volver***](./persistencia.md)