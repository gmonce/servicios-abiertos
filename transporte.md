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

## URIs 

### Búsqueda de líneas por parada 
**Recurso**: Parada y líneas 

**URI**: 
```
transporteRest/lineas/{parada} 
```

**Descripción**: Recibe un código de parada de ómnibus, y devuelve la descripcion de la parada y la lista de líneas que pasan por ella. 


**Ejemplo** (Python): 
```
import json
import urllib
import urllib2

codigo_parada=3029
url_base='http://www.montevideo.gub.uy/transporteRest/'

r=urllib2.urlopen(url_base+'lineas/'+str(codigo_parada))
web_pg=r.read()
j=json.loads(web_pg)
print "Parada:",j['descripcion']
for linea in j['lineas']:
	print "Línea:",linea['codigo'],linea['descripcion']
```
### Horarios de pasada 

**Recurso**: Lista de pasadas 

**URI**: 
```
transporteRest/{parada}/{tipo_dia} 
transporteRest/pasadas/{parada}/{tipo_dia}/{hora} 
```

**Descripción**: Recibe un código de parada de ómnibus, y un tipo de día ("HABIL","SABADO","DOMINGO") y devuelve la lista de todas las pasadas en el día. Si además se especifica una hora, entonces solamente devuelve las siguientes diez pasadas luego de la hora especificada. 

**Ejemplo** (Python): 
```
import json
### Horarios de pasada 
import urllib
import urllib2

codigo_parada=3029
tipo_dia='HABIL'
url_base='http://www.montevideo.gub.uy/transporteRest/'

r=urllib2.urlopen(url_base+'pasadas/'+str(codigo_parada)+'/'+tipo_dia)
web_pg=r.read()
j=json.loads(web_pg)
for pasada in j:
	print "Línea", pasada['linea'], "Hora:",str(pasada['horaDesc']), "Destino:",pasada['destino']
```

### Horarios de pasada, por línea

**Recurso**: Lista de pasadas 

**URI**: 
```
transporteRest/pasadas/{parada\}/{tipo_dia}/{linea}
transporteRest/pasadas/{parada\}/{tipo_dia}/{linea}/{hora} 
```

**Descripción**: Recibe un código de parada de ómnibus, un tipo de día, y un código de línea de ómnibus, y devuelve la lista de todas las pasadas en el día, para esa línea. Si además se especifica una hora, entonces solamente devuelve las siguientes diez pasadas luego de la hora especificada, de la línea especificada.

**Ejemplo** (Python): 
```
import json
import urllib
import urllib2

codigo_parada=3029
tipo_dia='HABIL'
codigo_linea=3 #Código de la línea 405
hora='18:30'
url_base='http://www.montevideo.gub.uy/transporteRest/'

r=urllib2.urlopen(url_base+'pasadas/'+str(codigo_parada)+'/'+tipo_dia+'/'+str(codigo_linea)+'/'+hora)
web_pg=r.read()
j=json.loads(web_pg)
for pasada in j:
	print "Línea", pasada['linea'], "Hora:",str(pasada['horaDesc']), "Destino:",pasada['destino']
```


