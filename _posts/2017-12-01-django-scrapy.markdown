---
layout: post
title:  "Django + Scrapy"
description: "How I have integrated Django with Scrapy (not necessarily the best solution)."
date:   2018-01-03 21:21:01 +0100
categories: ["Python", "Data"]
---
## First steps

We will use `Django` to easily access and store (maybe present too) the data that we want to extract from the web with `Scrapy`. We also need the extension `scrapy-djangoitem` that will connect the data model of the two frameworks. The scrapy project should be inside the django project at the same level of an app.

If you are reading this post I suppose that you have a basic knowledge of how Django and Scrapy works. You can read further in the following links:

* [Install django](https://docs.djangoproject.com/en/2.0/intro/install/): `pip install django`
* [New django project](https://docs.djangoproject.com/en/2.0/intro/tutorial01/): `django-admin startproject mysite`
* [New django app](https://docs.djangoproject.com/en/2.0/intro/tutorial01/): `python manage.py startapp polls`
* [Install scrapy](https://doc.scrapy.org/en/latest/intro/install.html): `pip install Scrapy`
* [Install scrapy-djangoitem](https://github.com/scrapy-plugins/scrapy-djangoitem): `pip install scrapy-djangoitem`
* [New scrapy project & spider](https://doc.scrapy.org/en/latest/intro/tutorial.html): `pip install django`

{% highlight console %}
# How the tree directory should look like
...
├── requirements.txt
├── manage.py
├── django-project
│   ├── django-project
│   │   ├── settings.py
│   │   ├── urls.py
│   │   ├── views.py
│   │   └── wsgi.py
│   ├── example-app
│   │   ├── admin.py
│   │   ├── apps.py
│   │   ├── migrations
│   │   ├── models.py
│   │   ├── static
│   │   │   ├── ...
│   │   ├── templates
│   │   │   ├── ...
│   │   ├── tests.py
│   │   ├── urls.py
│   │   ├── views.py
│   ├── scrapy-proyect
│   │   ├── ..  
│   │   ├── apps.py
│   │   ├── crawlproperty.py
│   │   ├── geckodriver.log
│   │   ├── items.py
│   │   ├── middlewares.py
│   │   ├── pipelines.py
│   │   ├── settings.py
│   │   ├── spiders
│   │   │   ├── spider.py
...
{% endhighlight %}

## Django configuration

Make sure you add to the django app and the scrapy project to the settings of your django project `django-project/django-project/settings.py`:

{% highlight python %}
INSTALLED_APPS = [
    'example-app.apps.ExampleAppConfig',
    'scrapy-proyect.apps.ScrapyProyectConfig',
    ...
]
{% endhighlight %}

## Scrapy configuration

We need to add an empty __init__.py file in `django-project/scrapy-project/__init__.py` and `django-project/scrapy-project/spiders/__init__.py` in order for django to recognize the scrapy project as a package.

We will also modify our `django-project/scrapy-project/settings.py` as the following, so django and scrapy can communicate:

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

`django-project/scrapy-project/apps.py` needs to be created in order to simulate scrapy project as a django app:

{% highlight python %}
from django.apps import AppConfig

class ScrapyPrpjectConfig(AppConfig):
    name = 'scrapy-project'
{% endhighlight %}

In `django-project/scrapy-project/items.py` we create the scrapy items which are inherited from DjangoItem and we assign them to the django model in the `django_model` variable:

{% highlight python %}
import scrapy
from scrapy_djangoitem import DjangoItem
from example-app.models import Xxxx, Yyyy

class XxxxItem(DjangoItem):
    django_model = Xxxx

class YyyyItem(DjangoItem):
    django_model = Yyyy
{% endhighlight %}

Now the DjangoItems are ready to use in the spider. Also we could access the Django model through the Django ORM (delete, create, queries...). `django-project/scrapy-project/spiders/spider.py`

{% highlight python %}
# Scrapy Items imports
from scrapy-project.items import XxxxItem, YyyyItem
# Django model imports
from example-app.models import Xxxx, Yyyy
{% endhighlight %}

Usually you are going to use an [Item Pipeline](https://doc.scrapy.org/en/latest/topics/item-pipeline.html) for cleaning, checking duplicates and storing the item in the database (though you could also do it in the spider depending on your needs) `django-project/scrapy-project/spiders/pipelines.py`:

{% highlight python %}
from scrapy-project.items import XxxxItem, YyyyItem
from example-app.models import  Xxxx, Yyyy

class XxxxPipeline(object):
    def process_item(self, item, spider):
        # the cleaning magic starts!
        ....
        # finish!
        item.save()
        return item
...
{% endhighlight %}

Also, you can interact with your Scrapy DjangoItem object and also with your Django object only with the appropriate imports as shown.

Last thing to do, is enable our pipelines in your scrapy settings file `django-project/scrapy-project/settings.py` 

{% highlight python %}
# Configure item pipelines
#ITEM_PIPELINES = {
    'scrapy_project.pipelines.XxxxPipeline': 200,
    ...
#}
{% endhighlight %}

**HAPPY SCRAPING!**

I'll try to do more posts to explore the following further:

>Usually you would use the scrapy command line to start the crawler. But if you want to do recurrent scraping in the same doming probably you would have to:
* **Programmatically**: use asynchronous messaging system like [Celery](http://www.celeryproject.org/) and a broker like [RabbitMQ](https://www.rabbitmq.com/)
* **From views**: triggering spiders from Django views with 'python-scrapyd-api', as explain in [How to use Scrapy with Django Application](https://medium.com/@ali_oguzhan/how-to-use-scrapy-with-django-application-c16fabd0e62e)
 



