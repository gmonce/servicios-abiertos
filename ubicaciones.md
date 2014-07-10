Servicios RESTful de Ubicaciones 
================================

*Intendencia de Montevideo*

## Recursos

### Calle

**Métodos**: GET

**Descripción**: Este recurso incluye el código y el nombre de una calle

**Media-Type**: application/json

**Schema**:
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

*Ejemplo*:
```
{"nombre": "AV AGRACIADA" , "codigo" : 126} 
```
### Ubicación

**Métodos**: GET

**Descripción**: Punto en el mapa 

**Media-Type**: application/geojson 

**Schema**:
```
{
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
 } 
```

**Ejemplo**:
```
{"geom":\{"type":"Point","coordinates":[573153.204,6140488.1186]}
```

## URIs

### Búsqueda de calle por nombre

**Recurso**: Calle

**URI**: 
```
ubicacionesRestWEB/ubicacion/calles/?nombre={texto} 
```

**Descripción**: Recibe en el token `texto` parte del nombre de una calle, y devuelve una lista de calles que incluyan el texto en su nombre 

**Ejemplo** (Python): 

```
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

```

### Búsqueda de vías de tránsito que cruzan a otra 

**Recurso**: Calle

**URI**: 
```
ubicacionesRestWEB/ubicacion/cruces/{codigo_calle}/?nombre={texto} 
```

**Descripción**: Recibe en el token `codigo_calle` un código de vía válido y en el token `texto` parte del nombre de otra vía y devuelve una lista de vías que incluyan el texto y crucen a la calle original. Si no se incluye el texto, devuelve todos los cruces. 

**Ejemplo** (Python): 
```
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
	print vias['codigo'],vias['nombre']

```

### Búsqueda de esquinas 

**Recurso**: Ubicación 

**URI**: 
```
ubicacionesRestWEB/ubicacion/esquina/{via1}/{via2} 
```

**Descripción**: Recibe en los tokens `via1` y `via2` dos códigos de vía válidos, y devuelve el punto del mapa con la ubicación 

**Ejemplo** (Python): 
```
import json
import urllib
import urllib2

codigo_via_1=126
codigo_via_2=636
url_base='http://www.montevideo.gub.uy/ubicacionesRestProd/'

r=urllib2.urlopen(url_base+'esquina/'+str(codigo_via_1)+'/'+str(codigo_via_2))
web_pg=r.read()
j=json.loads(web_pg)
print j['geom']['coordinates']
```

### Búsqueda de direcciones 

**Recurso**: Ubicación 

**URI**: 
```
ubicacionesRestWEB/ubicacion/direccion/{via}/{puerta} | 
```

**Descripción**: Recibe en los tokens `via` y `puerta` el código de una vía y un número de puerta, y devuelve el punto correspondiente en el mapa

**Ejemplo** (Python): 
```
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
```

### Búsqueda de padrón 

**Recurso**: Ubicación

**URI**: 
```
ubicacionesRestWEB/ubicacion/padron/{numero} 
```

**Descripción**: Recibe el token numero correspondiente al numero de padrón y devuelve su centroide


**Ejemplo** (Python): 

```
```
