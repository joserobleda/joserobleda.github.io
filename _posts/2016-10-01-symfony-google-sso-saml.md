---
layout: post
title: "Symfony + Google Apps: SSO para tu app con SAML"
excerpt: Paso a paso, como configurar SSO para los usuarios de tu dominio en Google Apps
comments: true
tags: [php, dev, saml, google, symfony]
categories: symfony
image:
  feature: feature-code.jpeg
  credit: Ilya Pavlov
  creditlink: https://unsplash.com/@ilyapavlov
date: 2016-10-02T12:05:00+01:00
---

Hay miles de posts que hablan sobre como funciona los sitemas de autenticación mediante OAuth, el típico *login con Facebook, Google, Github, loQueSea*. Sin embargo, en ciertas ocasiones, este tipo de soluciones no resuelven bien ciertos problemas. 


### Casi cualquier app necesita un backoffice

A estas alturas ya casi todos nos hemos enfrentado a esto. Tenemos una app que hemos desarrollado pensando en que escale lo máximo posible, pero siempre hay un aspecto que require de una acción manual. Ya sea por un error o reglas de negocio, siempre aparece algo que nos genera la necesidad de crear un backoffice para ejecutar estas operaciones. Ante esto, el razonamiento más común suele ser:

1. Problema: Tengo que realizar alguna operacion manual  
--> Solución: Implementar un backoffice
2. Problema: Gestión de los usuarios del backoffice  
--> Solución: Utilizaré a los usuarios de mi app
3. Problema: Controlar que usuarios de mi app acceso  
--> Solución: Aplicar roles solo a ciertos usuarios

Nada del otro mundo, ¿verdad? Pues depende. **Si quieres cumplir con ciertas normativas** como la de la LOPD o ciertas normas ISO, desde el momento en que hagas esto **deberás describir todo tu sistema de gestión de usuarios en detalle**. Deberás describir desde qué es una contraseña segura y como te aseguras de que tus usuarios las usan, hasta el algoritmo de cifrado que usas para almacenarlas.

¿Qué hay de malo en esto? Nada en absoluto en el aspecto técnico. Pero **no es un requisito de tu negocio y vas a tener que trabajar bastante en ello**. No pretendo que nadie descuide la seguridad de su aplicación, pero **existen otras opciones que aumentan la seguridad y que implican un coste menor**, tanto de implantación como de mantenimiento.


### Single Sing On con SAML

O dicho más largo, *Single Sign On* con *Security Assertion Markup Language*. 

**SAML es un estándar abierto para identificar y autorizar a usuarios entre un proveedor de indentidad y un proveedor de servicios**.
En nuestro caso tenemos un *backoffice* que da servicio a los usuarios, así que tenemos claro quién es el proveedor de servicios, pero... ¿Quién jugaría el rol de proveedor de identidad? Pués depende.

Dependiendo de cada proyecto, la base de usuarios que usará el backoffice podría ser diferente. Quizás te resultaría útil  que los usuarios provinieran de un LDAP o quizás directamente de Github. Una opción bastante común en las empresas es usar Google Apps, así que este será el proveedor de identidad que usaremos en este post.

La idea es implementar un sistema de login único, es decir, **utilizar el mismo usuario de Google Apps para hacer login en nuestro backoffice**. De esta manera toda la gestión de contraseñas y usuarios la delegamos en Google, simplificando la implementación y el mantenimiento.

### Paso 1: Definiendo nuestra propia Google App

Lo primero que debemos hacer crear una "custom app" en Google Apps. Desde el panel de adminsitración vamos a Apps y desde ahí a SAML apps:

<img src="/images/saml-google-apps/apps.png" width="200px" />
&nbsp; 👉 &nbsp;
<img src="/images/saml-google-apps/saml.png" width="200px" />

Pulsamos en el botón de "añadir":

![](/images/saml-google-apps/add.png)

Y en el modal, pulsamos en "Setup my own custom app"

![](/images/saml-google-apps/choose.png)

En el siguiente paso optaremos por la opción 2, es decir, descargaremos el *IDP metadata*. Este archivo contiene la información necesaria para para poder configurar la autenticación de nuestra app. En este ejemplo vamos a guardar el fichero dentro de la carpeta de configuración de nuestra app Symfony en la ruta `app/config/saml/google-apps.com.xml`.

