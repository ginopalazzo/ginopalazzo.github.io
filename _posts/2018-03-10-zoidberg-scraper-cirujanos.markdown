---
layout: post
title:  "Zoidberg: Un scraper en Python para buscar opiniones de cirujanos"
lang: es
ref: zoidberg-surgery-doctor-scraper
description: "¿Listo para intervenir, doctor? - Sí, pero primero tengo que operar."
date:   2018-03-10 11:04:22 +0100
tags: ["Python", "Data"]
categories: ["Scraper"]
---

## Motivatión
Tengo, con 34 años, el llamado choque o pinzamiento femoroacetabular. Suele afectar la cadera del adulto joven y ocurre cuando la cabeza esférica del femur roza o no se mueve de forma normal en la cavidad acetabular. [Wikipedia](https://en.wikipedia.org/wiki/Femoroacetabular_impingement).

<div class="full">
    <a href="/assets/posts/{{page.ref}}/impingement.jpeg">
    <img class="img-fluid" src="/assets/posts/{{page.ref}}/impingement.jpeg">
    </a>
</div>

La única solución a largo plazo es quirúrgica. Y como todo el mundo, me gustaría que el mejor cirujano me "abriese" la cadera.
El problema: Es complicado encontrar opiones reales de cirujanos, incluso tras una búsqueda concienzuda en Google. Además, cuando encuentras un foro en el que la gente habla sobre la operación en la que estás interesado, es difícil encontrar información especifica, en este caso, opiniones sobre un cirujano.

Por eso, decidí crear un pequeño scraper, que buscará cualquier comentario en el que se mencione a un doctor y devolverá un archivo csv o json para poder leer las opiniones sobre este cirujano.

## Introducción
He liberado el código (en una versión muuuy alpha), siendo posible añadir más fuentes a las búsquedas u otras enfermedades o dolencias . Actualmente solo busca cirujanos españoles para el choque femoroacetabular.
- [https://github.com/ginopalazzo/zoidberg](https://github.com/ginopalazzo/zoidberg)

[![pypi](https://img.shields.io/pypi/v/zoidberg.svg)](https://pypi.python.org/pypi/dr-zoidberg)
[![Travis Build](https://img.shields.io/travis/ginopalazzo/zoidberg.svg)](https://travis-ci.org/ginopalazzo/zoidberg)
[![Documentation Status](https://readthedocs.org/projects/zoidberg/badge/?version=latest)](https://zoidberg.readthedocs.io/en/latest/?badge=latest)

"¿Listo para intervenir, doctor?" - "Sí, pero primero tengo que operar."


![Dr John Zoidberg](https://upload.wikimedia.org/wikipedia/en/4/4a/Dr_John_Zoidberg.png)

- Free software: MIT license
- Documentation: [https://zoidberg.readthedocs.io.](//zoidberg.readthedocs.io).

El objetivo de Zoidberg es ayudar a encontrar información sobre un cirujano.
La información que necesita Zoidberg para buscar es:
- Country (i.e. ES para España)
- Doctor (i.e. Margalet)
- Area (i.e. traumatologia)
- Illness (i.e. femoroacetabular)
- output (csv or json file)

## Tecnología
He usado Python 3, aunque (pero no lo he probado) probablemente funcione  con Python 2 y [Scrapy](https://scrapy.org/), un scraper abierto.
Para crear el paquete de Python he utilizado [cookiecutters](https://github.com/audreyr/cookiecutter). Hay muchas plantillas que puedes utilizar para cualquier tipo de proyecto como [standard Python](https://github.com/audreyr/cookiecutter-pypackage) (el que he utilizado), [Django](https://github.com/pydanny/cookiecutter-django) ...
También un recurso interesante es la guía de Jeff Knupp llamada [open sourcing a python project the right way](https://jeffknupp.com/blog/2013/08/16/open-sourcing-a-python-project-the-right-way/).

## Cómo instalarlo

{% highlight console %}
pip install dr-zoidberg
{% endhighlight%}

o clónalo desde:
{% highlight console %}
git clone git@github.com:ginopalazzo/zoidberg.git
{% endhighlight%}

## Cómo utilizarlo

En un proyecto:

{% highlight python %}
from zoidberg.zoidberg_main import Zoidberg

z = Zoidberg(country='es', doctor='margalet', area="traumatologia", illness="femoroacetabular", path='test.csv', output='csv')
z.conf()
z.run()
{% endhighlight %}

O en CLI (Terminal) (Zoidberg te preguntará algunas preguntas para entender qué quieres buscar):

{% highlight console %}
zoidberg
{% endhighlight %}

## Cómo funciona
El proyecto de Zoidberg está organizado de la siguiente manera:

{% highlight console %}
.
├── __init__.py
├── scraper
│   ├── __init__.py
│   ├── db
│   │   └── es
│   │       └── es_db.json
│   ├── items.py
│   ├── middlewares.py
│   ├── pipelines.py
│   ├── settings.py
│   └── spiders
│       ├── __init__.py
│       ├── es_spider.py
│       └── zoidberg_spider.py
├── zoidberg.py

{% endhighlight %}

- `.zoidberg.py` es el archivo que contiene la clase Zoidberg y ejecuta un `CrawlerRunner` junto con los parametros country, doctor, area, illness, output y path.
- `.scraper/` es un poryecto de Scrapy llamado scraper.
- `.scraper/db` contiene el json de paises con la base de datos de los foros donde buscar (dominios) y sus correspondientes urls para cada enfermedad o dolencia.
- `.scraper/pipelines.py` limpia la información scrapeada por el crawler y la escribe en el fichero de salida seleccionado.
- `.scraper/settings.py` la configuración por defecto del spider de Scrapy y AUTOTHROTTLE_ENABLED = True para un [scrapero responsable](https://doc.scrapy.org/en/latest/topics/autothrottle.html).
- `.scraper/spiders/` contiene los spiders organizados por país. Todos los spiders heredan de `ZoidbergSpider`.


## Próximos pasos
Todavía que mucho trabajo por hacer...

- CLI: Cambiar de [argparse](https://docs.python.org/3/library/argparse.html) a [click.pocoo.org](http://click.pocoo.org/5/).
- Devolver una lista de los paises disponibles.
- Devolver una lista de las áreas disponibles para un país.
- Devolver un lista de las dolencias disponibles para un área.
- Añadir palabras clave o dolencia para buscar.
- Buscar a un doctor para cada área o dolencia.
- Tests concienzudos.

## Conclusiones
A pesar de funcionar, Zoidberg es todavía una pequeña langosta que necesita mucha mejora, aunque funciona perfectamente para el primer propósito para para la que fue concebida; encontrar un buen doctor para mi artroscopia de cadera.

Margales se supone que es el mejor especialista español en el choque femoroacetabular, pero después de utilizar Zoidberg, me di cuenta de que hay mucha gente quejándose de la nueva técnica que utiliza llamada "out-inside". Ni una queja del doctor Pérez Carro, mi nuevo cirujano de cadera.

Puedes ver las diferencias que encontró Zoidberg entre los dos doctores:
- [margalet.csv]({{"/assets/posts/zoidberg-surgery-doctor-scraper/margalet.csv" | absolute_url}})
- [perez-carro.csv]({{"/assets/posts/zoidberg-surgery-doctor-scraper/perez-carro.csv" | absolute_url}})
