---
layout: post
title:  "Zoidberg: Python scraper to search for surgeon reviews"
description: "Are you ready to operate, Doctor? - I'd love to, but first I have to perform surgery."
date:   2018-03-10 11:04:22 +0100
categories: ["Python", "Data"]
---

## Motivation
I (34 yo) have a femoroacetabular impingement. It may affect the hip joint in young and middle-aged adults and occurs when the ball shaped femoral head rubs abnormally or does not permit a normal range of motion in the acetabular socket. [Wikipedia](https://en.wikipedia.org/wiki/Femoroacetabular_impingement).

<div class="full">
    <a href="/assets/posts/{{page.slug}}/impingement.jpeg">
    <img class="img-fluid" src="/assets/posts/{{page.slug}}/impingement.jpeg">
    </a>
</div>

The only long term solution is surgery. And like everyone else, I'd like the best doctor to "open" my hip.
The problem is, it's hard to find doctor reviews from real people even if you try to search in Google, and when you find a forum where people talk about the operations, it's even harder to find specific information.

Because of that, I decided to create a small scraper that will search for every comment where a doctor appears and it retrieves a json or csv file to read about it.

## Overview
I have open sourced it (in a very alpha version), making it possible to add more resources of others illnesses or syndromes. Right now it only search for a given Spanish doctor for femoroacetabular impingement.
- [https://github.com/ginopalazzo/zoidberg](https://github.com/ginopalazzo/zoidberg)

[![pypi](https://img.shields.io/pypi/v/zoidberg.svg)](https://pypi.python.org/pypi/dr-zoidberg)
[![Travis Build](https://img.shields.io/travis/ginopalazzo/zoidberg.svg)](https://travis-ci.org/ginopalazzo/zoidberg)
[![Documentation Status](https://readthedocs.org/projects/zoidberg/badge/?version=latest)](https://zoidberg.readthedocs.io/en/latest/?badge=latest)

Are you ready to operate, Doctor? - I'd love to, but first I have to perform surgery.


![Dr John Zoidberg](https://upload.wikimedia.org/wikipedia/en/4/4a/Dr_John_Zoidberg.png)

- Free software: MIT license
- Documentation: [https://zoidberg.readthedocs.io.](//zoidberg.readthedocs.io).

The goal of Zoidberg is to help people find useful information of a surgeon. So the information you should provide to Zoidberg is:
- Country (i.e. ES for Spain)
- Doctor (i.e. Margalet)
- Area (i.e. traumatologia)
- Illness (i.e. femoroacetabular)
- output (csv or json file)

## Technology
I used Python 3, although (but not tested) it will probably work in python 2 also, and [Scrapy](https://scrapy.org/), an open source scraper framework.
I also use [cookiecutters](https://github.com/audreyr/cookiecutter) CL utility  to create a Python package. There are plenty of templates for every type of project, like [standard Python](https://github.com/audreyr/cookiecutter-pypackage) (the one I used), [Django](https://github.com/pydanny/cookiecutter-django) ...
Also a good resource is the Jeff Knupp guide, [open sourcing a python project the right way](https://jeffknupp.com/blog/2013/08/16/open-sourcing-a-python-project-the-right-way/).

## How to install it

{% highlight console %}
pip install dr-zoidberg
{% endhighlight%}

or clone from:
{% highlight console %}
git clone git@github.com:ginopalazzo/zoidberg.git
{% endhighlight%}

## How to use it

From a project:

{% highlight python %}
from zoidberg import zoidberg

z = zoidberg.Zoidberg(country='es', doctor='margalet', area="traumatologia", illness="femoroacetabular", path='test.csv', output='csv')
z.conf()
z.run()
{% endhighlight %}

Or from CLI (although it will change in futures versions):

{% highlight python %}
python zoidberg.py -c es -d margalet -a traumatologia -i femoroacetabular -p test.csv -o csv
{% endhighlight %}

## How it works
Zoidberg project is organized in the following tree:

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

- `.zoidberg.py` is the file that holds Zoidberg class that runs a `CrawlerRunner` with the user parameters (country, doctor, area, illness, output and path).
- `.scraper/` a Scrapy project named scraper.
- `.scraper/db` holds the country jsons with the db of the Internet forums (domains) and the urls for each illness.
- `.scraper/pipelines.py` cleans the information scraped by the spider and writes it in the output file selected.
- `.scraper/settings.py` default settings of the scrapy spider and AUTOTHROTTLE_ENABLED = True for [responsible scraping](https://doc.scrapy.org/en/latest/topics/autothrottle.html).
- `.scraper/spiders/` hold the spiders organized by country. All spiders inherit from `ZoidbergSpider`.


## Next steps
Lots of work still to do...

- CLI: Change [argparse](https://docs.python.org/3/library/argparse.html) to [click.pocoo.org](http://click.pocoo.org/5/).
- Get a list of countries available.
- Get  a list of areas available for a country.
- Get a list of illnesses available for an area.
- Add keywords of illnesses for search.
- Search a doctor for every area or illness.
- Comprehensive tests

## Conclusions
Despite it working, Zoidberg is still a child lobster scraper that need a lot of improvement, although it works perfectly for the first purpose it was conceived for; search a good doctor for my hip arthroscopy.

Margalet is supposed to be the best femoracetabular specialist in Spain, but after using Zoidberg, I realized that lots of people are complaining about his new technique called "out inside". No complaints about Pérez Carro, my new hip doctor.

If you understand Spanish, you can see the differences Zoidberg found between the doctors:
- [margalet.csv]({{"/assets/posts/zoidberg-surgery-doctor-scraper/margalet.csv" | absolute_url}})
- [perez-carro.csv]({{"/assets/posts/zoidberg-surgery-doctor-scraper/perez-carro.csv" | absolute_url}})
