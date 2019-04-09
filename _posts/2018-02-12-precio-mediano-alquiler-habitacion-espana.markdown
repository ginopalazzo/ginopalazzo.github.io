---
layout: post
title:  "Cuánto cuesta una habitación según el municipio en España"
lang: es
ref: spanish-median-rental-room-prices
description: "Mapa que representa el precio mediano de alquiler de habitaciones en España."
date:   2018-02-12 11:30:00 +0100
tags: ["Data", "Visualization", "Python", "d3.js"]  
categories: ["Maps"]
---
## Introducción

El mercado inmobiliario español puede ser intenso:
* Lo salarios son uno de los más bajos en la Unión Europea (el salario anual más común, la moda estadística, es de 16,490€. Alrededor de 1,100€ al mes después de impuestos).
* Alta tasa de desempleo.
* Ofertas de trabajo concentradas en las grandes ciudades..
* Auge desmesurado en el precio de alquiler en ciudades como Madrid y Barcelona (aunque no solo).

<div class="full">
    <img class="img-fluid" src="/assets/posts/{{page.ref}}/barcelona-room-rent.png">
</div>

Dada esta situación, compartir piso se ha convertido en la única solución para muchos trabajadores que quieren vivir cerca de donde trabajan.

Se va a tratar de explorar un poco más en profundida qué está pasando en el mercado de alquiler de habitación en España con el siguiente mapa interactivo:

<iframe id="js-iframe" class="container-fluid" sandbox="allow-popups allow-scripts allow-forms allow-same-origin" src="/assets/posts/spanish-median-rental-room-prices/playground.html" marginwidth="0" marginheight="0" style="height:650px; border:none;width: 990px; display: block; margin: 0px auto; " scrolling="no"></iframe>

Se puede explorar el mapa en una página distinta para ver mejor el [código fuente asociado](/assets/posts/spanish-median-rental-room-prices/playground.html).

A continuación se explica cómo se han sacado los datos y se ha hecho el mapa.
## Datos
El mayor actor digital en el mercado de los marketplaces de alquiler de habitaciones es [idealista.com](https://www.idealista.com). También hay muchos actores secundarios como [Badi](https://badiapp.com/), [Milanuncios](https://www.milanuncios.com/pisos-compartidos/), [Spotahome](https://www.spotahome.com/es/alquiler/madrid/habitaciones-amuebladas)...

Se han extraido los datos de Idealista porque ya había desarrollado un scrapeador que funcionaba medianamente bien extrayendo datos de Idealista: [Dedomeno: Extrayendo datos de Idealista. El mercado inmobiliario español]({{ site.baseurl }}{% post_url 2018-02-10-dedomeno-espana-scrapeador-python-inmobiliario %}).

Se ha tomado una captura del mercado de alquilerde habitaciones en España mostrado en Idealista para un día aleatorio de febrero 2018. Se ha usado, al igual que en la entrada [Precio de aquiler de garages en barrios de Madrid]({{ site.baseurl }}{% post_url 2018-02-02-precio-garaje-barrios-madrid %}), dos frameworks de Python: [Scrapy (para el escrapeo) and Django (para consulta sencillas)]({{ site.baseurl }}{% post_url 2017-12-01-django-scrapy-es %}).

También se ha utilizado pandas, una libreria de Python para limpiar los datos en un formato DataFrame, agrupando por el código de provincia y municipio para calcular la mediana. También se ha añadido el geocode oficial `m_code`para cada municipio para que sea compatible con código de municipio dado por el [Instituto Nacional de Estadística (INE)](http://www.ine.es/): `m_code = int(province) + munic`

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
Se puede descargar el csv resultante en :
[spain_muni_median_rent_room.csv]({{"/assets/posts/spanish-median-rental-room-prices/spain_muni_median_rent_room.csv" | absolute_url}})

Como se puede ver en el mapa interactivo al principio de esta entrada, hay muchos municipios sin datos.

Por ejemplo, la provincia de guadalajara solo tiene unos pocos municipios cercanos a Madrid donde hay oferta de alquiler de habitaciones. Esto es debido a que en las áreas rurales el precio de alquilar una casa completa es más bajo que el alquiler de una habitación en las grandes ciudades.
Así pues, el precio de alquiler de un piso en los municipios fronterizos a Madrid ha crecido considerablemente, haciendo atractivo el alquiler de habitaciones en estos municipios.

<div class="full">
    <img class="img-fluid" src="/assets/posts/{{page.ref}}/guadalajara-room-rent.png">
</div>

Explorando en profundidad el mapa podemos ver un fenómeno extraño; el mayor precio de alquiler de habitaciones se situa en las dos pequñenas islas del Mediterrano Ibiza y Formentera.
Esto puede ser explicado por la alta demanda existente en alquiler vacacional (especialmente con Airbnb) y no hay suficientes casas disponibles para el alquiler de larga estancia.

Esto lo cuenta en mayor detalle [Alfredo Pascual](http://www.twitter.com/Guyb) en un artículo en El Confidencial llamado [En Ibiza no hay quien viva](https://www.elconfidencial.com/vivienda/2017-03-05/ibiza-alquiler-apartamento-turismo_1341558/)

<div class="full">
    <img class="img-fluid" src="/assets/posts/{{page.ref}}/formentera-room-rent.png">
</div>

En Ibiza se puede comprobar cuál es el tamaño de la muestra para cada municipio. Por ejemplo en Sant Josep de sa Talia o Santa Euleria des Rius hay alrededor de 10 habitaciones en alquiler:

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

## Visualización 
[Martín Gonzalez](https://github.com/martgnz) mantiene un repositorio que proporciona un simple script para general archivos TopoJSON a partir del [Instituto Geografico Nacional español](http://www.ign.es/ign/main/index.do) llamado [es-atlas](https://github.com/martgnz/es-atlas). Este está inspirado en [us-atlas](https://github.com/topojson/us-atlas) y [world-atlas](https://github.com/topojson/us-atlas) de Mike Bostock.
También mantiene [Span](https://github.com/newsappsio/spam) junto con Lukas Appelhans, una pequeña libreria para crear mapas Canvas modernos con [D3](https://github.com/d3/d3).

Se ha copiado uno de sus ejemplos, cambiado las leyendas y colores, detallado más las fronteras administrativas y se ha hecho un pequeño snippet directamente en la consola de Django para cambiar la variable `rate`:

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

Se puede descargar el json usado en este ejemplo de:
* [municipalities.json](/assets/posts/{{page.ref}}/municipalities.json)
* [provinces.json](/assets/posts/{{page.ref}}/provinces.json)

## Conclusiones

Como ocurrió en la entrada anterior, [Precio de aquiler de garages en barrios de Madrid]({{ site.baseurl }}{% post_url 2018-02-02-precio-garaje-barrios-madrid %}), la calidad de la muestra es pobre:

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

Esto es debido a que no se dispone de precios finales (16,5K habitaciones es una muestra decente). No se tiene forma de saber cuál fue el precio final de alquilier de la habitación.

Tal y como se concluyó en la entrada anterior, esto se podía haber evitado si se hubiese tomado una captura diária durante al menos un mes y deshacerse de aquellos inmuebles que estuvieran activos durante largos periodos de tiempo. Esto evitaría los precios inflados de habitaciones que jamás de alquilarían a la vez que tendríamos una muestra mayor y de mejor calidad.

Por otro lado, este mapa mejora mucho la versión de la entrada anterior: es in interactivo, se puede hacer zoom y muestra la información del nombre del municipio y su precio mediano de alquiler.
