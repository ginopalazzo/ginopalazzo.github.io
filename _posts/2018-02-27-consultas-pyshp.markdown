---
layout: post
title:  "Pequeño snippet para hacer queries en registros pyshp"
lang: es
ref: pyshp-queries
description: "pyshp es la libreria de Python más usada en archivos shp. Vamos a intentar hacer queries con ella."
date:   2018-02-27 07:17:11 +0100
categories: ["Python", "Data"]
---
## Introducción
Si quieres leer o escribir un archivo shp en Python, probablemente vayas a usar [pyshp](https://github.com/GeospatialPython/pyshp) (o puede que [Fiona](https://pypi.python.org/pypi/Fiona)).

He intentado encontrar un complemento de Python para la libreria pyshp en [pypi](https://pypi.python.org/pypi?:action=browse&show=all&c=391) con el que pudiera realizae queries en la base de datos contenida en el archivo `.dbf`.

Dado la [sección censal](http://en.eustat.eus/documentos/elem_3830/definicion.html) española obtenemos el shapefile ([INE Cartografía digitalizada](http://www.ine.es/censos2011_datos/cen11_datos_resultados_seccen.htm)) y lo visualizamos con [Mapshaper](http://mapshaper.org/):

<div class="full">
    <a href="/assets/posts/{{page.ref}}/spain.png">
    <img class="img-fluid" src="/assets/posts/{{page.ref}}/spain.png">
    </a>
</div>

El objetivo es seleccionar algunas partes del mapa del shapefile y salvarlo como otra colección de archivos `.shp`, `.dbf`, `shx`. Por ejemplo: 
- select from Spanish census section:
    -  province: Madrid 
    -  autonomy: Castilla y León, Galicia

<div class="full">
    <a href="/assets/posts/{{page.ref}}/madrid-castilla-y-leon-galicia.png">
    <img class="img-fluid" src="/assets/posts/{{page.ref}}/madrid-castilla-y-leon-galicia.png">
    </a>
</div>

## Shapefile

Shapefile es un formato de datos vectoriales geoespaciales. Realmente, es una colección de tres archivos:
- `.shp: geometria binaria (poligonos...).
- `.dbf`: datos de las figuras o registros. En formato dBase.
- `.shx`: índice de figuras (no es obligatorio) para indexar más rápido.

También es posible que se incluyan otros archivos como `.proj`, `.shp.xml`, `.sbn` ...

## Query snippet
El archivo `.dbf` tiene los siguientes campos para cada figura:
{% highlight python %}
['OBJECTID', 'CUSEC', 'CUMUN', 'CSEC', 'CDIS', 'CMUN', 'CPRO', 'CCA', 'CUDIS', 'OBS', 'CNUT0', 'CNUT1', 'CNUT2', 'CNUT3', 'CLAU2', 'NPRO', 'NCA', 'NMUN', 'Shape_Leng', 'Shape_area', 'Shape_len']
{% endhighlight %}

Por ejemplo, `NPROV` es el nombre de la provincia, `NMUN` es el nombre del municipio y`CUMUN` es el código del municipio.

Queremos selección en la misma query múltiples campos con diversos valores. Por ejemplo, NPRO = Madrid, Sevilla y NMUN = Barcelona.

{% highlight python %}
import shapefile

class ShapeFileUtils(shapefile.Reader):
    ''' Inherit from shapefile.Reader '''
    
    def __init__(self,  *args, **kwargs):
        shapefile.Reader.__init__(self,  *args, **kwargs)
        # list of name fields
        self.name_fields = [field[0] for field in
                            list(self.fields[1:])]
    
    def query(self, **args):
        '''
        Makes a query on a shapefile.Reader object
        :param args: list of fields and values to query
        :return: the query shapefile.Writer object
        '''
        w = shapefile.Writer(shapeType=self.shapeType)
        w.fields = self.fields[1:]
        # shapefile 2.x version:
        # w.encoding = self.encoding
        for key, value in args.items():
            index = self.name_fields.index(key)
            for shaperec in self.iterShapeRecords():
                if shaperec.record[index] in value:
                    w.record(*shaperec.record)
                    # shapefile 2.x version should use:
                    # w.shape(shaperec.shape)
                    # shapefile 1.x version should use:
                    w._shapes.append(shaperec.shape)
        return w
{% endhighlight %}

Primero, cargamos el shapefile del [Instituto Nacional de Estadísitca]](http://www.ine.es/censos2011_datos/cen11_datos_resultados_seccen.htm) español en el objeto `ShapeFileItils`que extiende de la clase `shapefile.Reader`:

{% highlight python %}
sf = ShapeFileUtils("SECC_CPV_E_20111101_01_R_INE",
                     encoding="latin1")
{% endhighlight %}

Después, realizamos una query seleccionamos algunas provincias del Norte de España, la Comunidad Autónoma de Castilla y León y algunos de los municipios más poblados de Asturias, pero sin seleccionar la provincia:

{% highlight python %}
wq = sf.query(
         NPRO=['Coruña, A', 'Ourense', 'Lugo', 'Pontevedra', 'Cantabria'],
         NMUN=['Oviedo', 'Gijón', 'Avilés'],
         NCA=['Castilla y León']
    )
<shapefile.Writer object at 0x111278908>
wq.save('shapefiles/test/query_test')
{% endhighlight %}

El codigo anterior devolverá un objeto `shapefile.Writer` que podremos visualizar en [Mapshaper](http://mapshaper.org/):

<div class="full">
    <a href="/assets/posts/{{page.ref}}/galicia-castilla-y-leon-cantabria-asturias-municipios.png">
    <img class="img-fluid" src="/assets/posts/{{page.ref}}/galicia-castilla-y-leon-cantabria-asturias-municipios.png">
    </a>
</div>

También es posible realizar dos quieries para guardar dos shapefiles disintos y representarlos juntos. En este caso el municipio de Madrid y su provincia:

<div class="full">
    <a href="/assets/posts/{{page.ref}}/madrid-madrid.png">
    <img class="img-fluid" src="/assets/posts/{{page.ref}}/madrid-madrid.png">
    </a>
</div>

{% highlight python %}
wq2 = sf.query(NPRO=['Madrid'])
wq3 = sf.query(NMUN=['Madrid'])
{% endhighlight %}

Vista de la sección censal del Retiro, dentro del municipio y la provincia de Madrid:

<div class="full">
    <a href="/assets/posts/{{page.ref}}/madrid-madrid-retiro.png">
    <img class="img-fluid" src="/assets/posts/{{page.ref}}/madrid-madrid-retiro.png">
    </a>
</div>

## Próximos pasos

Resulta algo incómodo realizar las queris de esta manera. Estaría bien poder realizar consultas de manera simiar a cómo se hace con el ORM de Django. Por ejemplo:

 sf.objects.filter(
    NPRO='Barcelona'
).exclude(
    NMUN='Prat de Llobregat, El'
)...
