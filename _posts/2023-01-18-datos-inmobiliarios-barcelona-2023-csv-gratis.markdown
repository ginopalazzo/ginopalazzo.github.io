---
layout: post
title:  "Vivienda Barcelona 2023 (archivo .csv)"
lang: es
ref: 2023-Barcelona-Real-Estate-Data-Free-CVS
description: "130.000 anuncios de venta y alquiler del municipio de Barcelona en 2023 en un archivo .csv gratuito"
date:   2023-01-18 12:17:42 +0100
categories: ["Scraper"]
tags: ["API", "Data","Python"]
---

### Introducción

La vivienda (y su precio) es percibido por los españoles como uno de los principales problemas del país en el año 2023:
<div class="full">
    <a href="/assets/posts/{{page.ref}}/eu-problems.jpeg">
    <img style="max-width:620px;  width:100%;" class="img-fluid" src="/assets/posts/{{page.ref}}/eu-problems.jpeg">
    </a>
</div>

Entre otras medidas para rebajar el precio de la vivienda, varios expertos inmobiliarios proponen publicar los precios del mercado inmobiliario por referencia catastral.

`La referencia catastral es un identificador único por el que se describen los bienes inmuebles de España (salvo País Vasco y Navarra).`

### ¿Cómo obtener datos del mercado inmobiliario en España?

Para tener una imagen completa del mercado inmobiliario español sería necesario conocer los precios de compra-venta y alquiler de los inmuebles.

Los <ins>datos de compraventa **reales**</ins> solo los disponen los notarios y [registradores](https://www.registradores.org/){:target="_blank"}. Este precio aun así puede no ser fiable del todo porque en algunas transacciones se acuerda pagar en metálico (pago en B) una parte del inmueble para obtener ventajas impositivas.

Los <ins>datos de alquiler **reales**</ins> solo están en mano de las inmobiliarias (y sus agrupaciones), introduciendo sesgos en el precio ya que no se tiene en cuenta el alquiler entre particulares.

Estos datos o no son accesibles o el coste es prohibitivo.

Otras opciones para obtener datos del mercado inmobiliario español pasan por los portales inmobiliarios. Estos datos indican el precio deseado por el anunciante, pero no el precio final. Aun así son datos que, tratados adecuadamente, pueden resultar interesantes.

[Idealista.com](https://www.idealista.com){:target="_blank"} es el líder del mercado de anuncios inmobiliarios en España (Italia y Portugal).

### Los datos

Durante el 2023 se publicaron alrededor de 130.000 anuncios de viviendas en el término municipal de Barcelona.

Para que la información fuese manejable en un csv, cada anuncio se ha reducido a los datos de la tabla inferior. Como se puede observar, además del nombre de la columna se indica el número de instancias y su descripción:

{% highlight python %}
df.count()
title                       127497  # título del anuncio
operation                   127497  # rent/sale
state                       127497  # activo o incativo
address                     127497  # dirección
latitude                    127497  # latitud de coordenadas
longitude                   127497  # longitud de coordendas
hasHiddenAddress            127497  # si la dirección es exacta
administrativeAreaLevel1    127497  # provincia
administrativeAreaLevel2    127497  # municipio
administrativeAreaLevel3    127497  # distrito
administrativeAreaLevel4    127497  # barrio
isProfessional              127497  # es de inmobiliaria
commercialName              127497  # nombre de la inmobiliaria
date                        127497  # fecha
price                       127497  # precio
houseType                   127497  # tipo de casa
extendedPropertyType         17370  # tipo extendido de casa
constructedArea             127497  # m2 construidos
usableArea                   71818  # m2 usables
plotOfLand                    1204  # m2 de parcela
roomNumber                  127497  # número de habitacions
bathNumber                  127497  # número de baños
isInTopFloor                127497  # es última planta?
isDuplex                    127497  # es un duplex?
flatLocation                185874  # es interior o exterior?
isStudio                    127497  # es un estudio?
isPenthouse                 127497  # es un ático=
energyCertificationType     127497  # certificado energético
energyPerformance            36388  # medidad del anterior
status                      127497  # estado del inmueble
lift                        127497  # ascensor
boxroom                      53904  # trastero
swimmingPool                 50557  # piscina
garden                       49044  # jardín
communityCosts               15307  # costes de comunidad
heaterType1                  85532  # tipo de calefacción
heaterType2                  48963  # tipo de calefacción 2
construction_year            76702  # año de construcción
manyFloors                    1287  # tiene varios pisos
orientation                  63163  # orientación del inmueble
terrace                      40872  # terraza
hasGarage                    11297  # garaje
garagePrice                   1820  # precio del garaje
floorNumber                 117731  # número de planta
airConditioning              68651  # aire acondicionado
{% endhighlight %}

Un ejemplo de un registro o anuncio sería el siguiente:

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

### CSV de la Vivienda de Barcelona en 2023
La forma más eficiente (sin contar un API pública) para compartir esta información es mediante un archivo csv. Para ello se han pasado los datos de una BBDD a un dataframe de Python mediante una query del ORM de Django (posteriormente mejorando los tiempos y eficiencia con FastAPI) y después se ha utilizado la función df.to_csv(). Todo el proceso lleva unos 10 segundos (aunque se podría optimizar bastante):

{% highlight python %}
import pandas as pd
query = House.objects.filter('XX').values('YY')
df = pd.DataFrame(query.iterator())
df.to_csv('barcelona-bcn-2023.csv', sep ='|')  
{% endhighlight %}


- <button class="button-55" role="button">[GitHub] barcelona-bcn-2023.csv</button>

- <button class="button-55" role="button">[Kaggle] barcelona-bcn-2023.csv</button>

Si no se tiene experiencia con la librería pandas de Python, se recomienda utilizar [VisiData](https://www.visidata.org/){:target="_blank"} para explorar archivos csv de gran tamaño.

{% highlight zsh %}
vd --csv-delimiter "|" barcelona-bcn-2023.csv
{% endhighlight %}

### Más información

Como he mencionado anteriormente, este archivo csv es una pequeña muestra de la información disponible:
- Otros tipos de inmuebles como oficinas, garajes, habitaciones ...
- 8130 municipios más además del término municipal de Barcelona
- Información espacial de las distintas áreas geográficas
- Registro de cada cambio de precio de un inmueble
- Registro de cada fecha de alta y baja del inmueble
- Datos de todas las inmobiliarias de España
- ...

Cualquier duda, puede contactar a través de:

<div class="col-4 d-flex ">
    <a target="_blank" href="https://github.com/{{ site.github_username| cgi_escape | escape }}"><span class="icon-github social-icon">
    <a target="_blank" href="https://twitter.com/{{ site.twitter_username| cgi_escape | escape }}"><span class="icon-twitter social-icon">
    <a target="_blank" href="https://www.linkedin.com/in/ginesramirez/?locale=en_US"><span class="icon-linkedin social-icon">
    <a target="_blank" href="mailto:{{ site.email }}"><span class="icon-at-sign social-icon"> </span>
