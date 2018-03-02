---
layout: post
title:  "Compose Spanish Census GeoJSON & TopoJSON map"
description: "How to compose a GeoJSON and TopoJSON map from dbf and shp file."
date:   2018-01-13 16:17:01 +0100
categories: ["Data", "Visualization"]
---
## Overview
Public administrations usually provides maps and public records in shapefile formats.

## Shapefile

The shapefile is a geospatial vector data format introduced in the earlies 90s . Actually is a collection of three files:
- `.shp: binary shapes (polygon...), the geometry itself.
- `.dbf`: data of shapes or records. In dBase format.
- `.shx`: shape index format for quicker indexing.

Other files like `.proj`, `.shp.xml`, `.sbn` ... may be include.

In this example we are going to use the Spanish [census section](http://en.eustat.eus/documentos/elem_3830/definicion.html) shapefile ([INE Cartografía digitalizada](http://www.ine.es/censos2011_datos/cen11_datos_resultados_seccen.htm)):

<div class="full">
    <a href="/assets/posts/{{page.slug}}/ine-spain.png">
    <img class="img-fluid" src="/assets/posts/{{page.slug}}/ine-spain.png">
    </a>
</div>

Despite useful for many purposes, shapefiles are not the best option for human reading or web visualization. The main problem is the size of the `.shp` files:

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

GeoJSON, based on JSON, allows us to store collections of geometric data types (including their non-spatial attributes) in one central structure.

A notable extension of GeoJSON is TopoJSON, that encodes geospatial topology and provides smaller file sizes (up to 80% reduction).

{% highlight console %}
  241037101 Feb 22 12:29 SECC_CPV_E_01_R_INE_geoJSON.json
   35648891 Feb 22 12:24 SECC_CPV_E_01_R_INE_topoJSon.json
   11220226 Mar 14  2014 SECC_CPV_E_01_R_INE.dbf
        404 Mar 14  2014 SECC_CPV_E_01_R_INE.prj
  109242236 Mar 14  2014 SECC_CPV_E_01_R_INE.shp
     287780 Mar 14  2014 SECC_CPV_E_01_R_INE.shx
{% endhighlight %}

How to convert shapefiles into json type of files?

#### Web browser
- [Mapsharer](http://mapshaper.org/): Best one. Shapefile, geojson, topojson, csv, svg & map representation
- [geojson-topojson](http://jeffpaine.github.io/geojson-topojson/): GeoJSON <-> TopoJSON
- [Distillery](http://shancarter.github.io/distillery/): GeoJSON -> TopoJSON & Map representation 

#### NodeJS & CL
[Mike Bostock](https://medium.com/@mbostock) wrote wonderful long post with neat examples:
https://medium.com/@mbostock/command-line-cartography-part-1-897aa8f8ca2c

#### Python
There are several gist solutions with different libraries:
- [json+ogr](https://gist.github.com/AlexArcPy/2fc9f41ca164f76fcbb30ebca273b59f )
- [Bulk convert shapefiles to geojson using ogr2ogr](https://gist.github.com/benbalter/5858851)
- [PyShp, shp to geojson in python](https://gist.github.com/frankrowe/6071443)
- [Fiona](https://gist.github.com/jwass/6245313)