![](/images/saml-google-apps/download.png)

Ahora simplemente tenemos que introducir los datos de nuestra app: Su nombre, una descripción y, si queremos, un logo.

![](/images/saml-google-apps/info.png)

El siguiente paso será configurar los datos del proveedor de servicio, es decir, nuestro backoffice. 

![](/images/saml-google-apps/info.png)

A continuación tenemos que introducir los relativos a nuestro backoffice.
Google no da demasiados detalles de que significa cada campo en esta pantalla, pero tan solo dos datos son obligatorios:

**ACS (Assertion Consumer Service)**: Será la URL dónde se verificará si el login es correcto. Esta ruta la definiremos más adelante en nuestra app como `/backoffice/saml/login_check`

**Entity ID**: Será un identificador único que definirá a nuestra app. Aquí ponemos la URL de nuestro backoffice, de manera que nos aseguramos de que es un dato único y además fácil de recordar.

El resto de campos los dejaremos intactos. Es posible que según tus necesidades te interese configurarlos, pero para este ejemplo no es necesario.

![](/images/saml-google-apps/provider.png)


Cómo último paso podremos configurar un mapeo entre los campos que utiliza Google y los que utilizaremos en nuestra app. Como en este caso no necesitamos ninguno especial, podemos continuar sin más y finializar con el proceso.

![](/images/saml-google-apps/mapping.png)

Solo nos quedará activar esta app para todos los usuarios o para los grupos que queramos.

![](/images/saml-google-apps/config.png)

Y listo. Pasamos a implementar SAML en nuestra app.


### SAML con Symfony

Para este ejemplo asumo que tenemos una instalación limpia de Symfony 3.

