---
layout: post
title:  "101 API de aplicaciones móviles"
lang: es
ref: 101-Mobile-API
description: "Cómo descubrir, capturar y emular llamadas API de aplicaciones móviles"
date:   2022-05-22 11:07:21 +0100
categories: ["Scraper"]
tags: ["Python", "Data"]
---

## Introducción
El objetivo de este post es describir los pasos que he necesitado para descubrir y utilizar la API de una aplicación móvil (cuyo nombre no diré).

He dividido el problema en los siguientes puntos:

1º Descubrimiento del API: Descubir qué llamadas se realizan desde una aplicación móvil.
    1.1 Problema: ¿Cómo capturo las llamadas al API que hace una aplicación móvil?
        Solución: Programa de captura de tráfico
    1.2 Problema: Vaya, veo las llamadas pero están encriptadas...
        Solución: Man in the middle con certificados. Sólo en iOS.
2º Autenticación en el API: De nada sirve saber cuáles son las llamadas que se realizan si no podemos recrearlas. Para ello necesitamos descubrir qué mecanismos tiene la aplicación para autenticarse.
    2.1 Problema: No sé cómo se optienen todos esos datos de autenticación
        Solución: Decompilar la aplicación móvil.
    2.2 Problema: No puedo leer 
3º Utilizar el API
    Una vez sabemos qué llamadas pueden hacerse al API y cómo nos autenticamos para realizarlas, solo nos 


Como en este pequeño blog, generalmente querrás organizar las entradas con contenido parecido en categorías o etiquetas.

En este caso, lo que quiero hacer es asignar un color a cada etiqueta en Jekyll.

Para ello, he creado en el directorio raíz un directorio llamado `_categories`, donde se almacenará cada categoría como un archivo HTML (markdown).

{% highlight console %}
.
├── _categories
│   ├── d3js.html
│   ├── data.html
│   ├── jekyll.html
│   ├── python.html
│   └── visualization.html
{% endhighlight %}

El código de una categoría `_categories/jekyll.html` podría ser el siguiente:

{% highlight ruby %}
---
title: Jekyll
date: 2016-12-17 11:09:00 -05:00
color: "#ffc107"
---
{% endhighlight %}

En este post `_post/2017-11-20-jekyll-color-code-categories` añadiremos una lista de categorías:

{% highlight ruby%}
---
layout: post
title:  "Categorías de color en Jekyll"
description: "Cómo implementar categorías de color reutilizables Jekyll."
date:   2017-12-02 11:07:21 +0100
categories: ["Jekyll", "Visualization"]
---
## Introducción2
Como en este pequeño blog, generalmente querrás organizar las entradas ...
{% endhighlight %}

También es necesario añadir una colección de 'categories' en el archivo de configuración `./_config.yml` :

{% highlight ruby %}
collections:
  - categories
{% endhighlight %}

Para mostrar dicha categoría en una entrada o en la página principal utilicé el siguiente snippet (aunque seguramente podría haber pensado una solución mejor):

{% highlight liquid %}
    {{ "{% for category in page.categories " }}%}
        {{ "{% for category_template in site.categories " }}%}
            {{ "{% if category == category_template.title " }}%}
            <strong style="color:{{ "{{ category_template.color " }}}};">
                {{ "{{ category " }}}}
            </strong>
            {{ "{% endif " }}%}
        {{ "{% endfor " }}%}  
    {{ "{% endfor " }}%}
{% endhighlight %}

>Ha sido complicado evitar las llamadas "liquid template tags" como `{{ "{% xxx " }}%)` mientras escribía esta entrada; Encontré una solución relativamente limpia para este problema sin usar plug-ins en:
[https://stackoverflow.com/questions/3426182/how-to-escape-liquid-template-tags](https://stackoverflow.com/questions/3426182/how-to-escape-liquid-template-tags)

El resultado de esta solución es la que puedes ver en este blog.

