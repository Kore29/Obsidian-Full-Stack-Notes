#eventos #js #lenguajes

En los artículos anteriores explicamos [que es un evento Javascript](Resumen.md) y como [gestionar eventos a través de HTML](Eventos en HTML), concretamente, desde un atributo de una etiqueta HTML. Sin embargo, vimos que uno de los problemas de esta forma de trabajar es que estamos **acoplando el código HTML al código Javascript**.

En general, es preferible manejar todos los eventos desde nuestros ficheros `.js`, evitando tener nombres de funciones sueltas en el HTML, que luego tendremos que actualizar y mantener.

## Manejar eventos desde Javascript

Existen varias formas de **gestionar eventos** sin necesidad de hacerlo desde nuestro `.html` mediante un atributo en línea. Principalmente, son tres formas:

### Utilizando listeners

La forma preferida de trabajar con eventos desde Javascript es utilizar el evento `.addEventListener()` sobre nuestro elemento particular, de esta forma podremos escuchar el evento indicado de forma alternativa a usar el `onClick`:

- 1️ Buscar el elemento en el DOM
- 2️ Escuchar el evento en el elemento
- 3️ Asociar la función al evento del listener

```html
<button>Saludar</button>

<script>
const button = document.querySelector("button");

button.addEventListener("click", function () {
  alert("Hello!");
});
</script>
```

Observa que previamente, en lugar de añadir el atributo `onClick` al `<button>`, lo hemos localizado mediante `querySelector()`. Esto podríamos hacerlo mediante una clase, pero en este ejemplo lo hemos hecho directamente con el botón, para simplificar.

Sin embargo, es posible que leyendo código te encuentres con otras formas diferentes de asociar eventos desde Javascript, que aunque son menos utilizadas, es importante conocerlas.

### Utilizando BOM

El **BOM** (_Browser Object Model_) es un mecanismo tradicional de los navegadores para acceder a ciertos elementos a través de propiedades Javascript. La idea es la misma que vimos al utilizar listeners, sólo que en esta ocasión haremos uso de una propiedad Javascript, a la que le asignaremos la función con el código asociado:

```html
<button>Saludar</button>

<script>
const button = document.querySelector("button");

button.onclick = function() {
  alert("Hello!");
}
</script>
```

Observa que en este caso, lo que hacemos es asignar una función con el código deseado en la propiedad `.onclick` del elemento `<button>`. Esta es una propiedad especial que tienen todos los elementos del DOM y que se ejecutan cuando sucede el evento en cuestión.

Ventajas y desventajas:

- 💖 Sencillo, corto y fácil de entender
- 💔 Si ya tenemos una función asignada, asignar una nueva sobreescribiría la anterior.
- 💔 Es un modelo antiguo. El BOM no es estándar y pueden existir navegadores que no lo soporten.

> La propiedad `.onclick` (_o del evento en cuestión_) siempre irá **en minúsculas**, ya que se trata de una propiedad Javascript, y Javascript es sensible a mayúsculas y minúsculas.

### Utilizando `setAttribute()`

En este caso, realmente lo que estamos haciendo es equivalente a añadir un atributo `onClick` en el `<button>` de nuestro HTML, solo que lo hacemos a través de la API de Javascript:

```html
<button>Saludar</button>

<script>
const button = document.querySelector("button");

const doTask = () => alert("Hello!");
button.setAttribute("onclick", "doTask()");
</script>
```

Si inspeccionamos el HTML del navegador, veremos que el método `.setAttribute()` lo que ha hecho es añadir el atributo `onClick` con el valor indicado en el HTML.

==En resumen:==

- A grandes rasgos, se trata de formas alternativas de gestionar eventos desde HTML, pero haciéndolo mediante Javascript para no depender de código acoplado en el HTML.
    
- En general, el método ideal a utilizar es `.addEventListener()`. Es el más cómodo, mantenible y legible, ya que carece de las desventajas de los demás. Profundizaremos en él en el siguiente artículo.