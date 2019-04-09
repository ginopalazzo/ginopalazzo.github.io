---
layout: post
title:  "Median rental room prices in Spanish municipalities"
lang: en
ref: spanish-median-rental-room-prices
description: "Map representation of the median room prices for Spanish municipalities."
date:   2018-02-12 11:30:00 +0100
tags: ["Data", "Visualization", "Python", "d3.js"] 
categories: ["Maps"] 
---
## Overview

Spanish real estate market can be intense:
* Wages are one of the lowest in the E.U. (most common annually salary, statistical mode, is 16,490€. Around 1,100€ per month after taxes)
* High rate of unemployment.
* Job vacancies concentrate in the big cities.
* Incredible rise of the house rent prices in cities like Madrid and Barcelona.

<div class="full">
    <img class="img-fluid" src="/assets/posts/{{page.ref}}/barcelona-room-rent.png">
</div>

Given the situation, room sharing is becoming the only choice for many employees that want to live close to their place of work.

Let's try to explore a little deeper what is happening in the Spanish room rental market with the next interactive map.

<iframe id="js-iframe" class="container-fluid" sandbox="allow-popups allow-scripts allow-forms allow-same-origin" src="/assets/posts/spanish-median-rental-room-prices/playground.html" marginwidth="0" marginheight="0" style="height:650px; border:none;width: 990px; display: block; margin: 0px auto; " scrolling="no"></iframe>

You can explore the map in a separate page to see a [cleaner source code](/assets/posts/spanish-median-rental-room-prices/playground.html)

