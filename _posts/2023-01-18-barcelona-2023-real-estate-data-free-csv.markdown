---
layout: post
title:  "2023 Barcelona Housing (.csv file)"
lang: en
ref: 2023-Barcelona-Real-Estate-Data-Free-CVS
description: "130.000 rent and sale housing ads of Barcelona municipality in a free .csv file"
date:   2023-01-18 12:17:42 +0100
categories: ["Scraper"]
tags: ["API", "Data","Python"]
---

### Introduction

Housing (and of course, its price) is perceived by the spanish citizen as one of the main problems of Spain in the year 2023:
<div class="full">
    <a href="/assets/posts/{{page.ref}}/eu-problems.jpeg">
    <img style="max-width:620px;  width:100%;" class="img-fluid" src="/assets/posts/{{page.ref}}/eu-problems.jpeg">
    </a>
</div>

Several real estate experts have proposed to publish the real estate transaction prices for each house code, as a measure of lowering the price of the Spanish real estate.

`The Spanish "catastral code" is an unique id that describes every real estate item in Spain (except the Basque Country and Navarra).`

### How to get the Spanish real estate data?

To have a complete picture of the Spanish real estate market it would be necessary to know the sale and rental prices of properties.

<ins>**True** sales and purchase prices</ins> are only available to notaries and [Spanish property registration](https://www.registradores.org/){:target="_blank"}. This price may not be completely reliable because in some transactions it is agreed to pay in cash (payment in B) a part of the property to obtain tax advantages.

<ins>**True** rental data prices</ins> are only in the hands of real estate agencies (and their groups), introducing biases in the price since rentals between individuals are not taken into account.

This data is either not accessible or the cost is prohibitive.

Other options to obtain data on the Spanish real estate market go through real estate portals. These data indicate the price desired by the bidder, but not the final price. Even so, they are data that, treated properly, can be interesting.

[Idealista.com](https://www.idealista.com){:target="_blank"} is the market leader in real estate ads in Spain (Italy and Portugal).

### The Data

During 2023, around 130,000 housing advertisements were published in the municipality of Barcelona.

To make the information manageable in a csv file, each ad/record has been reduced to the data in the table below. As you can see, in addition to the name of the column, the number of instances and their description are indicated:

{% highlight python %}
df.count()
title                       127497  # Ad title
operation                   127497  # rent/sale
state                       127497  # active or inactive
address                     127497  # address
latitude                    127497  # latitude
longitude                   127497  # longitude
hasHiddenAddress            127497  # is the address 
administrativeAreaLevel1    127497  # province
administrativeAreaLevel2    127497  # municipality
administrativeAreaLevel3    127497  # district
administrativeAreaLevel4    127497  # neighborhood
isProfessional              127497  # is a agent
commercialName              127497  # name of the agent
date                        127497  # date
price                       127497  # price
houseType                   127497  # house type
extendedPropertyType         17370  # extended house type
constructedArea             127497  # m2 constructed
usableArea                   71818  # m2 usable
plotOfLand                    1204  # m2 of land
roomNumber                  127497  # number of rooms
bathNumber                  127497  # number of wc 
isInTopFloor                127497  # is in the top floor?
isDuplex                    127497  # is a duplex?
flatLocation                185874  # interior or exterior?
isStudio                    127497  # is a studio?
isPenthouse                 127497  # is a penthouse?
energyCertificationType     127497  # energy certification
energyPerformance            36388  # energy performance
status                      127497  # housing status
lift                        127497  # lift
boxroom                      53904  # box room
swimmingPool                 50557  # swimming pool
garden                       49044  # garden
communityCosts               15307  # community costs
heaterType1                  85532  # heater type
heaterType2                  48963  # heater type 2
construction_year            76702  # year of construction
manyFloors                    1287  # has many floors?
orientation                  63163  # orientation
terrace                      40872  # terrace
hasGarage                    11297  # has garage?
garagePrice                   1820  # garage price
floorNumber                 117731  # floor number
airConditioning              68651  # air conditioning
{% endhighlight %}

This is one example of a housing record/add: 

{% highlight python %}
                       
 column             ║ value                                                       ║
                    ║                                                             ║
 title              ║ Ático en Urb. chamberí barrio Nuevos Ministerios-Ríos Rosas ║
 operation          ║ rent                                                        ║
 state              ║ inactive                                                    ║
 address            ║ Urb. chamberí barrio Nuevos Ministerios-Ríos Rosas          ║
 latitude           ║ 40.4390835                                                  ║
 longitude          ║ -3.7002443                                                  ║
 hasHiddenAddress   ║ True                                                        ║
 administrativeArea…║ Madrid                                                      ║
 administrativeArea…║ Madrid                                                      ║
 administrativeArea…║ Chamberí                                                    ║
 administrativeArea…║ Nuevos Ministerios-Ríos Rosas                               ║
 isProfessional     ║ True                                                        ║
 commercialName     ║ Viancasa Gestión inmobiliaria                               ║
 date               ║ 2023-01-01 12:55:16.944851+00:00                            ║
 price              ║ 1800                                                        ║
 houseType          ║ flat                                                        ║
 extendedPropertyTy…║ penthouse                                                   ║
 constructedArea    ║ 120                                                         ║
 usableArea         ║ 115.0                                                       ║
 plotOfLand         ║                                                             ║
 roomNumber         ║ 1                                                           ║
 bathNumber         ║ 2                                                           ║
 isInTopFloor       ║ False                                                       ║
 isDuplex           ║ False                                                       ║
 flatLocation       ║ external                                                    ║
 isStudio           ║ False                                                       ║
 isPenthouse        ║ True                                                        ║
 energyCertificatio…║ c                                                           ║
 energyPerformance  ║                                                             ║
 status             ║ good                                                        ║
 lift               ║ True                                                        ║
 boxroom            ║ True                                                        ║
 swimmingPool       ║ False                                                       ║
 garden             ║ False                                                       ║
 communityCosts     ║                                                             ║
 heaterType1        ║ central                                                     ║
 heaterType2        ║                                                             ║
 construction_year  ║ 1991.0                                                      ║
 manyFloors         ║                                                             ║
 orientation        ║ sur, oeste                                                  ║
 terrace            ║ True                                                        ║
 hasGarage          ║                                                             ║
 garagePrice        ║ 120.0                                                       ║
 floorNumber        ║ Planta 7ª                                                   ║
 airConditioning    ║ True                                                        ║

{% endhighlight %}

### 2023 Barcelona Housing CSV
The most efficient way (not counting a public API) to share this information is through a csv file. To do this, the data from a DB has been passed to a Python dataframe through a query from the Django ORM (later with FastAPI to improve times and efficiency) and then we use the df.to_csv() function. The entire process takes about 10 seconds (although it could be optimized):

{% highlight python %}
import pandas as pd
query = House.objects.filter('XX').values('YY')
df = pd.DataFrame(query.iterator())
df.to_csv('barcelona-bcn-2023.csv', sep ='|')  
{% endhighlight %}


- <button onclick="location.href='/assets/posts/{{page.ref}}/barcelona-bcn-2023.csv'" class="button-55" role="button">[GitHub] barcelona-bcn-2023.csv</button>

- <button class="button-55" role="button">[Kaggle] barcelona-bcn-2023.csv</button>

If you do not have experience with the Python pandas library, it is recommended to use [VisiData](https://www.visidata.org/){:target="_blank"} to explore large csv files.

{% highlight zsh %}
vd --csv-delimiter "|" barcelona-bcn-2023.csv
{% endhighlight %}

### More information

As I mentioned above, this csv file is a small sample of the information available:
- Other types of properties such as offices, garages, rooms...
- 8,130 more municipalities (besides ​​Barcelona)
- Spatial information of the different geographical areas
- Record of each price change of a property
- Data from all real estate agencies in Spain
- ...

For any question, do not hesitate to ask:

<div class="col-4 d-flex ">
    <a target="_blank" href="https://github.com/{{ site.github_username| cgi_escape | escape }}"><span class="icon-github social-icon">
    <a target="_blank" href="https://twitter.com/{{ site.twitter_username| cgi_escape | escape }}"><span class="icon-twitter social-icon">
    <a target="_blank" href="https://www.linkedin.com/in/ginesramirez/?locale=en_US"><span class="icon-linkedin social-icon">
    <a target="_blank" href="mailto:{{ site.email }}"><span class="icon-at-sign social-icon"> </span>
