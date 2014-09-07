---
layout: post
title: "Así Serán Los Módulos en ECMAScript 6"
date: 2014-09-07T18:52:26+02:00
comments: true
---

Parece que ya tenemos la versión definitiva de como se trabajará con módulos en **ECMAScript 6**. Según cuenta [Axel Rauschmayer](http://rauschma.de/) se ha buscado lo mejor de los dos sistemas que se usan actualmente:

- La versión de servidor ([CommonJS Modules](http://spinejs.com/docs/commonjs)), que estaremos acostumbrados a ver en [NodeJS](http://nodejs.org/)
- La versión para browsers ([AMD](http://en.wikipedia.org/wiki/Asynchronous_module_definition)), que usan librerías como [RequireJS](http://requirejs.org/)

El resultado es que nos acostumbraremos a ver cosas como estas:

{% highlight css %}
//------ lib.js ------
export const sqrt = Math.sqrt;
export function square(x) {
    return x * x;
}
export function diag(x, y) {
    return sqrt(square(x) + square(y));
}
{% endhighlight %}


{% highlight css %}
import { square, diag } from 'lib';
console.log(square(11)); // 121
console.log(diag(4, 3)); // 5
{% endhighlight %}

Además de esta forma explícita de trabajar con módulos dispondremos de una API para la carga de estos:

{% highlight css %}
System.import(
    ['module1', 'module2'],
    function (module1, module2) {  // success
        ...
    },
    function (err) {  // failure
        ...
    }
);
{% endhighlight %}

Además empezaremos a ver la nueva etiqueta `<module>` que reemplazará a `<script>` cuando necesitemos cargar módulos desde HTML

Esto es solo un pequeño extracto de lo que está por venir, puedes leer toda la información aquí: [http://www.2ality.com/2014/09/es6-modules-final.html](http://www.2ality.com/2014/09/es6-modules-final.html)