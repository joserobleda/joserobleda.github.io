---
layout: post
title: "Encontrar la última versión que existió de un archivo en git"
modified:
categories: git
excerpt: Si trabajas habitualmente con git seguro que en alguna ocasión se te ha presentado la necesidad de recuperar código
comments: true
tags: []
image:
  feature:
date: 2015-01-12T15:35:25+01:00
---

Si trabajas habitualmente con git seguro que en alguna ocasión se te ha presentado la necesidad de recuperar código de un archivo que ya no existe en el repositorio.

Con este comando podrás visualizar rapidamente la última versión que existió antes de ser eliminado:

````
git show $(git rev-list --max-count=1 --all -- PATH/TO/FILE)^:PATH/TO/FILE
````

Quizás exista una versión en la que no sea necesario repetir la ruta del archivo, los comentarios están abiertos por si alguien conoce una forma mejor ;)