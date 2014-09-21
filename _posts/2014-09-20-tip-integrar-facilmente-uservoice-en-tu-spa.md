---
layout: post
title: "Tip: integrar fácilmente UserVoice en tu SPA"
modified:
categories: tip
excerpt: Como integrar UserVoice en tu web app fácilmente
comments: true
tags: ['js', 'html', 'code', 'uservoice', 'widget']
image:
  feature:
date: 2014-09-20T21:01:02+02:00
---

En mi opinión [UserVoice](https://www.uservoice.com/) es un gran servicio. No sólo nos permite tener un formulario de contacto de la forma más simple posible, si no que integra todo un sistema de feedback y tickets para poder dar soporte a tus usuarios al mismo tiempo que estos valoran tu app y sugieren mejoras y cambios de forma muy usable e intuitiva.

Una de las cualidades que hace a este servicio popular es que para integrarlo en tu web no tienes que tocar ni una sola línea de JS. Simplemente añades su script a tu página, añades algunos atributos en tu HTML y listo. Por ejemplo:

{% highlight css %}
<a href="mailto:soporte@mycoolapp.com" data-uv-trigger>Contact</a>
{% endhighlight %}

Pero... ¿qué ocurre si lo dónde queremos integrar no es una web al uso? A día de hoy seguro que muchos trabajáis desarrollando [SPA](http://en.wikipedia.org/wiki/Single-page_application). Cuando carguemos contenido de forma dinámica añadir marcado a nuestro HTML **no funciona**.

##### Opciones

Por supuesto UserVoice tiene una API para trabajar con su widget que dispone de [varios mecanismos](https://developer.uservoice.com/docs/widgets/methods/). Podemos dejar que el widget nos genere su propio componente HTML:

{% highlight css %}
UserVoice.push(['addTrigger', {
    mode: 'contact',
    trigger_position: 'bottom-right'
}]);
{% endhighlight %}

O usar nuestro propio componente:

{% highlight css %}
UserVoice.push(['addTrigger', '#contact_us', {
    mode: 'contact'
}]);
{% endhighlight %}

Ponemos este pequeño código allí dónde cambiemos el contenido de nuestro HTML y listo, problema resuelto. Por ejemplo:

{% highlight css %}
// view:change is a custom event fired every time our HTML changes
$(document).on('view:change', function () {
    UserVoice.push(['addTrigger', '#contact_us', {
        mode: 'contact'
    }]);
});
{% endhighlight %}

Pero no es un resultado que me guste al 100%, debido a que si usamos los atributos HTML, lo cual es muy cómodo y sencillo de utilizar por todo el equipo, y al mismo tiempo definimos un comportamiento en JS, tendremos código duplicado y difícil de mantener.

##### Solución

He tenido que buscar en el script JS ofuscado para encontrar la que para mi es la mejora solución, ya que no viene documentada en la web de UserVoice:

{% highlight css %}
UserVoice.scan();
{% endhighlight %}

Quedando la versión definitiva así:

{% highlight css %}
// view:change is a custom event fired every time our HTML changes
$(document).on('view:change', function () {
    UserVoice.scan();
});

{% endhighlight %}


Podemos lanzar este comando cuando el contenido de nuestro vista cambie y conseguiremos que la configuración del widget siga en el marcado. Fácil, sencillo y para <span style="text-decoration:line-through;">toda la familia</span> **todo el equipo**.
