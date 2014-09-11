---
layout: post
title: "Cómo hacer un merge selectivo con git"
modified:
categories: git
excerpt: Como hacer un merge selectivo con git desde "big-branch" a "small-branch"
comments: true
tags: [git tech development]
image:
  feature:
date: 2014-09-11T18:09:27+02:00
---


<img src="https://camo.githubusercontent.com/233e6dc4e0b7db13dea0ee1164c9eaac875269db/687474703a2f2f636c2e6c792f31493233336a316c3258336333313143314331482f4769742d4c6f676f2d32436f6c6f722e706e67" style="margin: 2em 0 0" />

En el proyecto en el que trabajo, [dokify](https://dokify.net), ahora somos más que nunca en el equipo de desarrollo y cada vez tenemos más ramas abiertas al mismo tiempo.

Cuando nuestra *querida* tester [Lioba](https://twitter.com/liobadc) se toma unos días de descanso, acumulamos muchos [pull request](https://help.github.com/articles/using-pull-requests) pendientes de merge. Alguna de estas ramas son para pequeños fix, otras para grandes funcionalidades, pequeñas mejoras, etc...

##### Problema:

Un problema que nos ha surgido ha sido el tener una funcionalidad en la que estamos trabajando durante varias semanas, digamos `big-feature`, que contiene cambios relativos a la nueva funcionalidad y los pertinentes ajustes en el sistema, y al mismo tiempo tener ramas que necesitan de cambios que están en esta rama, por ejemplo `small-feature`.

Me planteo dos soluciones:

1. Dividir en piezas más pequeñas los cambios introducidos en la rama `big-feature` para conseguir así iteraciones más rápidas y evitar que el problema llegue a aparecer (o que la espera sea tan corta que no sea un problema).

2. Hacer un merge selectivo de los cambios desde `big-branch` a `small-branch`.

La primera opción creo que es buena en general, no solo para este escenario. Sin embargo habrá muchos escenarios en los que complique más el hecho de separar `big-branch` en varias ramas algo que claramente es una sola feature. Sea como sea, parece bastante claro que debemos usar la segunda idea.

##### Primera idea:

{% highlight css %}git cherry-pick{% endhighlight %}

Para copiar a `small-branch` aquellos commits que necesitamos. Pero no nos resuelve el problema realmente, porque queremos el resultado final que tiene `big-branch` en este momento, y buscar todos los commits que se han hecho para llegar al estado actual puede llevarnos mucho tiempo.

##### Segunda idea:

Como casi siempre, en google hay alguien a quién ya le ha pasado esto, y en [este caso](http://jasonrudolph.com/blog/2009/02/25/git-tip-how-to-merge-specific-files-from-another-branch/) tuvieron muchas más ideas.

##### Solución:

La solución más sencilla, al menos que haya encontrado, parece ser:

{% highlight css %}
git checkout source_branch <paths>...
{% endhighlight %}

Una vez tenemos los ficheros que necesitamos, todos los cambios en estos ficheros quedan añadidos para commit, para asegurarnos de que enviamos solo los cambios que necesitamos:

{% highlight css %}
git reset && git add -AN
{% endhighlight %}

Y acto seguido, podemos escoger de forma selectiva los cambios que queremos:

{% highlight css %}
git commit -p
{% endhighlight %}


Y vosotros... **¿Tenéis este problema? ¿Qué solución le habéis dado?**
