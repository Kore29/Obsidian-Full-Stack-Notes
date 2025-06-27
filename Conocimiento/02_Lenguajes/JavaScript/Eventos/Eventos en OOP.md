#eventos #poo #js #lenguajes #frontend

En el artículo anterior, vimos el método `.addEventListener()` y como podemos utilizarlo. Sin embargo, si tenemos que manejar múltiples eventos, el código se nos comienza a complicar y es difícil de organizar.

## Usar clases para escuchar eventos

Si tenemos que gestionar múltiples eventos, una forma de mantenerlo organizado sería crear una clase, donde cada método gestiona un tipo de evento. De esta forma podríamos crear una estructura de código cómoda de mantener y ampliar que se dedique a gestionar los eventos.

Vamos a traducirlo a un ejemplo, donde crearemos la clase `EventManager`. Mucho cuidado, porque hay una errata intencionada que explicaremos a continuación:

```js
class EventManager {
  constructor(element) {
    element.addEventListener("click", this.sendMessage());  /*       ❌ Error */
  }

  sendMessage() {
    alert("Has hecho click en el botón");
    return 42;
  }
}

const button = document.querySelector("button");
const eventManager = new EventManager(button);
```

Al hacer una instancia de clase con `new EventManager()`, le pasamos el botón por parámetro, de modo que el `constructor()` de la clase empezará a escuchar el evento y asociarlo con el método `sendMessage()` de la clase. Sin embargo, **hay un error en este código**.

> Es muy frecuente que se cometa este error. En el segundo parámetro del `.addEventListener()` se espera una función , sin embargo, lo que estamos haciendo realmente es **ejecutar una función antes** y luego pasarle lo que devuelva la función.

En este caso, en `sendMessage()` hemos puesto un `return 42`, por lo que realmente estaríamos haciendo un `.addEventListener("click", 42)`, que obviamente es incorrecto.

## Cómo definir las funciones

Hay varias formas de definir las funciones asociadas al evento del `addEventListener()`. Si no tienes claro como hacer referencia a los métodos de clases en el `addEventListener()`, lee esta parte antes de continuar con el artículo:

Vía arrow functionsVía referenciaVía bind

### Mediante arrow functions

La forma preferida hoy en día es utilizar **funciones flecha anónimas**. Como el segundo parámetro de `.addEventListener()` espera una función, podemos pasar una función flecha anónima que «envuelva» y ejecute la función que nos interesa y devuelva su resultado.

