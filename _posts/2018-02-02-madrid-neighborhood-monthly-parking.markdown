---
layout: post
title:  "Madrid neighbourhood monthly parking"
description: "Map representation of the median price by neighbourhood in Madrid municipality area."
date:   2018-02-02 19:10:22 +0100
categories: ["Data", "Visualization", "Python", "d3.js"]
---
## Data
It's hard to find well detailed data about the Real Estate market in Spain. Usually the data prices are only for houses (not car parks, offices nor sites) and the municipality is minimum territorial entity of any property data.

The city of Madrid, for example, is a municipality.

No data for garages. No real estate data prices for districts or neighbourhoods of Madrid found.

[idealista.com][idealista] is the leader of the real estate on-line marketplace in Spain. They offer [some data][idealista-prices] and a limited [API][idealista-api].

Poor information.   

I decided to take a snapshot of Idealista garage market for a random day in February. I used [Scrapy][scrapy] (for scraping) and [Django][django] (for handy queries) python frameworks. I wrote a small note about how I've integrated [Django and Scrapy]({{ site.baseurl }}{% post_url 2017-12-01-django-scrapy %}) and I will try to explore deeper this topic later. I also used pandas library to clean the data in a DataFrame format, grouping by Municipality, district and neighbourhood code to calculate the median. I also added the official geocode for each neighbourhood.

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
[rent-garage-madrid-feb.csv]({{"/assets/posts/madrid-neighbourhood-monthly-parking/rent-garage-madrid-feb.csv" | absolute_url}})
where `price` is the median price for each neighbourhood.

## Visualization 

Once you have the data, it's useful to represent it in a map to understand it more easily.
This is the output:

<div class="full">
    <img class="img-fluid" src="/assets/posts/madrid-neighborhood-monthly-parking/madrid-realestate-garage.png">
</div>

**How?**

The field geocode is the string join between the district's and the neighbourhood's code. That's how [Spanish National Statistical Institute](http://www.ine.es/) and [Madrid CityHall Open Data](https://datos.madrid.es/) represent it.

And I found a really nice and well maintained [TopoJSON of Madrid][martgnz-madrid] with the neighbourhoods and districts borders made by [martgnz][martgnz].

I use d3.js to visualize the median prices of each neighbourhood by a gray scale (lighter lower, darker higher prices).

As we expected the prices in the city center are the highest:
+ high purchasing power
+ high population density
+ old buildings without parking
+ more pedestrian streets.

## Conclusions 

#### Offer prices & sample size
This experiment does not represent the real garage rental prices in Madrid. They are a snapshot only of the offer prices for a day. So the data is incomplete and more sample size is required.

We could avoid this problem if we take an snapshot every day for a month of the Idealista.com data and delete the garages that have been on line for long periods of time. This would avoid the highest garage prices that are not going to be rented and also we could make our sample size bigger.

#### Interactive map
Is a nice map but it doesn't give us much information about the exact median price by neighbourhood, neither the district borders (grouping inside the neighbourhoods) or neighbourhood names (usually people don't recognize the neighbourhood only by shape...).

I'll try to avoid this problem in the next post, [Median rental room prices in Spanish municipalities]({{ site.baseurl }}{% post_url 2018-02-12-spanish-median-rental-room-prices %}).

[django]: https://www.djangoproject.com/ 
[scrapy]: https://scrapy.org
[idealista]: https://idealista.com 
[idealista-prices]: https://www.idealista.com/informes-precio-vivienda
[idealista-api]: http://developers.idealista.com/access-request
[martgnz-madrid]: https://github.com/martgnz/madrid-atlas
[martgnz]: https://github.com/martgnz/
[jekyll-talk]: https://talk.jekyllrb.com/
