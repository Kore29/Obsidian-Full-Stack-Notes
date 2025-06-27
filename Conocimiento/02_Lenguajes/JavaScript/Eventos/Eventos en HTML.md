#eventos #js #html #lenguajes #frontend

Probablemente, la forma más fácil de trabajar con [eventos Javascript](Eventos en JavaScript) es mediante **atributos de una etiqueta HTML**. Sin embargo, aunque es la más sencilla, también es la menos recomendable, pero es un buen punto de partida para comenzar a trabajar con eventos.

## Manejar eventos desde HTML

Recordemos que un **evento Javascript** es una característica especial que ha sucedido en nuestra página y a la cuál le asociamos una funcionalidad, de modo que se ejecute cada vez que suceda dicha característica especial. Por ejemplo, el evento `click` se dispara cuando el usuario hace **click** en un elemento de nuestra página.

Imaginemos el siguiente código HTML:

```html
<button>Saludar</button>
```

En nuestro navegador nos aparecerá un botón con el texto «**Saludar**». Sin embargo, si lo pulsamos, no realizará ninguna acción ni tendrá funcionamiento. Esto ocurre porque por defecto, el botón no tiene funcionalidad, hay que dársela mediante Javascript.

Para solucionar esto, podemos asociarle un evento mediante un atributo:

```html
<button onClick="alert('Hello!')">Saludar</button>
```

En este ejemplo, estamos **escuchando** los eventos `click`:

- 1️⃣ Si el usuario hace click en el `<button>`, se disparará un evento `click` (_en ese elemento_).
- 2️⃣ El botón, con el atributo `onClick` (_cuando hagas click_), está escuchando el evento `click`
- 3️⃣ Si ocurre el evento, se ejecutará el código asociado en el valor del atributo HTML.

> El nombre del evento es **click**, sin embargo, en los atributos HTML se coloca siempre precedido de `on`. Las minúsculas/mayúsculas dan igual, aunque lo más habitual es usar [camelCase](https://lenguajejs.com/fundamentos/introduccion/convenciones-de-nombres/).

## Organizando la funcionalidad

El valor del atributo `onClick` llevará la funcionalidad en cuestión que queremos ejecutar cuando se produzca el evento. En nuestro ejemplo anterior, hemos colocado un `alert()`, pero lo habitual es que necesitemos ejecutar un fragmento de código más extenso, por lo que lo ideal sería meter todo ese código en una función, y en lugar del `alert()`, ejecutar dicha función:

```html
<script>
  function doTask() {
    alert("Hello!");
  }
</script>

<button onClick="doTask()">Saludar</button>
```

Ahora todo está un poco mejor organizado. Sin embargo, no es muy habitual tener bloques `<script>` de código Javascript en nuestro HTML, sino que lo habitual suele ser externalizarlo en ficheros `.js` para dividir y organizar mejor nuestro código.

Hagamos algunos cambios más teniendo esto en cuenta:

```html
<script src="tasks.js"></script>
<button onClick="doTask()">Saludar</button>
```

Bien. Ahora tenemos nuestro código Javascript en un fichero externo `tasks.js`, y en el atributo `onClick` llamamos a una función `doTask()`, que es la que tendremos que implementar en el `tasks.js`.

Sin embargo, ahora aparece un nuevo problema que quizás aún no sea muy evidente. En nuestro `<button>` estamos haciendo referencia a una función `doTask()` que, aparentemente, confiaremos en que se encuentra declarada dentro del fichero `tasks.js`.

Esto podría convertirse en un problema, si posteriormente, o dentro de cierto tiempo, nos encontramos modificando código en el fichero `tasks.js` y le cambiamos el nombre a la función `doTask()`, ya que podríamos olvidar que hay una llamada a una función Javascript en uno (_o varios_) ficheros `.html`. Se dice que **el código HTML está acoplado al código Javascript** (_depende uno del otro_).

> Por esta razón, suele ser buena práctica no incluir llamadas a funciones Javascript en nuestro código `.html`, sino que es mejor hacerlo desde el fichero externo `.js`, localizando los elementos del DOM con un `.querySelector()` o similar.

En resumen:

- Gestionar **eventos Javascript** desde HTML es muy sencillo.
- Hay que tener en cuenta que «mezclamos» (acoplamos) código Javascript dentro de HTML.
- Para que el código sea más legible y fácil de mantener, se recomienda gestionar los eventos únicamente desde Javascript, como veremos en los siguientes artículos.