Al estar dentro de una [función flecha](https://lenguajejs.com/javascript/introduccion/funciones/#arrow-functions), no tiene concepto propio de `this`, por lo que no pierde el valor, y `this` sigue haciendo referencia a la clase del componente:

```js
class EventManager {
  constructor(element) {
    element.addEventListener("click", () => this.sendMessage());
  }

  sendMessage() {
    alert("Has hecho click en el botón");
    console.log(this);    // this = referencia a EventManager
  }
}

const button = document.querySelector("button");
const eventManager = new EventManager(button);
```

Además de ser mucho más legible, permite el paso de parámetros a la función de forma sencilla y clara.

### Mediante referencia a la función

Antes intentamos usar `this.sendMessage()`, pero vimos que es incorrecto porque no indicamos una función, sino que la ejecutamos e indicamos **el valor que devuelve**.

La forma correcta de hacerlo es **pasar la referencia a la función**. Es decir, lo mismo, pero omitir los paréntesis `()` para que no se ejecute:

```js
class EventManager {
  constructor(element) {
    element.addEventListener("click", this.sendMessage);
  }

  sendMessage() {
    alert("Has hecho click en el botón");
    console.log(this);    // this = referencia al <button>
  }
}

const button = document.querySelector("button");
const eventManager = new EventManager(button);
```

Esta opción tiene un **inconveniente**: Al pasarle la referencia a la función, el valor de `this` en `sendMessage()` cambia y pasa a ser el `<button>` y no la clase. Esto suele ser confuso y complejo. Además, al no poder añadirle los paréntesis, tampoco podríamos pasarle parámetros.

Por lo tanto, esta opción es sólo recomendable para métodos muy sencillos que no usen `this`.

### Mediante funciones con bind

Una forma intermedia entre las dos anteriores, es utilizar el método `.bind()`. ¿Qué hace esto realmente? Pues en pocas palabras, realiza una copia de la función que queremos ejecutar, y le pasa por parámetro el elemento al que va a apuntar `this`:

```js
class EventManager {
  constructor(element) {
    element.addEventListener("click", this.sendMessage.bind(this));
  }

  sendMessage() {
    alert("Has hecho click en el botón");
    console.log(this);    // this = referencia a EventManager
  }
}

const button = document.querySelector("button");
const eventManager = new EventManager(button);
```

De esta forma solucionamos el problema anterior, ahora `this` si hará referencia a la clase en cuestión, siendo posible ejecutar otros métodos o consultar y guardar información en las propiedades. Si queremos añadirle parámetros, basta con incluirlos después del `this` del primer parámetro del `.bind()`.

>  Esta era la solución usada en Javascript  y anteriores. Personalmente, me parece bastante confusa de entender y leer, por lo que prefiero evitarla a favor de las arrow functions.

## El método mágico `handleEvent`

Como hemos visto, aunque el trabajo con eventos no es especialmente complejo, dependiendo de la situación se puede complicar mucho. Además, cuando tenemos muchos eventos, se vuelve tedioso de organizar, y corremos el riesgo de que se vuelva muy complejo. Existe un **patrón Javascript** muy interesante y desconocido que permite organizar y administrar los eventos de una forma muy elegante.

En lugar de a una función , es posible asociar un evento a un objeto . Este objeto debe contener un método mágico `.handleEvent()`. Si lo hacemos, dicho método recibirá el evento y podremos gestionarlo desde su interior:

```js
const button = document.querySelector("button");

const eventManager = {
  handleEvent: function(ev) {
    if (ev.type === "click") {
      /* Lógica para evento onClick */
    } else if (ev.type === "mouseleave") {
      /* Lógica para evento onMouseLeave */
    }
    /* ... */
  }
}
button.addEventListener("click", eventManager);
button.addEventListener("mouseleave", eventManager);
```

Obviamente, la forma de manejar los eventos dependerá del programador. Hay quienes preferirán hacerlo con `if` anidados, otros con un `switch`, otros con un diccionario, etc.

## El método mágico `handleEvent` en clases

Como ya habrás imaginado, esto se puede trasladar a un objeto instanciado a partir de una **clase**. Podemos crear una clase, o incluso varias instancias del objeto, de forma que sea mucho más flexible y reutilizable para nosotros.

Ten en cuenta que `this` en el contexto del método `handleEvent()` apunta al propio objeto `eventManager`, por lo que podemos utilizarlo para acceder a otros métodos o propiedades del objeto:

```html
<button>Click me!</button>

<script>
const button = document.querySelector("button");

class EventManager {
  handleEvent(ev) {
    const methodName = `on${ev.type.charAt(0).toUpperCase() + ev.type.slice(1)}`;

    if (this[methodName]) {
      this[methodName](ev);
    } else {
      this.#defaultHandler(ev);
    }
  }

  onClick(type, element) {
    alert("¡Has hecho click!");
    console.log({ type, element });
  }

  onLeave(type, element) {
    alert("¡Has abandonado el botón!");
    console.log({ type, element });
  }

  #defaultHandler(ev) {
    console.log(`Evento no manejado: ${ev.type}`);
  }
}

const eventManager = new EventManager();
button.addEventListener("click", eventManager);
button.addEventListener("mouseleave", eventManager);
</script>
```

- 1️ El método `handleEvent` crea una constante `methodName` con el nombre del método a llamar.
- 2️ Se comprueba si existe el método, y si es así, se llama.
- 3️ Si no existe, se ejecuta el método privado `#defaultHandler()`.