Read further to understand the data and visualization behind it.
## Data
There is a mayor digital player in the Spanish room sharing advertising market, [idealista.com](https://www.idealista.com), and lots of secondary players like [Badi](https://badiapp.com/), [Milanuncios](https://www.milanuncios.com/pisos-compartidos/), [Spotahome](https://www.spotahome.com/es/alquiler/madrid/habitaciones-amuebladas)...

I've extracted the data from idealista because I've already made a scrapper for this web-page: [Dedomeno: A Spanish real estate (Idealista) python scraper]({{ site.baseurl }}{% post_url 2018-02-10-dedomeno-spanish-real-estate-python-scraper %}).

I took a snapshot of Idealista room renting market for Spain on a random day in February. I used, like I did in [Madrid Neighborhood monthly parking]({{ site.baseurl }}{% post_url 2018-02-02-madrid-neighborhood-monthly-parking %}) post, [Scrapy (for scraping) and Django (for simple queries) python frameworks]({{ site.baseurl }}{% post_url 2017-12-01-django-scrapy %}).

I used pandas library to clean the data in a DataFrame format, grouping by province and municipality  code to calculate the median. I also added the official geocode `m_code` for each municipality so it could be compatible with the [Spanish National Statistical Institute (INE)](http://www.ine.es/) municipality code: `m_code = int(province) + munic`

{% highlight csv %}
# Panda DataFrame grouping by administrative boundaries
country  province  area  zone  munic  m_code     € median
ES       01        01    001   059    1059     287.5
                   02    001   001    1001     175.0
                               021    1021     600.0
                   04    001   031    1031     250.0
                   05    001   901    1901     200.0
                   06    001   002    1002     260.0
                               036    1036     225.0
         02        01    001   003    2003     175.0
                         003   029    2029     125.0
                               039    2039     150.0
                   02    001   069    2069     155.0
                         003   081    2081     180.0
                   05    001   037    2037     175.0
         03        01    001   137    3137     300.0
                         002   063    3063     200.0
                         003   138    3138     130.0
                         004   082    3082     250.0
                         006   047    3047     250.0
                         007   026    3026     200.0
                               030    3030     250.0
                               110    3110     275.0
                                               ...  
                         003   032    48032    250.0
                         004   095    48095    315.0
                   06    001   065    48065    230.0
                         004   025    48025    220.0
                               075    48075    212.5
                   07    002   090    48090    300.0
         49        01    001   275    49275    150.0
                   02    001   219    49219    150.0
         50        02    001   165    50165    250.0
                   08    001   095    50095    170.0
                               252    50252    100.0
                   14    003   008    50008    300.0
                   17    001   297    50297    250.0
                         002   272    50272    225.0
                         003   089    50089    232.5
                               131    50131    230.0
                         004   219    50219    205.0
                         005   235    50235    250.0
                               288    50288    300.0
         51        01    001   001    51001    290.0
         52        01    001   001    52001    300.0
  
{% endhighlight %}
You can download the csv from:
[spain_muni_median_rent_room.csv]({{"/assets/posts/spanish-median-rental-room-prices/spain_muni_median_rent_room.csv" | absolute_url}})

As you can see in the interactive map at the beginning of this post, there is a huge number of municipalities with no data.

For example Guadalajara province only has a few municipalities close to Madrid province where people offer a room to rent. This is because in rural areas the price of a full apartment is lower than renting a room in big cities.

<div class="full">
    <img class="img-fluid" src="/assets/posts/{{page.ref}}/guadalajara-room-rent.png">
</div>

Exploring the map further we can see a rare phenomenon; the highest room rental prices are on two small islands in the Mediterranean sea, part of the Balearic Islands: Ibiza and Formentera.
This could be explained because there is a high demand for holiday apartments (specially with airbnb) and there are not enough houses available for the local people to afford them.

If you know how to read Spanish, take a look at this newspaper article called "No place to live in Ibiza": 
[En Ibiza no hay quien viva](https://www.elconfidencial.com/vivienda/2017-03-05/ibiza-alquiler-apartamento-turismo_1341558/)

<div class="full">
    <img class="img-fluid" src="/assets/posts/{{page.ref}}/formentera-room-rent.png">
</div>

In Ibiza we can check what is the sample size for each municipality, and for example in Sant Josep de sa Talia or Santa Euleria des Rius there are around 10 rooms for rent:

{% highlight python %}
# Formentera (7024)
     province area zone munic district neighborhood  code  
8943       07   04  001   024     None         None  7024

Sant Josep de sa Talaia (7048)
df2.loc[df2['code']=='7048'].count()
geocode_raw     12
Santa Euleria des Riu
df2.loc[df2['code']=='7054'].count()
geocode_raw     10
{% endhighlight %}

## Visualization 
[Martín Gonzalez](https://github.com/martgnz) maintains a repository that provides a simple script to generate TopoJSON files from the Spanish National Geographic Institute’s National Reference Geographic Equipment vector data called [es-atlas](https://github.com/martgnz/es-atlas), that is inspired in Mike Bostock’s [us-atlas](https://github.com/topojson/us-atlas) and [world-atlas](https://github.com/topojson/us-atlas).
He also maintains with Lukas Appelhans [Span](https://github.com/newsappsio/spam), a small library to create modern Canvas maps with [D3](https://github.com/d3/d3).

Lazy as I am, I just copied one of his examples, changed the legends, more detailed administrative borders, colors and made a small snippet directly in django console to change the `rate` variable:

{% highlight python %}
for i in range(len(data['objects']['municipios']['geometries'])):
    municipality_id = data['objects']['municipios']['geometries'][i]['id']
    price = 0
    try:
        price = m.loc[municipality_id].price_raw
    except KeyError:
        pass
    data['objects']['municipios']['geometries'][i]['properties']['rate'] = price
{% endhighlight %}

You can download the json used in this example from:
* [municipalities.json](/assets/posts/{{page.ref}}/municipalities.json)
* [provinces.json](/assets/posts/{{page.ref}}/provinces.json)

## Conclusions

As happened in last post, [Madrid Neighborhood monthly parking]({{ site.baseurl }}{% post_url 2018-02-02-madrid-neighborhood-monthly-parking %}), we have a poor quality sample size:

{% highlight python %}
df2.count()
geocode_raw     16653
price_raw       16653
planet          16653
continent       16653
country         16653
province        16653
area            16653
zone            16653
munic           16653
district        15526
neighborhood    10213
code            16653
dtype: int64
{% endhighlight %}

This is because there aren't any final prices (16,5K rooms is a decent sample). We don't have a way to be sure what the final price of that room was.

As we concluded in the previous post, we could avoid this issue if a snapshot were taken every day for at least one month and get rid of those items that have been on-line for a long period of time. This would avoid the highest items prices that are not going to be rented and also we could make our sample size bigger.

On the other hand, this map is far more understandable; it's interactive, zoomable and it shows information of the name and median rent price.