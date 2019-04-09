---
layout: post
title:  "Precio de aquiler de garages en barrios de Madrid"
lang: es
ref: madrid-neighborhood-monthly-parking
description: "Mapa de alquileres medianos de garajes en los barrios del municipio de Madrid."
date:   2018-02-02 19:10:22 +0100
tags: ["Data", "Visualization", "Python", "d3.js"]
categories: ["Maps"]
---
## Datos
Es difícil encontrar información limpia y detallada sobre el mercado inmobiliario en España. Es posible encontrar información sobre precios de casas (aunque no de garajes, oficinas u otras tipoligías de inmuebles) y el municipio es la mínima entidad territorial de la que se disponen datos.

La ciudad de Madrid es un ejemplo de municipio.

No hay datos para garajes. Tampoco hay datos para distritos o barrios en Madrid.

[idealista.com][idealista] es el lider del marketplace online inmobiliario en España. Ofrecen [algunos datos][idealista-prices] y una [API limitada][idealista-api].

No es demasiada información.

Se ha tomado una captura del mercado de alquiler de garjes de Idealista para un día aleatorio en febrero 2018. Se ha utilizado los frameworks de Python [Scrapy][scrapy] (para scrapear) y [Django][django] (para consultas a través de su ORM).
- Cómo integrar [Django y Scrapy]({{ site.baseurl }}{% post_url 2017-12-01-django-scrapy %}). Intentaré explorar más adelante este tema en mayor profundidad.
- Dedomeno

Se ha utlizado la libreria ´pandas´ para limpiar los datos en un formato DataFrame, agrupándolos por código de municipio, distrito y barrio para calcular el precio mediano. También se ha añadido el geocode oficial para cada barrio.

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

Es posible descargar el fichero csv:
[rent-garage-madrid-feb.csv]({{"/assets/posts/madrid-neighbourhood-monthly-parking/rent-garage-madrid-feb.csv" | absolute_url}})
donde `price` es el precio mediano para cada barrio.

## Visualización

Una vez obtenido los datos, es útil representarlos en un mapa para un mejor entendimiento.
Este es el resultado:

<div class="full">
    <img class="img-fluid" src="/assets/posts/madrid-neighborhood-monthly-parking/madrid-realestate-garage.png">
</div>

**¿Cómo?**

El campo geocode es la concatenación de los códigos de distrito y barrio. Así es como el [Instituto Nacional de Estadistica español](http://www.ine.es/) y los datos abiertos del [Ayuntamiento de Madrid](https://datos.madrid.es/) lo representan.

[Martgnz][martgnz] mantiene un elegante [TopoJSON of Madrid][martgnz-madrid] que incluye las fronteras de los barrios y distritos de Madrid.

Se ha usado d3.js para visualizar el precio medio de cada barrio en una escala de grises (claro precios más bajos, oscuro más altos).

Como era de esperar los precios en el centro de la ciudad son los más elevados:
+ mayor poder adquisitivo.
+ mayor densidad de población.
+ edificios antiguos sin garajes.
+ más zonas peatonales sin parking en calle.

## Conclusiones 

#### Precios & espacio muestral
Este experimento no representa el precio real de alquiler de los garajes en Madrid. Únicamente es una captura del precio anunciado par aun día concreto. Así pues, los datos son incompletos y el tamaño de la muestra es pobre.

Es posible evitar este problema si se toma una captura diaría durante un mes de estos datos de Idealista.como y se eliminan de los datos los garajes que llevan tiempo online.
Esto ampliaría el tamaño muestral y evitaría tener datos de los garajes que tienen un precio fuera del mercado.

#### Mapa interactivo
Es un mapa interesante pero no da demasiada información del precio mediano real por barrio. Tampoco visualiza las fronteras entre distritos o el nombre de los barrios.

En la siguiente entrada, [Precio mediano de alquiler de habitaciones en municipios españoles]({{ site.baseurl }}{% post_url 2018-02-12-spanish-median-rental-room-prices %}) se intenta incorporar las mejoras anteriormente detectadas.

[django]: https://www.djangoproject.com/ 
[scrapy]: https://scrapy.org
[idealista]: https://idealista.com 
[idealista-prices]: https://www.idealista.com/informes-precio-vivienda
[idealista-api]: http://developers.idealista.com/access-request
[martgnz-madrid]: https://github.com/martgnz/madrid-atlas
[martgnz]: https://github.com/martgnz/
