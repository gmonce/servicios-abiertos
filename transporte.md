## Servicios Abiertos de Transporte
### Intendencia de Montevideo

## Recursos

### Línea 

**Métodos**: GET

**Descripción:*** Línea de ómnibus 

**Media-Type**: application/json 

**Schema**:
```
	"title" : "linea",
	"type" : "object",
	"properties" : {
		{"codigo" : {"type": "integer"}},
 		{"descripcion" : {"type": "string"}
 	}
	"required": ["codigo" , "descripcion"]
 	"description" : "Línea de ómnibus" }

```
**Ejemplo**:
```
{"codigo":442,"descripcion":"2"}

```

## Parada y Líneas 

**Métodos**: GET

**Descripción**: Parada de ómnibus con la lista de líneas que pasar por ella 

**Media-Type**: application/json 


**Schema**:
```
	"title" : "parada_y_linea",
	"type" : "object",
	"properties" : {
		{"descripcion" : {"type": "string"}},
 		{"lineas" : {"type": "object"}
 	}
	"required": ["descripcion" , "lineas"]
 	"description" : "Parada de ómnibus y líneas que por ella pasan"
 } 
```

**Ejemplo**:
```
{"descripcion":"JOSE ANTONIO CABRERA y GDOR VIANA",
"lineas":[{"codigo":442,"descripcion":"2"},{"codigo":57,"descripcion":"141"},
{"codigo":61,"descripcion":"143"},
{"codigo":62,"descripcion":"144"},{"codigo":447,"descripcion":"174"},
{"codigo":3,"descripcion":"405"},{"codigo":19,"descripcion":"538"}
```

## Pasada 

**Métodos**: GET

**Descripción**: Pasada de un ómnibus por una parada 

**Media-Type**: application/json 

**Schema**:
```
{
	"title" : "pasada",
	"type" : "object",
	"properties" : {
		{"hora" : {"type": "integer"}},
 		{"destino" : {"type": "string"}},
 		{"linea": {"type":"string"}}
 		{"dia_anterior":{"type":"string"}}
 		{"horaDesc":{"type":"string"}}
 	}
	"required": ["hora" , "destino", "linea"]
 	"description" : "Pasada de un ómnibus por una parada"
}
```

**Ejemplo**:
```
{"hora":1656,"destino":"8 DE OCTUBRE Y COMERCIO","linea":"143","dia_anterior":"N","horaDesc":"16:56"}
```

h2. Servicios


|| Recurso || URI || Descripción || Ejemplo (python) ||
| Parada y líneas | transporteRestProd/lineas/\{parada\} | Recibe un código de parada de ómnibus, y devuelve la descripcion de la parada y la lista de líneas que pasan por ella. | {code}import json
import urllib
import urllib2

codigo_parada=3029
url_base='http://www.montevideo.gub.uy/transporteRestProd/'

r=urllib2.urlopen(url_base+'lineas/'+str(codigo_parada))
web_pg=r.read()
j=json.loads(web_pg)
print "Parada:",j['descripcion']for linea in j['lineas']:
	print "Línea:",linea['codigo'],linea['descripcion']
{code} |
| Lista de pasadas \\ | transporteRestProd/\{parada\}/\{tipo_dia\}&nbsp; \\
\\
transporteRestProd/pasadas/\{parada\}/\{tipo_dia\}/\{hora\} \\ | Recibe un código de parada de ómnibus, y un tipo de día ("HABIL","SABADO","DOMINGO") y devuelve la lista de todas las pasadas en el día. Si además se especifica una hora, entonces solamente devuelve las siguientes diez pasadas luego de la hora especificada. \\ | {code}
import json
import urllib
import urllib2

codigo_parada=3029
tipo_dia='HABIL'
url_base='http://www.montevideo.gub.uy/transporteRestProd/'

r=urllib2.urlopen(url_base+'pasadas/'+str(codigo_parada)+'/'+tipo_dia)
web_pg=r.read()
j=json.loads(web_pg)
for pasada in j:
	print "Línea", pasada['linea'], "Hora:",str(pasada['horaDesc']), "Destino:",pasada['destino']{code} |
