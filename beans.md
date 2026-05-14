## 🔙 [***Volver***](./Readme.md)

---

# ¿Qué es un Bean?

Si los Starters son los “kits de piezas”, los Beans son los objetos reales que Spring saca de esas cajas, ensambla y mantiene funcionando dentro de tu aplicación.

En el mundo de Java tradicional, tú creas un objeto usando la palabra `new`:

**Ejemplo**

```bash
    Servicio servicio = new Servicio();
```


En Spring, tú no haces eso. Tú le pides a Spring que gestione el objeto por ti. Ese objeto gestionado es lo que llamamos un **Bean**.

---

# 1. El Ciclo de Vida: El Contenedor de Spring (IoC)

Imagina que Spring es un director de orquesta. Los Beans son los músicos.

- 🎼 El director decide cuándo entra un músico.
- 🎻 El director le entrega el instrumento que necesita.
- 🚪 El director decide cuándo se retira.

Este “lugar” donde viven y se administran los Beans se llama **Contenedor de Inversión de Control (IoC)**.

---

# 2. ¿Cómo se crea un Bean?

No todos los objetos de tu código son Beans. Un objeto se convierte en Bean cuando le ponemos una “etiqueta” (anotación) para que Spring lo reconozca.

## 🧩 `@Component`

Es la etiqueta general. Le dice a Spring:

> “Oye, este objeto es importante, gestiónalo tú”.

## 🛠️ `@Service`

Una etiqueta específica para clases que guardan la lógica de negocio.

## 🗄️ `@Repository`

Específica para clases que se comunican con la base de datos.

---

# 3. ¿Por qué dejar que Spring maneje mis objetos?

Usar Beans ofrece tres ventajas clave que todo programador de Backend busca:

## 🔌 Inyección de Dependencias

Si el “Bean A” necesita al “Bean B” para funcionar, tú no tienes que crearlo manualmente. Spring busca al “Bean B”, lo encuentra y se lo entrega al “Bean A” automáticamente.

## 💾 Memoria eficiente (Singleton)

Por defecto, Spring crea una sola instancia (una sola copia) de cada Bean para toda la aplicación. Esto ahorra mucha memoria en servidores grandes.

## 🔄 Desacoplamiento

Puedes cambiar cómo funciona un Bean sin tener que romper todo el código que lo usa, ya que Spring se encarga de las conexiones.

---

# 4. Ejemplo en la vida real

Imagina una aplicación de ventas:

- Tienes un Bean llamado `CalculadoraDeImpuestos`.
- Tienes otro Bean llamado `GeneradorDeFacturas`.

Cuando `GeneradorDeFacturas` necesita calcular un impuesto, no hace un `new Calculadora()`. Simplemente le dice a Spring:

> “Necesito el Bean de impuestos”.

Y Spring se lo inyecta listo para usar.

---

# ⚠️ Regla de Oro

> Si Spring no conoce tu clase como un Bean (porque olvidaste la anotación), no podrá inyectar nada y recibirás el famoso error `NullPointerException`.

---

## 🔙 [***Volver***](./Readme.md)