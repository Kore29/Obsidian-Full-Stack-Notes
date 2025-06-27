En los artículos anteriores hemos visto que son los **eventos Javascript** y como gestionarlos a través de código HTML, o a través de código Javascript, utilizando la API del DOM. Sin embargo, como hemos visto, la forma más recomendable es hacer uso del método `.addEventListener()`, el cuál es mucho más potente y versatil para la mayoría de los casos.

- Con `.addEventListener()` se pueden añadir fácilmente **múltiples** funcionalidades.
- Con `.removeEventListener()` se puede **eliminar** una funcionalidad previamente añadida.
- Con `.addEventListener()` se pueden indicar ciertos **comportamientos** especiales.

## El método `.addEventListener()`

El método `.addEventListener()` nos permite empezar a escuchar un **evento** concreto, y en el caso de que ocurra, ejecutar la función asociada. De forma opcional, se le puede pasar un tercer parámetro  con ciertas opciones, que veremos más adelante:

|Método|Descripción|
|---|---|
|`.addEventListener(``event,``func)`|Escucha el evento `event`, y si ocurre, ejecuta `func`.|
|`.addEventListener(``event,``func,``options)`|Idem, pasándole ciertas opciones.|

Para verlo en acción, vamos a crear a continuación el mismo ejemplo de apartados anteriores, donde escucharemos un evento de tipo `click` para que se ejecute una función cuando ocurra:

- 1️ Buscamos el elemento que tendrá el evento, en este caso `<button>`
- 2️ Creamos una función `action()` que realizará la acción deseada.
- 3️ En el botón, escuchamos el evento `click` y le asociamos la función `action`.

```html
<button>Click me</button>

<script>
const button = document.querySelector("button");

function action() {
  alert("Hello!");
};

button.addEventListener("click", action);
</script>
```

Ten muy en cuenta los siguientes detalles:

- ✨ En el **primer parámetro** indicamos el nombre del evento, en nuestro ejemplo, `click`.
- ✨ Con `.addEventListener()` no se precede con `on` y se escriben en minúsculas, sin camelCase.
- ✨ En el **segundo parámetro** indicamos la función a ejecutar al ocurrir el evento.

SimplificandoMúltiples listenersOpciones

### Arrow functions en `.addEventListener()`

Si estás empezando en Javascript, es posible que el código anterior lo veas más claro que el siguiente código que vamos a ver. Sin embargo, es muy habitual escribir los eventos de esta otra forma:

```js
const button = document.querySelector("button");

button.addEventListener("click", function() {
  alert("Hello!");
});
```

En lugar de tener la función definida fuera, la utilizamos en el propio `.addEventListener()` y nos ahorramos ponerle un nombre, es decir, utilizamos una **función anónima**. Si prefieres utilizar las [funciones flecha](https://lenguajejs.com/javascript/fundamentos/funciones/#arrow-functions) de Javascript, quedaría incluso más legible:

```js
const button = document.querySelector("button");

button.addEventListener("click", () => {
  alert("Hello!");
});
```

De hecho, cuando tenemos un código muy corto, podemos incluso ahorrarnos las llaves en las [arrow function](https://lenguajejs.com/javascript/funciones/arrow-functions/) y escribirlo todo en una sola línea.

### Múltiples listeners

El método `.addEventListener()` permite asociar **múltiples funciones a un mismo evento**, algo que, aunque no es imposible, es menos sencillo e intuitivo en las modalidades de gestionar eventos que vimos anteriormente.

Observa que en este ejemplo, hemos añadido una clase `.red` de CSS, que coloca el color de fondo del botón en rojo. Además, hemos creado dos funcionalidades:

- 1️ `action`, que muestra un mensaje de saludo
- 2️ `toggle`, que añade o quita el color rojo del botón

```html
<button>Saludar</button>

<style>
.red { background: red }
</style>

<script>
const button = document.querySelector("button");
const action = () => alert("Hello!");
const toggle = () => button.classList.toggle("red");

button.addEventListener("click", action);         // Hello message
button.addEventListener("click", toggle);         // Add/remove red CSS
</script>
```

Al pulsar el botón se efectuan ambas acciones (_ya que hay dos listeners en escucha_). Primero veremos el mensaje de alerta, y luego, el color rojo se añade o se quita.

### Opciones de `.addEventListener()`

Al utilizar el método `.addEventListener()`, se puede indicar un tercer parámetro opcional. Se trata de un  opcional en el cual podemos indicar alguna de las siguientes opciones para modificar alguna característica del listener en cuestión que vamos a crear:

|Opción|Descripción|
|---|---|
|`capture`|El evento se dispara al inicio (_capture_), en lugar de al final (_bubble_). Ver más adelante.|
|`once`|Sólo ejecuta la función una primera vez. Luego, elimina el listener.|
|`passive`|La función nunca llama a `.preventDefault()`, mejorando así el rendimiento.|
|`signal`|Asocia una señal al listener para poder cancelarlo. Ver tema de [AbortController](https://lenguajejs.com/eventos/performance/abortcontroller/).|

Repasemos cada una de estas opciones:

- La opción `capture` nos permite modificar la **modalidad** en la que se escuchará el evento (_capture/bubbles_). Básicamente, lo que hace es modificar en que momento se analiza el evento. Profundizaremos en esto un poco más adelante, en el tema de **propagación de eventos**.
    
- La opción `once` nos permite indicar que el evento se procesará **solo la primera vez** que se dispare el evento. Internamente, lo que hace es ejecutarse una primera vez y luego llamar al `.removeEventListener()`, eliminando el listener una vez ha sido ejecutado.
    
- La opción `passive` nos permite crear un **evento pasivo** en el que indicamos que nunca llamaremos al método `.preventDefault()` para alterar el funcionamiento del evento. Esto puede ser muy interesante en temas de **rendimiento** (_por ejemplo, al hacer scroll en una página_), ya que los eventos pasivos son mucho menos costosos.
    
- La opción `signal` nos permite asociar al listener una señal que podemos utilizar posteriormente para cancelar eventos en grupo de forma eficiente. Lo veremos más adelante en el artículo sobre [AbortController](https://lenguajejs.com/eventos/performance/abortcontroller/).