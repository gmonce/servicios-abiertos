Servicios RESTful de Ubicaciones 
================================

Intendencia de Montevideo
=========================

Recursos
--------

1. Calle

Métodos: GET

Descripción: Este recurso incluye el código y el nombre de una calle

Media-Type: application/json

Schema:
```
	{
	"title" : "calle",
	"type" : "object",
	"properties" : {
		{"nombre" : {"type": "string"}},
 		{"codigo" : {"type": "integer"}
 	}
	"required": ["nombre" , "codigo"]
 	"description" : "Vía de tránsito de Montevideo"
	}
```

Ejemplo:
```
{"nombre": "AV AGRACIADA" , "codigo" : 126} 
```


| Ubicación | GET | Punto en el mapa | application/geojson | {code} {
	"title": "ubicacion",
	"type": "object",
	"properties":{
		"geom" : {
			"type" : "object",
			"properties" : {
				"type" : {"type" : "string"},
				"coordinates" :  {"type" : "array" , "items" : { "type" : "number" }}
		}
	}
	"required": ["geom"]
 	"description": "Ubicación en Montevideo"
 } {code} | \{"geom":\{"type":"Point","coordinates":[573153.204,6140488.1186]\}\} |


h2. URIs


|| Recurso || URI || Descripción || Ejemplo (python) ||
| Calle | ubicacionesRestWEB/ubicacion/calles/?nombre=\{texto\} | Recibe en el token \{texto\} parte del nombre de una calle, y devuelve una lista de calles que incluyan el texto en su nombre | {code}
import json
import urllib
import urllib2

nombre_via='agra'
url_base='http://www.montevideo.gub.uy/ubicacionesRestProd/'

print url_base+'ubicacion/calles/?nombre='+nombre_via
r=urllib2.urlopen(url_base+'calles/?nombre='+nombre_via)
web_pg=r.read()
j=json.loads(web_pg)
for vias in j:
	print vias['codigo'],vias['nombre']{code} |
| Calle | ubicacionesRestWEB/ubicacion/cruces/\{codigo_calle\}/?nombre=\{texto\} | Recibe en el token \{codigo_calle\} un código de vía válido y en el token \{texto\} parte del nombre de otra vía y devuelve una lista de vías que incluyan el texto y crucen a la calle original. Si no se incluye el texto, devuelve todos los cruces. | {code}
import json
import urllib
import urllib2

codigo_via_origen=126
nombre_via='a'
url_base='http://www.montevideo.gub.uy/ubicacionesRestProd/'
url=url_base+'cruces/'+str(codigo_via_origen)+'/?nombre='+nombre_via
print url
r=urllib2.urlopen(url)
web_pg=r.read()
j=json.loads(web_pg)
for vias in j:
	print vias['codigo'],vias['nombre'] {code} |
| Ubicación | ubicacionesRestWEB/ubicacion/esquina/\{via1\}/\{via2} | Recibe en los tokens \{via1\} y \{via2\} dos códigos de vía válidos, y devuelve el punto del mapa con la ubicación | {code}import json
import urllib
import urllib2

codigo_via_1=126
codigo_via_2=636
url_base='http://www.montevideo.gub.uy/ubicacionesRestProd/'

r=urllib2.urlopen(url_base+'esquina/'+str(codigo_via_1)+'/'+str(codigo_via_2))
web_pg=r.read()
j=json.loads(web_pg)
print j['geom']['coordinates']
 {code} |
| Ubicación | ubicacionesRestWEB/ubicacion/direccion/\{via\}/\{puerta\} | Recibe en los tokens \{via\} y \{puerta\} el código de una vía y un número de puerta, y devuelve el punto en el mapa correspondiente | {code}
import json
import urllib
import urllib2

codigo_via=126
puerta=3509
url_base='http://www.montevideo.gub.uy/ubicacionesRestProd/'

r=urllib2.urlopen(url_base+'direccion/'+str(codigo_via)+'/'+str(puerta))
web_pg=r.read()
j=json.loads(web_pg)
print j['geom']['coordinates']
 {code} |
| Ubicación | ubicacionesRestWEB/ubicacion/padron/\{numero\} | Recibe el token numero correspondiente al numero de paron y devuelve el centroide del mismo\\ | |

| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |

The outer pipes (|) are optional, and you don't need to make the raw Markdown line up prettily. You can also use inline Markdown.

Markdown | Less | Pretty
--- | --- | ---
*Still* | `renders` | **nicely**
1 | 2 | 3
