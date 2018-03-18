---
layout: post
title:  "Dedomeno: A Spanish real estate (Idealista) python scraper"
description: "Dedomeno is a python scraper with Django a Scrapy for idealista.com and, hopefully more "
date:   2018-02-10 13:21:45 +0100
categories: ["Python", "Data"]
---
## Motivation
As I mentioned before in the [rental room prices in Spanish municipalities]({{ site.baseurl }}{% post_url 2018-02-12-spanish-median-rental-room-prices %}) post, the Spanish real estate market can be intense.

For citizens:
Hard to make inform decision of house buying, real estate investment or even house rental.

For big companies:
Trust funds, real estate companies, banks... is so easy. The have or could buy the data and insights to make the right decision of investment.

Not a fair competition.

## Overview
[Idealista.com](https://www.idealista.com/) is de facto standard for searching a house or other real estate product. Is the biggest online house marketplace in Spain, even the real estate agencies advertise their products in this webpage.

They have plenty of data which of course, don't share it for free. They only have few and [not very detailed reports](https://www.idealista.com/informes-precio-vivienda). And an very limited [API](http://developers.idealista.com/access-request) that you first have to request for access, and not always get response.

Because of that, I decided to open source the python scraper that I have use to understand the Spanish real estate market and make inform house investments; I called it Dedomeno and you can find it in:

[https://github.com/ginopalazzo/dedomeno](https://github.com/ginopalazzo/dedomeno)

<div class="full">
    <a href="/assets/posts/{{page.slug}}/dedomeno-1.jpg">
    <img class="img-fluid" src="/assets/posts/{{page.slug}}/dedomeno-1.jpg">
    </a>
</div>

## Technology

The technology behind Dedomeno is almost all the most used Python frameworks:
- [Django](https://www.djangoproject.com/) - Django is a high-level Python Web framework that encourages rapid development and clean, pragmatic design
- [Scrapy](https://scrapy.org/) - An open source and collaborative framework for extracting the data you need from websites.
- [Celery](http://www.celeryproject.org/) - An asynchronous task queue/job queue based on distributed message passing

## Features
Dedomeno first build was intend to be a homemade crawler for idealista.com, but over time, in order to automatize all the process, it become more an more complex (not an exhaustive list):

- Crawler (Scrapy)
    + Proxy Rotator Middleware to not get banned .
    + Proxy banned alert Middleware (remove proxy and send an email).
    + Statistical random User Agent Middleware (with file backup).
    + Django postgresql database to store all types of real estate properties.
    + Celery task integrated with Django db.
    + Flower to monitorice the celery workers.

- Idealista.com features 
    + Scrap, clean and store 7 types of real estate properties (houses, offices, rooms...).
    + Scrap all real estate companies information and related it to the properties.
    + Check for off-line properties. Date of online and offline.
    + Check for price changes.
    + Geocode each property:
        * long, lat.
        * street, city/town, province.
        * standard geocode for province, city, district, neighborhood.
    + Make a Spanish geocode tree to make quick operation with sets.

- Web page (Django server)
    + Real Estate companies information.
    + Download the data.
    + View property details.
    + Spanish map view with average prices.

<div class="full">
    <a href="/assets/posts/{{page.slug}}/dedomeno-2.jpg">
    <img class="img-fluid" src="/assets/posts/{{page.slug}}/dedomeno-2.jpg">
    </a>
</div>

<div class="full">
    <a href="/assets/posts/{{page.slug}}/dedomeno-3.jpg">
    <img class="img-fluid" src="/assets/posts/{{page.slug}}/dedomeno-3.jpg">
    </a>
</div>

## How to use it
There is 2 ways of using Dedomeno:

#### Manual

Use crawlpropery.py to make manual crawls, just modify the CrawlPropertyReactor parameters to your needs:

{% highlight python %}
# dedomeno/idealista/crawlproperty.py

if __name__ == "__main__":
    spider = CrawlPropertyReactor(property_type='land',
             transaction='sale', provinces=['salamanca'])
    spider.conf()
    spider.run()
{% endhighlight %}

#### Programmatic

Use django + celery + flower + scrapy to do programmatic crawls and visualize the output:

1. start django server, http://127.0.0.1:8000/
- `python manage.py runserver`
2. remove all tasks from queue: celery (Only works when RabbitMQ is up)
- `celery -A proj purge`
4. Enter admin http://127.0.0.1:8000/admin and change `django-celery-beat` to schedule the periodic task in the db
4. Run the RabbitMQ message broker
- `sudo rabbitmq-server`
5. Celery:
    1. Start the celery worker
- `celery -A dedomeno worker --loglevel=INFO`
    2. Run Flower, a web based tool for monitoring and administrating Celery clusters
- `celery -A dedomeno flower`
    3. Start the celery beat (schedule tasks)
- `celery -A dedomeno beat -l info -S django`


## Next steps
Plenty of next steps, I could use some help ;) :
- Decouple source. Now Dedomeno only takes idealista.com information and is highly dependent of idealista.
- Add more sources (i.e. [fotocasa](https://www.fotocasa.es), the second player).
- Alerts when idealista make major and minor changes.
- Comprehensive tests
- Better map visualization
- Mix idealista, fotocasa information with official data
- ...

## Conclusion
Dedomeno works fine to crawl idealista.com in both, manual and programmatic way.

But thats it.

It hasn't been tested in production environment with multiple crawler threads and it is very sensitive to changes made by Idealista. Also it is not well design for having more sources.

But it's a start, happy scraping!
