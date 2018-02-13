---
layout: post
title:  "Madrid Neighborhood monthly parking"
description: "Map representation of the median price by neighborhood in Madrid municipality area."
date:   2018-02-02 19:10:22 +0100
categories: ["Data", "Visualization", "Python", "d3.js"]
---
## Data
It's hard to find well formed data about the Real Estate market in Spain. Usually the data prices are only for houses (not parkings, offices or lands) and the minimun territorial entity that holds the property is the municipality.

The city of Madrid, for example, is a municipality.

No data of garages. No real estate data prices of districts or neighborhood of Madrid

[idealista.com][idealista] is the leader of the real estate online marketplace in Spain (aslo in Portugal and Italy I think). They offer [some data][idealista-prices] and an [API][idealista-api].

Poor information.

I decided to take a snapshot of Idealista garage market for a random day in february. I use Scrapy (for scraping) and Django (for handy queries) python frameworks. I wrote a small note about using [Django][django] + [Scrapy][scrapy]. Also I used pandas library to clean the data in a DataFrame format, grouping by Municipality, district and neighborhood code to calculate the median. Also add the official geocode for each neighborhood.

Yo can download the csv from:
[rent-garage-madrid-feb.csv]({{"/assets/madrid-neighborhood-monthly-parking/rent-garage-madrid-feb.csv" | absolute_url}})

## Visualization 
 
<a href="https://github.com/martgnz/madrid-atlas">https://github.com/martgnz/madrid-atlas</a>

![Garage prices in Madrid]({{"/assets/madrid-neighborhood-monthly-parking/madrid-realestate-garage.png" | absolute_url}})

more

## Conclusions 

more

[django]: https://www.djangoproject.com/ 
[scrapy]: https://scrapy.org
[idealista]: https://idealista.com 
[idealista-prices]: https://www.idealista.com/informes-precio-vivienda
[idealista-api]: http://developers.idealista.com/access-request
[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
