---
layout: post
title:  "Conectar Scrapy con Django"
lang: es
ref: django-scrapy
description: "Mi solución (no necesesariamente la mejor) para integrar Django con Scrapy ."
date:   2018-01-03 21:21:01 +0100
categories: ["Python", "Data"]
---
## Primers Pasos

Vamos a utilizar `Django` para acceder, almacenar y probablemente presentar también de manera sencilla los datos que queremos extraer con `Scrapy`.
También necesitaremos la extensión `scrapy-djangoitem` que conectará el modelo de datos de los dos frameworks.
El proyecto de Scrapy debería estar dentro del proyecto de Django, al mismo nivel que una app.

Si estás leyendo este post, supongo que tienes un conocimiento básico de cómo funcionan Djngo y Srapy.
Si no, te recomiendo que eches un vistazo a los siguientes enlaces:

* [Instalar Django](https://docs.djangoproject.com/en/2.0/intro/install/): `pip install django`
* [Nuevo proyecto de Django](https://docs.djangoproject.com/en/2.0/intro/tutorial01/): `django-admin startproject mysite`
* [Nuava app de Django](https://docs.djangoproject.com/en/2.0/intro/tutorial01/): `python manage.py startapp polls`
* [Instalar Scrapy](https://doc.scrapy.org/en/latest/intro/install.html): `pip install Scrapy`
* [Instalar scrapy-djangoitem](https://github.com/scrapy-plugins/scrapy-djangoitem): `pip install scrapy-djangoitem`
* [Nuevo proyecto y spider en Scrapy](https://doc.scrapy.org/en/latest/intro/tutorial.html): `pip install django`

{% highlight console %}
# Ejemplo de cuál debería ser el árbol de directorios
...
├── requirements.txt
├── manage.py
├── django-project
│   ├── django-project
│   │   ├── settings.py
│   │   ├── urls.py
│   │   ├── views.py
│   │   └── wsgi.py
│   ├── example-app
│   │   ├── admin.py
│   │   ├── apps.py
│   │   ├── migrations
│   │   ├── models.py
│   │   ├── static
│   │   │   ├── ...
│   │   ├── templates
│   │   │   ├── ...
│   │   ├── tests.py
│   │   ├── urls.py
│   │   ├── views.py
│   ├── scrapy-proyect
│   │   ├── ..  
│   │   ├── apps.py
│   │   ├── crawlproperty.py
│   │   ├── geckodriver.log
│   │   ├── items.py
│   │   ├── middlewares.py
│   │   ├── pipelines.py
│   │   ├── settings.py
│   │   ├── spiders
│   │   │   ├── spider.py
...
{% endhighlight %}

## Configuración de Django 

Asegúrate de añadir la app de Django y el proyecto de Scrapy al archivo de configuración del proyecto de Django `django-project/django-project/settings.py`:

{% highlight python %}
INSTALLED_APPS = [
    'example-app.apps.ExampleAppConfig',
    'scrapy-proyect.apps.ScrapyProyectConfig',
    ...
]
{% endhighlight %}

## Configuración de Scrapy

Necesitamos añadir un archivo __init__.py vacio en `django-project/scrapy-project/__init__.py` y en `django-project/scrapy-project/spiders/__init__.py` para que Django reconozca el proyecto de Scrapy como un paquete.

Para que Django y Scrapy sean capaces de comunicarse, también es necesario modificar el archivo de configuración de `django-project/scrapy-project/settings.py`:

{% highlight python %}
import sys
import os
import django
# ------------ DJANGO SETTINGS ------------
sys.path.append('../dedomeno')
os.environ['DJANGO_SETTINGS_MODULE'] = 'django-project.settings'
django.setup()  # for > django 1.7

BOT_NAME = 'scrapy-project'

SPIDER_MODULES = ['scrapy-project.spiders']
NEWSPIDER_MODULE = 'scrapy-project.spiders'
...
{% endhighlight %}

Es necesario crear `django-project/scrapy-project/apps.py` para que el proyecto de Scrapy sea identificado por Django como una app:

{% highlight python %}
from django.apps import AppConfig

class ScrapyPrpjectConfig(AppConfig):
    name = 'scrapy-project'
{% endhighlight %}

En `django-project/scrapy-project/items.py` se crean los Items de Scrapy, que heredan de DjangoItem y los asignamos al modelo de Django en la variable `django_model`:

{% highlight python %}
import scrapy
from scrapy_djangoitem import DjangoItem
from example-app.models import Xxxx, Yyyy

class XxxxItem(DjangoItem):
    django_model = Xxxx

class YyyyItem(DjangoItem):
    django_model = Yyyy
{% endhighlight %}

Tras esto, los DjangoItems están listos para ser usados en el spider. También podemos acceder al modelo de Django a través del ORM de Django (borrar, crear, consultas...). `django-project/scrapy-project/spiders/spider.py`

{% highlight python %}
# Scrapy Items imports
from scrapy-project.items import XxxxItem, YyyyItem
# Django model imports
from example-app.models import Xxxx, Yyyy
{% endhighlight %}

Generalmente se usará un [Item Pipeline](https://doc.scrapy.org/en/latest/topics/item-pipeline.html) para limpiar, eliminar dublicados y almacener un item en la base de datos (aunque también es posible realizarlo dentro del spider, dependiendo de las necesidades) `django-project/scrapy-project/spiders/pipelines.py`:

{% highlight python %}
from scrapy-project.items import XxxxItem, YyyyItem
from example-app.models import  Xxxx, Yyyy

class XxxxPipeline(object):
    def process_item(self, item, spider):
        # the cleaning magic starts!
        ....
        # finish!
        item.save()
        return item
...
{% endhighlight %}

También, es posible interactuar con los objetos DjangoItem de Scrapy o con los objetos Django con los imports que se muestran arriba..

Por último, es necesrio habilitar los pipelines en el fichero de configuración de Scrapy `django-project/scrapy-project/settings.py` 

{% highlight python %}
# Configure item pipelines
#ITEM_PIPELINES = {
    'scrapy_project.pipelines.XxxxPipeline': 200,
    ...
#}
{% endhighlight %}

**HAPPY SCRAPING!**

Intentaré realizar más entradas en el blog para explorar los siguientes puntos:

>Normalmente se usa la linea de comandos para empezar un crawler. Pero si quieres realizar un scrapeo recurrente, probablemente será necesario hacer lo siguiente:
* **De forma Programática**: utilizando un sistema asíncrono de mensajería como [Celery](http://www.celeryproject.org/) y un broker como [RabbitMQ](https://www.rabbitmq.com/)
* **Desde las vistas**: lanzando los spiders desde las vistas de Django con 'python-scrapyd-api', tal y como explican en [How to use Scrapy with Django Application](https://medium.com/@ali_oguzhan/how-to-use-scrapy-with-django-application-c16fabd0e62e)
<<<<<<< HEAD
 
=======
 
>>>>>>> origin/master
