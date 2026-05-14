## 🔙 [***Volver***](./anotaciones.md)

---

# B. Para conectar piezas  
## 🔌 Inyección de Dependencias

# `@Autowired`
## 🔌 El conector automático de Spring

Si las anotaciones anteriores definen “quién es quién”
(el Chef, el Mesero, el Bibliotecario),
`@Autowired` es el cable que los conecta para que puedan trabajar juntos.

Spring utiliza esta anotación para gestionar automáticamente las dependencias entre componentes.

---

# ❓ ¿Qué significa “inyectar” una dependencia?

En programación tradicional, si una clase necesita otra clase, debes crearla manualmente usando `new`.

## 📌 Ejemplo tradicional

```java id="old-java-new"
UsuarioRepository repository = new UsuarioRepository();
```

Con Spring, ya no haces eso.

En lugar de crear el objeto manualmente, simplemente le dices a Spring:

```text
“Necesito esta pieza”.
```

Y Spring:

- la busca,
- la crea si es necesario,
- y la coloca automáticamente.

A esto se le llama:

### 💉 Inyección de Dependencias

---

## ⚙️ ¿Cómo funciona en la práctica?

Imagina que estás dentro de un @Service
y necesitas usar un @Repository.

En lugar de crear el repositorio manualmente,
simplemente declaras la variable y usas @Autowired.

---

## 🧠 Ejemplo conceptual

```java
@Service
public class UsuarioService {

    @Autowired
    private UsuarioRepository usuarioRepository;

}
```
## ✅ ¿Qué ocurre aquí?

Spring hace automáticamente esto:

- busca un Bean de tipo UsuarioRepository,
- lo encuentra dentro del contenedor,
- y lo conecta automáticamente con UsuarioService.

---

## 🚀 ¿Por qué es tan importante?
### ❌ Adiós al new

Ya no necesitas llenar el código de inicializaciones manuales.

Spring gestiona el ciclo de vida de los objetos por ti.

---

## 🔄 Piezas intercambiables

Como Spring es quien entrega las dependencias,
puedes cambiar implementaciones sin modificar las clases que las usan.

---

## 🛡️ Menos errores de configuración

Spring verifica que todas las dependencias necesarias existan antes de iniciar la aplicación.

---

## 🎭 Analogía del mundo real

Imagina que estás construyendo una casa.

### 👷 Personajes
- l Electricista → @Service
- El Destornillador → @Component
- Spring → el asistente invisible

---

## 🔧 ¿Qué ocurre?

El electricista necesita un destornillador.
Pero él NO lo fabrica.
Simplemente extiende la mano y dice:

```java
@Autowired
```
Y Spring automáticamente coloca la herramienta en su mano.

---

## ⚠️ Nota importante para principiantes

Para que **@Autowired** funcione, la clase que intentas inyectar DEBE ser conocida por Spring.

Es decir, debe tener una anotación como:

- ***@Component***
- ***@Service***
- ***@Repository***
- ***@Controller***
- ***@RestController***

---

## ❌ ¿Qué pasa si Spring no conoce la clase?

Spring no podrá encontrar el objeto dentro del contenedor
y mostrará errores de dependencia.

Porque el “asistente invisible” no sabrá dónde encontrar esa pieza.

---

## 🧠 Idea clave

***@Autowired*** conecta automáticamente los componentes de la aplicación.

Gracias a esto:

- las clases trabajan juntas,
- el código queda más limpio,
- y Spring controla todas las dependencias.

---

## 📌 Resumen rápido
| Concepto          | Significado                          |
| ----------------- | ------------------------------------ |
| `@Autowired`      | Inyección automática de dependencias |
| Función principal | Conectar Beans automáticamente       |
| Evita usar        | `new`                                |
| Requiere          | Beans registrados en Spring          |
| Personalidad      | El cable que conecta el sistema      |

---

## 🔙 [***Volver***](./anotaciones.md)