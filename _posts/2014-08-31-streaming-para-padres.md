---
layout: post
title: "Streaming Para Padres"
modified:
categories:
excerpt: "Llevo unos días dándole vueltas a un asunto: Cómo conseguir que tus padres adopten un sistema de streaming para ver contenidos."
tags: ["tecnología", "streaming", "popcorn", "series", "video"]
image:
  feature:
date: 2014-09-04T12:00:00+02:00
---

Llevo unos días dándole vueltas a un asunto: *Cómo conseguir que tus padres¹ adopten un sistema de streaming para ver contenidos*.


Me gustaría poder definir una unidad de medida que representara la curva de aprendizaje de un sistema (sea aplicación, hardware, etc...) para poder medir esto de forma más objetiva. Estoy convencido de que el **proceso de streaming** con los medios mas habituales de hoy en día **vs** el **botón rojo del mando** de la tv, a pesar de la <strike>mierda</strike> baja calidad de la tv que tenemos, hace que a la mayoría de la gente no le compense el cambio, por lo complejo que pueda parecer en un principio o [la fricción](http://us2.campaign-archive1.com/?u=374c664073e1a1fa3deca53b4&id=860acd760e&e=ea250d343d) que suponga. Sin entrar a hablar del hábito, que siempre es difícil de cambiar,
he pensado en un combinación que simplificaría bastante el proceso.

Para poder utilizar el sistema que planteo sería necesario disponer de un [Chromecast](http://www.google.com/chrome/devices/chromecast/) (que [al precio que está en amazon](http://www.amazon.es/gp/product/B00I4WQ8VG/ref=as_li_qf_sp_asin_tl?ie=UTF8&camp=3626&creative=24790&creativeASIN=B00I4WQ8VG&linkCode=as2&tag=devdepue-21), yo ya he pedido el mío).

![Chrome cast photo](http://www.google.com/chrome/assets/common/images/chromecast/marquee-promo-apps-device_2x.png)

Una vez con el cacharro en casa, hablemos de [PopcornTime](http://popcorntime.io/). Hace no mucho [añadían soporte para Chromecast](http://blog.time4popcorn.eu/chromecast-support-check/) y al enterarme me dije: *quizás ahora sí pueda explicar a mi padre como ver series² en streaming, vamos a echarle un vistazo*.

Demasiado bueno para ser verdad, solo una app, un buscador y a ver el capítulo... Pero no. Todo el contenido (que he podido encontrar) está en inglés. Sí, tienes la opción de integrar los subtítulos directamente, pero ya sabes la respuesta.

![what the fuck](http://ljdchost.com/yrkHw5M.gif)

Vale, seguramente tu padre lo diría en español, pero creo que se entiende. Así que nada, descartado... ¿O no? Quizás si la app pudiera reproducir un torrent cualquiera... puede que el invento siga siendo válido. Así que trás una búsqueda en google me encuentro con que PopcornTime es capaz de hacer streaming de cualquier torrent, [solo hay que arrastrar el torrent](http://www.reddit.com/r/PopCornTime/comments/258gfb/request_opening_a_torrent_file_in_popcorn_time/chepgl6) a la interfaz y *voilà*.

![sons-of-anarchy](/images/sons-of-anarchy.png)

Incluso si simplemente copiamos en el portapapeles un *[magnet link](http://es.wikipedia.org/wiki/Magnet)*, vamos a la interfaz de la aplicación y pegamos con `CTRL+V`, obtendremos el mismo resultado.

Aún me queda por encontrar un método eficaz para la búsqueda de torrents (recordemos, para padres). Entre las opciones que se me ocurren la que mas me está gustando es hacer una app tipo [desktoppr](https://www.desktoppr.co/). La idea sería: login con tu cuenta de dropbox, una interfaz web sencilla para seleccionar las series que sigues y automáticamente los archivos .torrent aparecerán sincronizados en tu pc. El proceso quedaría reducido únicamente a **abrir PopcornTime y arrastrar el capítulo que queremos ver**.

Si [eztv](http://eztv.it) tuviera contenido en español sería increíblemente sencillo hacer algo funcional, sobre todo por cosas como [esta](https://www.npmjs.org/package/eztv). He estado investigando para ver si existe algún portal de este tipo con contenido en nuestro idioma, pero no he encontrado nada que encaje para este propósito. [Kickass](http://kickass.to/) por ejemplo también tiene API, pero no hay una base de datos de episodios estructurada, con lo que a priori queda descartado.

Aún tengo que pulir este aspecto, pero creo que la cosa promete, ¿no?

**Probablemente continuará...**

<span style="font-size:0.7em">¹ Ante la duda, seas quien seas, hablamos de tus padres. Si tú; lector, eres padre, no te des por aludido.</span><br />
<span style="font-size:0.7em">² Por que aunque no tengo ni idea, me parece que ver series está bien... pero para ver pelis no lo useis eeee...</span>