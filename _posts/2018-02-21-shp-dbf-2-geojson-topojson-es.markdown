---
layout: post
title:  "Componer un mapa GeoJSON & TopoJSON de las secciones censales españolas"
lang: es
ref: shp-2-geojson
description: "Cómo componer un mapa GeoJSON y TopoJSON desde los archivos dbf y shp."
date:   2018-01-13 16:17:01 +0100
categories: ["Data", "Visualization"]
---
## Introducción
Las administraciones públicas distribuyen sus mapas (generalmente) en el formato Shapefile.

## Shapefile

El shapefile es un formato vectorial geoespacial introducido a principio de los 90. Realmente está compuesto por una colección de al menos tres archivos:
- `.shp: figuras en binario (polígonos...), son los delimitadores geométricos.
- `.dbf`: los datos asociados a las figuras o registros. En formato dBase.
- `.shx`: un índice de figuras para poder indexar los datos más rápidamente.

También pueden contener otros archivos como `.proj`, `.shp.xml`, `.sbn` ...

En este ejemplo se va a utilizar el shapefile ([INE Cartografía digitalizada](http://www.ine.es/censos2011_datos/cen11_datos_resultados_seccen.htm)) de las [secciones censales] (http://en.eustat.eus/documentos/elem_3830/definicion.html) de España:

<div class="full">
    <a href="/assets/posts/{{page.ref}}/ine-spain.png">
    <img class="img-fluid" src="/assets/posts/{{page.ref}}/ine-spain.png">
    </a>
</div>

A pesar de ser útiles para muchas cosas, los shapefiles no fácilmente legibles por el ser humano o integrables en una página web. Su principal problema es el tamaño del archivo `.shp`:

{% highlight console %}
ginopalazzo@MacBook-Air cartografia_censo2011_nacional $ ls -la
total 777048
        352 Feb 22 12:29 .
       1952 Feb 22 12:23 ..
   11220226 Mar 14  2014 SECC_CPV_E_01_R_INE.dbf
        404 Mar 14  2014 SECC_CPV_E_01_R_INE.prj
     363236 Mar 14  2014 SECC_CPV_E_01_R_INE.sbn
      10172 Mar 14  2014 SECC_CPV_E_01_R_INE.sbx
  109242236 Mar 14  2014 SECC_CPV_E_01_R_INE.shp
      19492 Mar 14  2014 SECC_CPV_E_01_R_INE.shp.xml
     287780 Mar 14  2014 SECC_CPV_E_01_R_INE.shx
{% endhighlight %}

109Mb of shapes and 11Mb of data is not the best way to represent a map in a browser.

## GeoJSON & TopoJSON

GeoJSON, basado en JSON, permite almacenar colecciones de tipos de datos geométricos (incluyendo atributos no espaciales) en una única estructura.

Una extensión interesante de GeoJSON es TopoJson, que codifica la topología geoespacial y proporciona ficheros de salida de tamaños significativamente menores (hasta un 80%).

{% highlight console %}
  241037101 Feb 22 12:29 SECC_CPV_E_01_R_INE_geoJSON.json
   35648891 Feb 22 12:24 SECC_CPV_E_01_R_INE_topoJSon.json
   11220226 Mar 14  2014 SECC_CPV_E_01_R_INE.dbf
        404 Mar 14  2014 SECC_CPV_E_01_R_INE.prj
  109242236 Mar 14  2014 SECC_CPV_E_01_R_INE.shp
     287780 Mar 14  2014 SECC_CPV_E_01_R_INE.shx
{% endhighlight %}

Cómo convertir shapefiles a archivos derivados de JSON?

#### Navegador web
- [Mapsharer](http://mapshaper.org/): El mejor. Shapefile, geojson, topojson, csv, svg y representaciones en mapa.
- [geojson-topojson](http://jeffpaine.github.io/geojson-topojson/): GeoJSON <-> TopoJSON.
- [Distillery](http://shancarter.github.io/distillery/): GeoJSON -> TopoJSON & Map representation.

#### NodeJS & CL
[Mike Bostock](https://medium.com/@mbostock) ha escrito largo y tendido sobre este tema con muchos ejemplos interesantes:
https://medium.com/@mbostock/command-line-cartography-part-1-897aa8f8ca2c

#### Python
Hay algunas soluciones gist con diferentes librerias en Python:
- [json+ogr](https://gist.github.com/AlexArcPy/2fc9f41ca164f76fcbb30ebca273b59f )
- [Bulk convert shapefiles to geojson using ogr2ogr](https://gist.github.com/benbalter/5858851)
- [PyShp, shp to geojson in python](https://gist.github.com/frankrowe/6071443)
- [Fiona](https://gist.github.com/jwass/6245313)