| Lista de pasadas \\ | transporteRestProd/pasadas/\{parada\}/\{tipo_dia\}/\{linea\}&nbsp; \\
\\
transporteRestProd/pasadas/\{parada\}/\{tipo_dia\}/{linea\}/\{hora\} \\ | Recibe un código de parada de ómnibus, un tipo de día, y un código de línea de ómnibus, y devuelve la lista de todas las pasadas en el día, para esa línea. Si además se especifica una hora, entonces solamente devuelve las siguientes diez pasadas luego de la hora especificada, de la línea especificada. \\ | {code}
import json
import urllib
import urllib2

codigo_parada=3029
tipo_dia='HABIL'
codigo_linea=3 #Código de la línea 405
hora='18:30'
url_base='http://www.montevideo.gub.uy/transporteRestProd/'

r=urllib2.urlopen(url_base+'pasadas/'+str(codigo_parada)+'/'+tipo_dia+'/'+str(codigo_linea)+'/'+hora)
web_pg=r.read()
j=json.loads(web_pg)
for pasada in j:
	print "Línea", pasada['linea'], "Hora:",str(pasada['horaDesc']), "Destino:",pasada['destino']{code} |


h4.




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
ubicacionesRest/calles/?nombre={texto} 
```

**Descripción**: Recibe en el token `texto` parte del nombre de una calle, y devuelve una lista de calles que incluyan el texto en su nombre 

**Ejemplo** (Python): 

```
import json
import urllib
import urllib2

nombre_via='agra'
url_base='http://www.montevideo.gub.uy/ubicacionesRest/'

print url_base+'calles/?nombre='+nombre_via
r=urllib2.urlopen(url_base+'calles/?nombre='+nombre_via)
web_pg=r.read()
j=json.loads(web_pg)
for vias in j:
	print vias['codigo'],vias['nombre']

```

### Búsqueda de vías de tránsito que cruzan a otra 

**Recurso**: Calle

**URI**: 
```
ubicacionesRest/cruces/{codigo_calle}/?nombre={texto} 
```

**Descripción**: Recibe en el token `codigo_calle` un código de vía válido y en el token `texto` parte del nombre de otra vía y devuelve una lista de vías que incluyan el texto y crucen a la calle original. Si no se incluye el texto, devuelve todos los cruces. 

**Ejemplo** (Python): 
```
import json
import urllib
import urllib2

codigo_via_origen=126
nombre_via='a'
url_base='http://www.montevideo.gub.uy/ubicacionesRest/'
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
ubicacionesRest/esquina/{via1}/{via2} 
```

**Descripción**: Recibe en los tokens `via1` y `via2` dos códigos de vía válidos, y devuelve el punto del mapa con la ubicación 

**Ejemplo** (Python): 
```
import json
import urllib
import urllib2

codigo_via_1=126
codigo_via_2=636
url_base='http://www.montevideo.gub.uy/ubicacionesRest/'

r=urllib2.urlopen(url_base+'esquina/'+str(codigo_via_1)+'/'+str(codigo_via_2))
web_pg=r.read()
j=json.loads(web_pg)
print j['geom']['coordinates']
```

### Búsqueda de direcciones 

**Recurso**: Ubicación 

**URI**: 
```
ubicacionesRest/direccion/{via}/{puerta}  
```

**Descripción**: Recibe en los tokens `via` y `puerta` el código de una vía y un número de puerta, y devuelve el punto correspondiente en el mapa

**Ejemplo** (Python): 
```
import json
import urllib
import urllib2

codigo_via=126
puerta=3509
url_base='http://www.montevideo.gub.uy/ubicacionesRest/'

r=urllib2.urlopen(url_base+'direccion/'+str(codigo_via)+'/'+str(puerta))
web_pg=r.read()
j=json.loads(web_pg)
print j['geom']['coordinates']
```

### Búsqueda de padrón 

**Recurso**: Ubicación

**URI**: 
```
ubicacionesRest/padron/{numero} 
```

**Descripción**: Recibe el token numero correspondiente al numero de padrón y devuelve su centroide


**Ejemplo** (Python): 

```
import json
import urllib
import urllib2

padron=1
url_base='http://www.montevideo.gub.uy/ubicacionesRest/'

r=urllib2.urlopen(url_base+'padron/'+str(padron))
web_pg=r.read()
j=json.loads(web_pg)
print j['geom']['coordinates']
```