Symfony aún no tiene soporte para autenticación mediante SAML, al menos no todavía. [Javier Eguiluz](https://twitter.com/javiereguiluz) ha propuesto incluir soporte a este estándar en el core hace unos pocos días. Puedes [echarle un vistazo a la issue aquí](https://github.com/symfony/symfony/issues/19992). Pero esto no es un problema, *there is a bundle for that!!!*

Después de investigar un poco y hacer algunas pruebas me he quedado con **LightSAML SP Bundle**. Tienen [su propia guía para empezar](https://www.lightsaml.com/SP-Bundle/Getting-started/) pero en este ejemplo simplificaremos algunos aspectos. 


##### Empezamos instalando el bundle

{% highlight bash %}
composer require lightsaml/sp-bundle
{% endhighlight %} 

{% highlight php %}
<\?php
// app/AppKernel.php

public function registerBundles()
{
    $bundles = array(
        // ...
        new LightSaml\SymfonyBridgeBundle\LightSamlSymfonyBridgeBundle(),
        new LightSaml\SpBundle\LightSamlSpBundle(),
    );
}

{% endhighlight %} 


##### Configuramos el bundle 

Tenemos que configurar el bundle con los parámetros necesarios. Necesitamos definir que "idp" (proveedor de identidad) usaremos. Para ello simplemente indicaremos el *path* del archivo *xml* que guardamos antes: `app/config/saml/google-apps.com.xml`

También debemos indicar dónde se encuentran los certificados que permitirán que la autenticación se realice de forma segura.

> No voy a entrar en detalle de como generar un certificado en este post. Con [googlear](https://www.google.es/search?q=how%20to%20generate%20crt%20key) un poco no tardarás en llegar a explicaciones sencillas como [esta](http://serverfault.com/a/224127)

{% highlight none %}
// app/config/security.yml
light_saml_symfony_bridge:
    own:
        entity_id: 'https://developerdepueblo.com/backoffice'
        credentials:
            -
                certificate: "%kernel.root_dir%/config/saml/mygoogleappsidp.crt"
                key:         "%kernel.root_dir%/config/saml/mygoogleappsidp.key"
                password:    ~
    party:
        idp:
            files:
                - "%kernel.root_dir%/config/saml/google-apps.com.xml"
{% endhighlight %}

Además, tendremos que configurar las rutas que gestionarán las peticiones necesarias para poder autenticar al usuario de manera segura. El bundle se encarga de gestionar esto por nosotros.
Tan solo debemos incluir estás rutas en nuestro proyecto.

{% highlight none %}
// app/config/routing.yml
lightsaml_sp:
    resource: "@LightSamlSpBundle/Resources/config/routing.yml"
    prefix: "/backoffice/saml"
{% endhighlight %} 

En este caso añadimos el prefijo `/backoffice/saml` solo por mantener cierta semántica, pero podríamos poner cualquier prefijo.

##### Proveedor de usuarios

Como hemos visto antes, SAML es un estándar para autenticar usuarios que privienen de un tercero, en este caso Google Apps. Sin embargo, a pesar de que los usuarios no están persistidos en nuestro sistema, **Symfony necesita una entidad *User*** con la que operar, por lo que necesitaremos instanciar un usuario en nuestra app para que todo funcione.

Como vimos antes, en Google Apps es posible limitar el acceso a una app SAML sólo a ciertos usuarios, por lo que la opción más sencilla es implementar un *mock* como proveedor de usuarios exclusivo para este fin. 

Sólo tenemos que crear una implementación sencilla de `UserProviderInterface`. **Es importante definir un rol custom** para poder identificar a estos usuarios más adelante, en este caso usamos **ROLE_ADMIN**.

{% highlight php %}
<?php

namespace AppBundle\Security;

use Symfony\Component\Security\Core\Exception\UnsupportedUserException;
use Symfony\Component\Security\Core\Exception\UsernameNotFoundException;
use Symfony\Component\Security\Core\User\User;
use Symfony\Component\Security\Core\User\UserInterface;
use Symfony\Component\Security\Core\User\UserProviderInterface;

class AdminUserMockProvider implements UserProviderInterface
{
    public function loadUserByUsername($username)
    {
        $user = new User($username, '', ['ROLE_ADMIN']);

        return $user;
    }

    public function refreshUser(UserInterface $user)
    {
        return $this->loadUserByUsername($user->getUsername());
    }

    public function supportsClass($class)
    {
        return (bool) $class;
    }
}

{% endhighlight %} 

Y definimos un servicio con ella:

{% highlight none %}
// app/config/services.yml
services:
  app.security.admin_user_mock_provider:
    class: AppBundle\Security\AdminUserMockProvider
{% endhighlight %} 

##### Configurando security.yml

Es el momento de configurar la seguridad de nuestra app, vamos a definir:  
- El proveedor de usuarios utilizando el servicio que creamos antes  
- Un firewall que solo aplique a las rutas de nuestro backoffice  
- Y por último, el nivel de acceso necesario para acceder a cada ruta


{% highlight none %}
// app/config/security.yml

security:
  providers:
    admin_user_mock:
      id: app.security.admin_user_mock_provider

  firewalls:  
    backoffice:
      pattern: ^/backoffice
      anonymous: true
      http_basic: false
      light_saml_sp:
        provider: admin_user_mock
        login_path: /backoffice/saml/login
        check_path: /backoffice/saml/login_check
        require_previous_session: false

  access_control:
    # Allow ananymous access the /saml paths
    - { path: ^/backoffice/saml, role: IS_AUTHENTICATED_ANONYMOUSLY }
    # Require user to be admin to access our backoffice
    - { path: ^/backoffice, role: ROLE_ADMIN }
{% endhighlight %} 

**Y listo!**

![](/images/gif/applause.gif)

Accediendo desde el browser a nuestra app, en la ruta `/backoffice` deberíamos encontrarnos que nos aparece el selector de cuentas de Google para indicar con que cuenta queremos iniciar sesión.

Recuerda que **solo podrán hacer login con las cuentas del dominio** dónde configuramos la app, y solo la podrán usar aquellos usuarios para los que la activaste.

### Reflexión final

Esta es la primera vez que utilizo SAML y aún no estoy familiarizado con el estándar. Creo que este sistema de login no tiene ningún aspecto negativo, pero si le ves algún "pero", ya sea al estándar o esta implementación en particular, no dudes en enviarme un comentario.

Por cierto, he subido todos estos cambios [en un solo commit ](https://github.com/joserobleda/symfony-saml-example/commit/4b9be4809e78a924ed3aa8ed2e37263d07dd6360), por si así se ve más claro.

