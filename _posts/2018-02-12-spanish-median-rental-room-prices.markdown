---
layout: post
title:  "Median rental room prices in Spanish municipalities"
description: "Map representation of the median price by neighborhood in Madrid municipality area."
date:   2018-02-12 11:30:00 +0100
categories: ["Data", "Visualization", "Python", "d3.js"]  
---

## Overview

hello!
<iframe class="container-fluid" sandbox="allow-popups allow-scripts allow-forms allow-same-origin" src="/assets/posts/spanish-median-rental-room-prices/playground.html" marginwidth="0" marginheight="0" style="height:650px; border:none;width: 990px; display: block; margin: 0px auto; " scrolling="no"></iframe>
## Data
It's hard to find well formed data about the Real Estate market in Spain. Usually the data prices are only for houses (not parkings, offices or lands) and the minimun territorial entity that holds the property is the municipality.

The city of Madrid, for example, is a municipality.

No data of garages. No real estate data prices of districts or neighborhood of Madrid found.

[idealista.com][idealista] is the leader of the real estate online marketplace in Spain (aslo in Portugal and Italy I think). They offer [some data][idealista-prices] and an [API][idealista-api].

Poor information.

I decided to take a snapshot of Idealista garage market for a random day in february. I use Scrapy (for scraping) and Django (for handy queries) python frameworks. I wrote a small note about using [Django][django] + [Scrapy][scrapy]. Also I used pandas library to clean the data in a DataFrame format, grouping by Municipality, district and neighborhood code to calculate the median. Also add the official geocode for each neighborhood.


{% highlight csv %}
# Panda DataFrame groupping by administrative boundaries
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
                         009   095    3095     200.0
                         010   041    3041     350.0
                         012   071    3071     190.0
                   02    001   098    3098     180.0
                               127    3127     400.0
                         002   018    3018     250.0
                         003   031    3031     300.0
                         004   069    3069     250.0
                         005   139    3139     225.0
                                               ...  
         48        02    004   089    48089    340.0
                         005   038    48038    310.0
                               064    48064    300.0
                               069    48069    300.0
                   03    001   017    48017    250.0
                         003   046    48046    200.0
                   04    001   915    48915    250.0
                   05    001   003    48003    300.0
                         002   027    48027    240.0
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
Hello!

## Conclusions