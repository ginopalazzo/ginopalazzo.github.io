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

No data of garages. No real estate data prices of districts or neighborhood of Madrid found.

[idealista.com][idealista] is the leader of the real estate online marketplace in Spain (aslo in Portugal and Italy I think). They offer [some data][idealista-prices] and an limited [API][idealista-api].

Poor information.   

I decided to take a snapshot of Idealista garage market for a random day in february. I use [Scrapy][scrapy] (for scraping) and [Django][django] (for handy queries) python frameworks. I wrote a small note about how I've integrated [Django and Scrapy]({{ site.baseurl }}{% post_url 2017-12-01-django-scrapy %}) and I will try to explore deeper in this topic later. Also I used pandas library to clean the data in a DataFrame format, grouping by Municipality, district and neighborhood code to calculate the median. Also added the official geocode for each neighborhood.

{% highlight csv %}
     municipality  district  neighbourhood  geocode  price
0              79         1              1       11  140.0
1              79         1              2       12  120.0
2              79         1              3       13  165.0
3              79         1              4       14  170.0
4              79         1              5       15  150.0
5              79         1              6       16  160.0
6              79         2              1       21  100.0
7              79         2              2       22   85.0
8              79         2              3       23  100.0

..            ...       ...            ...      ...    ...

120            79        20              6      206   75.0
121            79        20              7      207   70.0
122            79        20              8      208   80.0
123            79        21              1      211   82.5
124            79        21              2      212  100.0
125            79        21              3      213   70.0
126            79        21              4      214   65.0
127            79        21              5      215  100.0
{% endhighlight %}

You can download the csv from:
[rent-garage-madrid-feb.csv]({{"/assets/posts/madrid-neighborhood-monthly-parking/rent-garage-madrid-feb.csv" | absolute_url}})
where `price` is the median price for each neighbourhood.

## Visualization 

Once you have the data, it's usefull to represent it in a map to understand it more easily.
This is the output:

<div class="full">
    <img class="img-fluid" src="/assets/posts/madrid-neighborhood-monthly-parking/madrid-realestate-garage.png">
</div>

**¿How?**

The field geocode is the string join of the code of the district and the neighborhood. This is how is represented each neighborhood in:
* [Spanish National Stadistic](http://www.ine.es/)
* [Madrid CityHall Open Data](https://datos.madrid.es/)

And I found a really nice and well mantained [TopoJSON of Madrid][martgnz-madrid] with the neighborhoods and districts borders made by [martgnz][martgnz].

I use d3.js to visualize the median prices of each neighborhood by a grey scale (lighter lower, darker higher prices).

As we expected the prices in the city center are the highest:
+ high purchasing power
+ high population density
+ old buildings without parking
+ more pedestrian streets.

## Conclusions 

#### Offer prices & sample size
This experiment does not represent the real garage rental prices in Madrid. They are a snaptshoot only of the offer prices for a day. So the data is incomplete and more sample size is requiered.

We could avoid this problem if we take an snaptshoot every day for a month of the Idealista.com data and delete the garages that have been online for long periods of time. This would avoid the highest garage prices that are not going to be rented and also we could make our sample size bigger.

#### Interactive map
Is a nice map but it doesn't give as much information about the exact median price by neighborhood, neither the district borders (grouping inside the neighborhoods) or neighborhood names (usually people dont recognize the neighborhood only by shape...).

I'll try to avoid this problem in the next post, [Median rental room prices in Spanish municipalities]({{ site.baseurl }}{% post_url 2018-02-12-spanish-median-rental-room-prices %}).

[django]: https://www.djangoproject.com/ 
[scrapy]: https://scrapy.org
[idealista]: https://idealista.com 
[idealista-prices]: https://www.idealista.com/informes-precio-vivienda
[idealista-api]: http://developers.idealista.com/access-request
[martgnz-madrid]: https://github.com/martgnz/madrid-atlas
[martgnz]: https://github.com/martgnz/
[jekyll-talk]: https://talk.jekyllrb.com/