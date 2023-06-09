---
title: API Reference for SOCE indicators

language_tabs: # must be one of https://github.com/rouge-ruby/rouge/wiki/List-of-supported-languages-and-lexers  
  - python
  - javascript

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true

code_clipboard: true

meta:
  - name: description
    content: Documentation for SOCE indicators API
---

# Introduction

Welcome to the SOCE indicators SIE and GEN API! You can use our API to access SOCE SIE and GEN API endpoints, which can get information from indicators composed of SIE and GEN. The returned data is used to make some charts on the indicators website

We have language bindings in Python and JavaScript! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.


# Authentication


You do not need to authenticate in order to explore the SIE and GEN data.

<aside class="notice">
Debe reeemplazar el valor <code>serverapiSoceIP</code> con la dirección IP o nombre del servidor que USFQ publique.
</aside>

# SIE

## Get SIE Procesos Diariamente

```python
import http.client
import json

conn = http.client.HTTPSConnection("serverapiSoceIP", 5000)
payload = json.dumps([
  {
    "dateRange": {
      "dateRangeStart": {
        "pickedYear": "2008",
        "pickedMonth": "3",
        "pickedDay": "24"
      },
      "dateRangeEnd": {
        "pickedYear": "2008",
        "pickedMonth": "5",
        "pickedDay": "14"
      }
    },
    "monto": "true",
    "processStage": "adjudicada",
    "purchaseType": [
      "bien",
      "bien_y_servicio",
      "servicio",
      "farmacos"
    ],
    "timeScale": "diario",
    "typeGraph": 1
  }
])
headers = {
  'Content-Type': 'application/json'
}
conn.request("GET", "/api/soce_sie_ic_proceso/[   {     \"dateRange\": {       \"dateRangeStart\": {         \"pickedYear\": \"2008\",         \"pickedMonth\": \"3\",         \"pickedDay\": \"24\"       },       \"dateRangeEnd\": {         \"pickedYear\": \"2008\",         \"pickedMonth\": \"5\",         \"pickedDay\": \"14\"       }     },     \"monto\": \"true\",     \"processStage\": \"adjudicada\",     \"purchaseType\": [       \"bien\",       \"bien_y_servicio\",       \"servicio\",       \"farmacos\"     ],     \"timeScale\": \"diario\",     \"typeGraph\": 1   } ]", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))

```
```javascript
var myHeaders = new Headers();
myHeaders.append("Content-Type", "application/json");

var raw = JSON.stringify([
  {
    "dateRange": {
      "dateRangeStart": {
        "pickedYear": "2008",
        "pickedMonth": "3",
        "pickedDay": "24"
      },
      "dateRangeEnd": {
        "pickedYear": "2008",
        "pickedMonth": "5",
        "pickedDay": "14"
      }
    },
    "monto": "true",
    "processStage": "adjudicada",
    "purchaseType": [
      "bien",
      "bien_y_servicio",
      "servicio",
      "farmacos"
    ],
    "timeScale": "diario",
    "typeGraph": 1
  }
]);

var requestOptions = {
  method: 'GET',
  headers: myHeaders,
  body: raw,
  redirect: 'follow'
};

fetch("http://serverapiSoceIP/api/soce_sie_ic_proceso/[   {     \"dateRange\": {       \"dateRangeStart\": {         \"pickedYear\": \"2008\",         \"pickedMonth\": \"3\",         \"pickedDay\": \"24\"       },       \"dateRangeEnd\": {         \"pickedYear\": \"2008\",         \"pickedMonth\": \"5\",         \"pickedDay\": \"14\"       }     },     \"monto\": \"true\",     \"processStage\": \"adjudicada\",     \"purchaseType\": [       \"bien\",       \"bien_y_servicio\",       \"servicio\",       \"farmacos\"     ],     \"timeScale\": \"diario\",     \"typeGraph\": 1   } ]", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```

Este endpoint entrega procesos SIE, en los cuales se calcularon los indicadores individuales, obteniéndose una calificación promedio que sirve para medir el riesgo en el proceso. Los datos están agrupados diariamente, por lo tanto, la información se consolida a ese nivel de agrupación.

### HTTP Request

`GET http://serverapiSoceIP/api/soce_sie_ic_proceso/{{process_list}}`

### Definición de parámetros

Element | Description
--------- | -----------
process_list | Parámetro en formato JSON.

### Estructura del parámetro JSON

Element | Description
------- | -----------
dateRage | Elemento json para definir un rango de fechas
dateRangeStart | Elemento json para definir la fecha inicial
dateRangeEnd | Elemento json para definir la fecha final
pickedYear | Elemento para definir el año inicial o final
pickedMonth | Elemento para definir el mes inicial o final (el mes debe estar entre 0 y 11, siendo 0 Enero, y 11 Diciembre)
pickedDay | Elemento para definir el día inicial o final
monto | Elemento que puede ser TRUE o FALSE, en el caso de estar marcado se muestra información acerca del monto presupuestado del proceso
processStage | Elemento que contiene el estado del proceso. Para SIE se consideran 21 estados
purchaseType | Elemento array que contiene el tipo de compra que puede contener estos valores: bien, bien_y_servicio, servicio, farmacos
timeScale | Elemento que indica al endpoint la frecuencia en la que se debe recuperar los datos, en este caso, debe tener el valor "diario"
typeGraph | Elemnto que define la forma en la que se recuperan los datos, para este caso, debe tener el valor "1"


>Example JSON parameter:

```json
[
  {
    "dateRange": {
      "dateRangeStart": {
        "pickedYear": "2008",
        "pickedMonth": "3",
        "pickedDay": "24"
      },
      "dateRangeEnd": {
        "pickedYear": "2008",
        "pickedMonth": "5",
        "pickedDay": "14"
      }
    },
    "monto": "true",
    "processStage": "adjudicada",
    "purchaseType": [
      "bien",
      "bien_y_servicio",
      "servicio",
      "farmacos"
    ],
    "timeScale": "diario",
    "typeGraph": 1
  }
]
```

### Estructura de la respuesta JSON

Element | Description
------- | -----------
fecha_publicacion | Elemento que muestra la fecha de publicación
avg_indicadores | Elemento que muestra la calificación para esa fecha, este es el valor del índice compuesto que se calcula sacando el promedio por cada proceso y sus respectivos indicadores individuales, este valor está entre 0 y 100%
quartiles | Elemento que indica el valor del cuartial relacionado a la calificación obtenida en el rango de fechas
riesgo | Elemento que indica el riesgo para la fecha de publicación en la que se agrupan los resultados
std_monto_presupuestado | Elemento que muestra la desviación estándar sobre el monto presupuestado de los procesos considerados en el rango de fechas
std_monto_adjudicacion | Elemento que muestra la desviación estándar sobre el monto adjudicado de los procesos considerados en el rango de fechas
std_monto_contrato | Elemento que muestra la desviación estándar sobre el monto de contrato de los procesos considerados en el rango de fechas


>Example Response:

```json
[
  {
    "avg_indicadores": 67,
    "fecha_publicacion": "2008-06-05",
    "quartiles": 2,
    "riesgo": "Medio",
    "std_monto_adjudicacion": 0,
    "std_monto_contrato": 0,
    "std_monto_presupuestado": 0
  },
  {
    "avg_indicadores": 75,
    "fecha_publicacion": "2008-06-09",
    "quartiles": 3,
    "riesgo": "Bajo",
    "std_monto_adjudicacion": 0,
    "std_monto_contrato": 0,
    "std_monto_presupuestado": 0
  },
  {
    "avg_indicadores": 60,
    "fecha_publicacion": "2008-06-10",
    "quartiles": 2,
    "riesgo": "Medio",
    "std_monto_adjudicacion": 0,
    "std_monto_contrato": 0,
    "std_monto_presupuestado": 0
  }
]

```



## Get SIE Procesos Diariamente Detalle

```python
import http.client
import json

conn = http.client.HTTPSConnection("serverapiSoceIP", 5000)
payload = json.dumps([
  {
    "dateRange": {
      "dateRangeStart": {
        "pickedYear": "2008",
        "pickedMonth": "3",
        "pickedDay": "24"
      },
      "dateRangeEnd": {
        "pickedYear": "2008",
        "pickedMonth": "5",
        "pickedDay": "14"
      }
    },
    "monto": "true",
    "processStage": "adjudicada",
    "purchaseType": [
      "bien",
      "bien_y_servicio",
      "servicio",
      "farmacos"
    ],
    "timeScale": "diario",
    "typeGraph": 1
  }
])
headers = {
  'Content-Type': 'application/json'
}
conn.request("GET", "/api/soce_sie_ic_proceso_detail/[   {     \"dateRange\": {       \"dateRangeStart\": {         \"pickedYear\": \"2008\",         \"pickedMonth\": \"3\",         \"pickedDay\": \"24\"       },       \"dateRangeEnd\": {         \"pickedYear\": \"2008\",         \"pickedMonth\": \"5\",         \"pickedDay\": \"14\"       }     },     \"monto\": \"true\",     \"processStage\": \"adjudicada\",     \"purchaseType\": [       \"bien\",       \"bien_y_servicio\",       \"servicio\",       \"farmacos\"     ],     \"timeScale\": \"diario\",     \"typeGraph\": 1   } ]", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))
```

```javascript
var myHeaders = new Headers();
myHeaders.append("Content-Type", "application/json");

var raw = JSON.stringify([
  {
    "dateRange": {
      "dateRangeStart": {
        "pickedYear": "2008",
        "pickedMonth": "3",
        "pickedDay": "24"
      },
      "dateRangeEnd": {
        "pickedYear": "2008",
        "pickedMonth": "5",
        "pickedDay": "14"
      }
    },
    "monto": "true",
    "processStage": "adjudicada",
    "purchaseType": [
      "bien",
      "bien_y_servicio",
      "servicio",
      "farmacos"
    ],
    "timeScale": "diario",
    "typeGraph": 1
  }
]);

var requestOptions = {
  method: 'GET',
  headers: myHeaders,
  body: raw,
  redirect: 'follow'
};

fetch("http://serverapiSoceIP:5000/api/soce_sie_ic_proceso_detail/[   {     \"dateRange\": {       \"dateRangeStart\": {         \"pickedYear\": \"2008\",         \"pickedMonth\": \"3\",         \"pickedDay\": \"24\"       },       \"dateRangeEnd\": {         \"pickedYear\": \"2008\",         \"pickedMonth\": \"5\",         \"pickedDay\": \"14\"       }     },     \"monto\": \"true\",     \"processStage\": \"adjudicada\",     \"purchaseType\": [       \"bien\",       \"bien_y_servicio\",       \"servicio\",       \"farmacos\"     ],     \"timeScale\": \"diario\",     \"typeGraph\": 1   } ]", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```


Este endpoint entrega procesos SIE con información detallada, la misma que puede ser usada para calcular la calificación promedio del proceso, basada en la calificación de cada indicador individual.

### HTTP Request

`GET http://serverapiSoceIP/api/soce_sie_ic_proceso_detail/{{process_list}}`

Element | Description
--------- | -----------
process_list | Parámetro en formato JSON.

### Estructura del parámetro JSON

Element | Description
------- | -----------
dateRage | Elemento json para definir un rango de fechas
dateRangeStart | Elemento json para definir la fecha inicial
dateRangeEnd | Elemento json para definir la fecha final
pickedYear | Elemento para definir el año inicial o final
pickedMonth | Elemento para definir el mes inicial o final (el mes debe estar entre 0 y 11, siendo 0 Enero, y 11 Diciembre)
pickedDay | Elemento para definir el día inicial o final
monto | Elemento que puede ser TRUE o FALSE, en el caso de estar marcado se muestra información acerca del monto presupuestado del proceso
processStage | Elemento que contiene el estado del proceso. Para SIE se consideran 21 estados
purchaseType | Elemento array que contiene el tipo de compra que puede contener estos valores: bien, bien_y_servicio, servicio, farmacos
timeScale | Elemento que indica al endpoint la frecuencia en la que se debe recuperar los datos, en este caso, debe tener el valor "diario"
typeGraph | Elemnto que define la forma en la que se recuperan los datos, para este caso, debe tener el valor "1"


>Example JSON parameter:

```json
[
  {
    "dateRange": {
      "dateRangeStart": {
        "pickedYear": "2008",
        "pickedMonth": "3",
        "pickedDay": "24"
      },
      "dateRangeEnd": {
        "pickedYear": "2008",
        "pickedMonth": "5",
        "pickedDay": "14"
      }
    },
    "monto": "true",
    "processStage": "adjudicada",
    "purchaseType": [
      "bien",
      "bien_y_servicio",
      "servicio",
      "farmacos"
    ],
    "timeScale": "diario",
    "typeGraph": 1
  }
]
```

### Estructura de la respuesta JSON

Element | Description
------- | -----------
fecha_publicacion | Elemento que muestra la fecha de publicación
codigo | Elemento que muestra el código del Proceso
indicador_## | Elemento que muestra el valor de calificación del proceso en cada indicador. Se tienen indicadores desde el 01 hasta el 27, el valor 1 significafa que le proceso cumple con la ficha metodológica de cálculo, es decir es un proceso "bueno", mientras que el valor de 0 indica que ese proceso no cumplió la ficha, por lo tanto su calificación es "mala". Para el caso de valores NULOS o null, significa que el proceso no participó en el análisis o cálculo para un particular indicador.
monto_presupuestado | Elemento que muestra el monto presupuestado de los procesos considerados en el rango de fechas
monto_adjudicacion | Elemento que muestra el monto adjudicado de los procesos considerados en el rango de fechas
monto_contrato | Elemento que muestra el monto de contrato de los procesos considerados en el rango de fechas

>Example Response:

```json
[
  {
    "codigo": "RC-202348",
    "fecha_publicacion": "2008-06-05",
    "indicador_01": null,
    "indicador_04": null,
    "indicador_05": 1,
    "indicador_06": null,
    "indicador_09": null,
    "indicador_11": null,
    "indicador_15": 0,
    "indicador_17": 1,
    "indicador_19": null,
    "indicador_22": null,
    "indicador_25": null,
    "indicador_26": null,
    "indicador_27": null,
    "monto_adjudicacion": 30300,
    "monto_contrato": 0,
    "monto_presupuestado": 45000
  },
  {
    "codigo": "RC-202370",
    "fecha_publicacion": "2008-06-09",
    "indicador_01": null,
    "indicador_04": 1,
    "indicador_05": 1,
    "indicador_06": null,
    "indicador_09": null,
    "indicador_11": null,
    "indicador_15": 0,
    "indicador_17": 1,
    "indicador_19": null,
    "indicador_22": null,
    "indicador_25": null,
    "indicador_26": null,
    "indicador_27": null,
    "monto_adjudicacion": 0,
    "monto_contrato": 0,
    "monto_presupuestado": 31802.4
  },
  {
    "codigo": "UTCCRS-0418",
    "fecha_publicacion": "2008-06-10",
    "indicador_01": 1,
    "indicador_04": 0,
    "indicador_05": 1,
    "indicador_06": null,
    "indicador_09": null,
    "indicador_11": null,
    "indicador_15": 0,
    "indicador_17": 1,
    "indicador_19": null,
    "indicador_22": null,
    "indicador_25": null,
    "indicador_26": null,
    "indicador_27": null,
    "monto_adjudicacion": 11600,
    "monto_contrato": 0,
    "monto_presupuestado": 14500
  }
]
```


## Get SIE Procesos Mensual

```python
import http.client

conn = http.client.HTTPSConnection("serverapiSoceIP", 5000)
payload = ''
headers = {}
conn.request("GET", "/api/soce_sie_ic_proceso/[{
  \"dateRange\":{
    \"dateRangeStart\":{
      \"pickedYear\": \"2008\",
      \"pickedMonth\": \"5\",
      \"pickedDay\": \"25\"
    },
    \"dateRangeEnd\": {
      \"pickedYear\": \"2009\",
      \"pickedMonth\": \"1\",
      \"pickedDay\": \"20\"
    }
  },
  \"monto\": \"true\",
  \"processStage\": \"adjudicada\",
  \"purchaseType\": [\"bien\", \"bien_y_servicio\", \"servicio\", \"farmacos\"],
  \"timeScale\": \"mensual\", \"typeGraph\": 1
}]", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))
```

```javascript
var requestOptions = {
  method: 'GET',
  redirect: 'follow'
};

fetch("http://serverapiSoceIP:5000/api/soce_sie_ic_proceso/[{\n  \"dateRange\":{\n    \"dateRangeStart\":{\n      \"pickedYear\": \"2008\",\n      \"pickedMonth\": \"5\",\n      \"pickedDay\": \"25\"\n    },\n    \"dateRangeEnd\": {\n      \"pickedYear\": \"2009\",\n      \"pickedMonth\": \"1\",\n      \"pickedDay\": \"20\"\n    }\n  },\n  \"monto\": \"true\",\n  \"processStage\": \"adjudicada\",\n  \"purchaseType\": [\"bien\", \"bien_y_servicio\", \"servicio\", \"farmacos\"],\n  \"timeScale\": \"mensual\", \"typeGraph\": 1\n}]", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```


Este endpoint devuelve información sobre procesos SIE, en los cuales se calcularon los indicadores individuales, obteniéndose una calificación promedio que sirve para medir el riesgo en el proceso. Los datos están agrupados mensualmente, por lo tanto, la información se consolida a ese nivel de agrupación.


### HTTP Request

`GET http://serverapiSoceIP/api/soce_sie_ic_proceso/{{process_list}}`

### Definición de parámetros

Element | Description
--------- | -----------
process_list | Parámetro en formato JSON.


### Estructura del parámetro JSON

Element | Description
------- | -----------
dateRage | Elemento json para definir un rango de fechas
dateRangeStart | Elemento json para definir la fecha inicial
dateRangeEnd | Elemento json para definir la fecha final
pickedYear | Elemento para definir el año inicial o final
pickedMonth | Elemento para definir el mes inicial o final (el mes debe estar entre 0 y 11, siendo 0 Enero, y 11 Diciembre)
pickedDay | Elemento para definir el día inicial o final
monto | Elemento que puede ser TRUE o FALSE, en el caso de estar marcado se muestra información acerca del monto presupuestado del proceso
processStage | Elemento que contiene el estado del proceso. Para SIE se consideran 21 estados
purchaseType | Elemento array que contiene el tipo de compra que puede contener estos valores: bien, bien_y_servicio, servicio, farmacos
timeScale | Elemento que indica al endpoint la frecuencia en la que se debe recuperar los datos, en este caso, debe tener el valor "mensual"
typeGraph | Elemnto que define la forma en la que se recuperan los datos, para este caso, debe tener el valor "1"

>Example JSON parameter:

```json
[{
  "dateRange":{
    "dateRangeStart":{
      "pickedYear": "2008",
      "pickedMonth": "5",
      "pickedDay": "25"
    },
    "dateRangeEnd": {
      "pickedYear": "2009",
      "pickedMonth": "1",
      "pickedDay": "20"
    }
  },
  "monto": "true",
  "processStage": "adjudicada",
  "purchaseType": ["bien", "bien_y_servicio", "servicio", "farmacos"],
  "timeScale": "mensual", 
  "typeGraph": 1
}]
```

### Estructura de la respuesta JSON

Element | Description
------- | -----------
anio | Elemento que muestra el año de la fecha de publicación de un proceso
mes | Elemento que muestra el mes de la fecha de publicación de un proceso
avg_indicadores | Elemento que muestra la calificación para ese mes, este es el valor del índice compuesto que se calcula sacando el promedio por cada proceso y sus respectivos indicadores individuales, este valor está entre 0 y 100%
quartiles | Elemento que indica el valor del cuartial relacionado a la calificación obtenida en el rango de fechas
riesgo | Elemento que indica el riesgo para mes  en la que se agrupan los resultados
std_monto_presupuestado | Elemento que muestra el monto presupuestado de los procesos considerados en el rango de fechas
std_monto_adjudicacion | Elemento que muestra el monto adjudicado de los procesos considerados en el rango de fechas
std_monto_contrato | Elemento que muestra el monto de contrato de los procesos considerados en el rango de fechas


>Example Response:

```json
[
  {
    "anio": 2008,
    "avg_indicadores": 61.43,
    "mes": 6,
    "quartiles": 2,
    "riesgo": "Medio",
    "std_monto_adjudicacion": 11488.81,
    "std_monto_contrato": 0,
    "std_monto_presupuestado": 3611430.77
  },
  {
    "anio": 2008,
    "avg_indicadores": 74.3,
    "mes": 7,
    "quartiles": 2,
    "riesgo": "Medio",
    "std_monto_adjudicacion": 51532.97,
    "std_monto_contrato": 0,
    "std_monto_presupuestado": 50616
  },    
  {
    "anio": 2008,
    "avg_indicadores": 75.46,
    "mes": 11,
    "quartiles": 3,
    "riesgo": "Bajo",
    "std_monto_adjudicacion": 158689.38,
    "std_monto_contrato": 0,
    "std_monto_presupuestado": 163574.61
  },
  {
    "anio": 2008,
    "avg_indicadores": 75.13,
    "mes": 12,
    "quartiles": 3,
    "riesgo": "Bajo",
    "std_monto_adjudicacion": 222129.84,
    "std_monto_contrato": 0,
    "std_monto_presupuestado": 231053.43
  }
]
```

## Get SIE Procesos Mensual Detalle

```python
import http.client

conn = http.client.HTTPSConnection("serverapiSoceIP", 5000)
payload = ''
headers = {}
conn.request("GET", "/api/soce_sie_ic_proceso_detail/[{
  \"dateRange\":{
    \"dateRangeStart\":{
      \"pickedYear\": \"2008\",
      \"pickedMonth\": \"5\",
      \"pickedDay\": \"25\"
    },
    \"dateRangeEnd\": {
      \"pickedYear\": \"2020\",
      \"pickedMonth\": \"10\",
      \"pickedDay\": \"20\"
    }
  },
  \"monto\": \"true\",
  \"processStage\": \"adjudicada\",
  \"purchaseType\": [\"bien\", \"bien_y_servicio\", \"servicio\", \"farmacos\"],
  \"timeScale\": \"mensual\", \"typeGraph\": 1
}]", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))
```

```javascript
var requestOptions = {
  method: 'GET',
  redirect: 'follow'
};

fetch("http://serverapiSoceIP:5000/api/soce_sie_ic_proceso_detail/[{\n  \"dateRange\":{\n    \"dateRangeStart\":{\n      \"pickedYear\": \"2008\",\n      \"pickedMonth\": \"5\",\n      \"pickedDay\": \"25\"\n    },\n    \"dateRangeEnd\": {\n      \"pickedYear\": \"2020\",\n      \"pickedMonth\": \"10\",\n      \"pickedDay\": \"20\"\n    }\n  },\n  \"monto\": \"true\",\n  \"processStage\": \"adjudicada\",\n  \"purchaseType\": [\"bien\", \"bien_y_servicio\", \"servicio\", \"farmacos\"],\n  \"timeScale\": \"mensual\", \"typeGraph\": 1\n}]", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```

Este endpoint devuelve información detallada sobre procesos SIE, la misma que puede ser usada para calcular la calificación promedio de los procesos considereados en frecuencia mensual.

### HTTP Request

`GET http://serverapiSoceIP/api/soce_sie_ic_proceso_detail/{{process_list}}`

### Definición de parámetros

Element | Description
--------- | -----------
process_list | Parámetro en formato JSON.

### Estructura del parámetro JSON

Element | Description
------- | -----------
dateRage | Elemento json para definir un rango de fechas
dateRangeStart | Elemento json para definir la fecha inicial
dateRangeEnd | Elemento json para definir la fecha final
pickedYear | Elemento para definir el año inicial o final
pickedMonth | Elemento para definir el mes inicial o final (el mes debe estar entre 0 y 11, siendo 0 Enero, y 11 Diciembre)
pickedDay | Elemento para definir el día inicial o final
monto | Elemento que puede ser TRUE o FALSE, en el caso de estar marcado se muestra información acerca del monto presupuestado del proceso
processStage | Elemento que contiene el estado del proceso. Para SIE se consideran 21 estados
purchaseType | Elemento array que contiene el tipo de compra que puede contener estos valores: bien, bien_y_servicio, servicio, farmacos
timeScale | Elemento que indica al endpoint la frecuencia en la que se debe recuperar los datos, en este caso, debe tener el valor "mensual"
typeGraph | Elemnto que define la forma en la que se recuperan los datos, para este caso, debe tener el valor "1"


>Example JSON parameter:

```json
[{
  "dateRange":{
    "dateRangeStart":{
      "pickedYear": "2008",
      "pickedMonth": "5",
      "pickedDay": "25"
    },
    "dateRangeEnd": {
      "pickedYear": "2008",
      "pickedMonth": "10",
      "pickedDay": "20"
    }
  },
  "monto": "true",
  "processStage": "adjudicada",
  "purchaseType": ["bien", "bien_y_servicio", "servicio", "farmacos"],
  "timeScale": "mensual", 
  "typeGraph": 1
}]
```

### Estructura de la respuesta JSON

Element | Description
------- | -----------
fecha_publicacion | Elemento que muestra la fecha de publicación, para este caso, el mes es el objetivo del análisis
codigo | Elemento que muestra el código del Proceso 
indicador_## | Elemento que muestra el valor de calificación del proceso en cada indicador. Se tienen indicadores desde el 01 hasta el 27, el valor 1 significafa que le proceso cumple con la ficha metodológica de cálculo, es decir es un proceso "bueno", mientras que el valor de 0 indica que ese proceso no cumplió la ficha, por lo tanto su calificación es "mala". Para el caso de valores NULOS o null, significa que el proceso no participó en el análisis o cálculo para un particular indicador.
monto_presupuestado | Elemento que muestra el monto presupuestado de los procesos considerados en el rango de fechas
monto_adjudicacion | Elemento que muestra el monto adjudicado de los procesos considerados en el rango de fechas
monto_contrato | Elemento que muestra el monto de contrato de los procesos considerados en el rango de fechas


>Example Response:

```json
[
  {
    "codigo": "TIZA-AG",
    "fecha_publicacion": "2008-11-24",
    "indicador_01": 1,
    "indicador_04": 1,
    "indicador_05": 1,
    "indicador_06": null,
    "indicador_09": null,
    "indicador_11": null,
    "indicador_15": 0,
    "indicador_17": 1,
    "indicador_19": null,
    "indicador_22": null,
    "indicador_25": null,
    "indicador_26": null,
    "indicador_27": null,
    "monto_adjudicacion": 62,
    "monto_contrato": 0,
    "monto_presupuestado": 82
  },
  {
    "codigo": "MG-2008-AG-320",
    "fecha_publicacion": "2008-11-19",
    "indicador_01": 1,
    "indicador_04": 1,
    "indicador_05": 1,
    "indicador_06": null,
    "indicador_09": null,
    "indicador_11": null,
    "indicador_15": 0,
    "indicador_17": 1,
    "indicador_19": null,
    "indicador_22": null,
    "indicador_25": null,
    "indicador_26": null,
    "indicador_27": null,
    "monto_adjudicacion": 200,
    "monto_contrato": 0,
    "monto_presupuestado": 324
  },
  {
    "codigo": "RC-102564",
    "fecha_publicacion": "2008-06-18",
    "indicador_01": null,
    "indicador_04": null,
    "indicador_05": 1,
    "indicador_06": null,
    "indicador_09": null,
    "indicador_11": null,
    "indicador_15": 0,
    "indicador_17": 0,
    "indicador_19": null,
    "indicador_22": null,
    "indicador_25": null,
    "indicador_26": null,
    "indicador_27": null,
    "monto_adjudicacion": 0,
    "monto_contrato": 0,
    "monto_presupuestado": 96329.52
  }  
]
```

## Get SIE Procesos Anual 

```python
import http.client

conn = http.client.HTTPSConnection("serverapiSoceIP", 5000)
payload = ''
headers = {}
conn.request("GET", "/api/soce_sie_ic_proceso/[{
  \"dateRange\":{
    \"dateRangeStart\":{
      \"pickedYear\": \"2008\",
      \"pickedMonth\": \"1\",
      \"pickedDay\": \"1\"
    },
    \"dateRangeEnd\": {
      \"pickedYear\": \"2015\",
      \"pickedMonth\": \"2\",
      \"pickedDay\": \"3\"
    }
  },
  \"monto\": \"true\",
  \"processStage\": \"adjudicada\",
  \"purchaseType\": [\"bien\", \"bien_y_servicio\", \"servicio\", \"farmacos\"],
  \"timeScale\": \"anual\",\"typeGraph\": 1
}]", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))
```

```javascript
var requestOptions = {
  method: 'GET',
  redirect: 'follow'
};

fetch("http://serverapiSoceIP:5000/api/soce_sie_ic_proceso/[{\n  \"dateRange\":{\n    \"dateRangeStart\":{\n      \"pickedYear\": \"2008\",\n      \"pickedMonth\": \"1\",\n      \"pickedDay\": \"1\"\n    },\n    \"dateRangeEnd\": {\n      \"pickedYear\": \"2015\",\n      \"pickedMonth\": \"2\",\n      \"pickedDay\": \"3\"\n    }\n  },\n  \"monto\": \"true\",\n  \"processStage\": \"adjudicada\",\n  \"purchaseType\": [\"bien\", \"bien_y_servicio\", \"servicio\", \"farmacos\"],\n  \"timeScale\": \"anual\",\"typeGraph\": 1\n}]", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```

Este endpoint devuelve información sobre procesos SIE, en los cuales se calcularon los indicadores individuales, obteniéndose una calificación promedio que sirve para medir el riesgo en el proceso. Los datos están agrupados anualmente, por lo tanto, la información se consolida a ese nivel de agrupación.

### HTTP Request

`GET http://serverapiSoceIP/api/soce_sie_ic_proceso/{{process_list}}`

### Definición de parámetros

Element | Description
--------- | -----------
process_list | Parámetro en formato JSON.


### Estructura del parámetro JSON

Element | Description
------- | -----------
dateRage | Elemento json para definir un rango de fechas
dateRangeStart | Elemento json para definir la fecha inicial
dateRangeEnd | Elemento json para definir la fecha final
pickedYear | Elemento para definir el año inicial o final
pickedMonth | Elemento para definir el mes inicial o final (el mes debe estar entre 0 y 11, siendo 0 Enero, y 11 Diciembre)
pickedDay | Elemento para definir el día inicial o final
monto | Elemento que puede ser TRUE o FALSE, en el caso de estar marcado se muestra información acerca del monto presupuestado del proceso
processStage | Elemento que contiene el estado del proceso. Para SIE se consideran 21 estados
purchaseType | Elemento array que contiene el tipo de compra que puede contener estos valores: bien, bien_y_servicio, servicio, farmacos
timeScale | Elemento que indica al endpoint la frecuencia en la que se debe recuperar los datos, en este caso, debe tener el valor "anual"
typeGraph | Elemnto que define la forma en la que se recuperan los datos, para este caso, debe tener el valor "1"

>Example JSON parameter:

```json
[{
  "dateRange":{
    "dateRangeStart":{
      "pickedYear": "2008",
      "pickedMonth": "1",
      "pickedDay": "1"
    },
    "dateRangeEnd": {
      "pickedYear": "2015",
      "pickedMonth": "2",
      "pickedDay": "3"
    }
  },
  "monto": "true",
  "processStage": "adjudicada",
  "purchaseType": ["bien", "bien_y_servicio", "servicio", "farmacos"],
  "timeScale": "anual",
  "typeGraph": 1
}]
```

### Estructura de la respuesta JSON

Element | Description
------- | -----------
anio | Elemento que muestra la año de la fecha de publicación de un proceso
avg_indicadores | Elemento que muestra la calificación para ese año, este es el valor del índice compuesto que se calcula sacando el promedio por cada proceso y sus respectivos indicadores individuales, este valor está entre 0 y 100%
quartiles | Elemento que indica el valor del cuartial relacionado a la calificación obtenida en el rango de fechas
riesgo | Elemento que indica el riesgo para mes  en la que se agrupan los resultados
std_monto_presupuestado | Elemento que muestra el monto presupuestado de los procesos considerados en el rango de fechas
std_monto_adjudicacion | Elemento que muestra el monto adjudicado de los procesos considerados en el rango de fechas
std_monto_contrato | Elemento que muestra el monto de contrato de los procesos considerados en el rango de fechas


>Example Response:

```json
[
  {
    "anio": 2008,
    "avg_indicadores": 74.24,
    "quartiles": 2,
    "riesgo": "Medio",
    "std_monto_adjudicacion": 225133.97,
    "std_monto_contrato": 0,
    "std_monto_presupuestado": 485746.31
  },
  {
    "anio": 2009,
    "avg_indicadores": 75.44,
    "quartiles": 3,
    "riesgo": "Bajo",
    "std_monto_adjudicacion": 209476.26,
    "std_monto_contrato": 0,
    "std_monto_presupuestado": 286897.32
  },
  {
    "anio": 2010,
    "avg_indicadores": 78.96,
    "quartiles": 3,
    "riesgo": "Bajo",
    "std_monto_adjudicacion": 356021.15,
    "std_monto_contrato": 0,
    "std_monto_presupuestado": 9832336.13
  },
  {
    "anio": 2011,
    "avg_indicadores": 73.13,
    "quartiles": 2,
    "riesgo": "Medio",
    "std_monto_adjudicacion": 72486.74,
    "std_monto_contrato": 0,
    "std_monto_presupuestado": 80630.65
  },
  {
    "anio": 2012,
    "avg_indicadores": 76.12,
    "quartiles": 3,
    "riesgo": "Bajo",
    "std_monto_adjudicacion": 264797.61,
    "std_monto_contrato": 0,
    "std_monto_presupuestado": 376732.35
  },
  {
    "anio": 2013,
    "avg_indicadores": 72.33,
    "quartiles": 2,
    "riesgo": "Medio",
    "std_monto_adjudicacion": 15834.73,
    "std_monto_contrato": 0,
    "std_monto_presupuestado": 131189.39
  }
]
```


## Get SIE Procesos Anual Detalle

```python
import http.client

conn = http.client.HTTPSConnection("serverapiSoceIP", 5000)
payload = ''
headers = {}
conn.request("GET", "/api/soce_sie_ic_proceso_detail/[{
  \"dateRange\":{
    \"dateRangeStart\":{
      \"pickedYear\": \"2008\",
      \"pickedMonth\": \"1\",
      \"pickedDay\": \"1\"
    },
    \"dateRangeEnd\": {
      \"pickedYear\": \"2009\",
      \"pickedMonth\": \"2\",
      \"pickedDay\": \"3\"
    }
  },
  \"monto\": \"true\",
  \"processStage\": \"adjudicada\",
  \"purchaseType\": [\"bien\", \"bien_y_servicio\", \"servicio\", \"farmacos\"],
  \"timeScale\": \"anual\",\"typeGraph\": 1
}]", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))
```

```javascript
var requestOptions = {
  method: 'GET',
  redirect: 'follow'
};

fetch("http://serverapiSoceIP:5000/api/soce_sie_ic_proceso_detail/[{\n  \"dateRange\":{\n    \"dateRangeStart\":{\n      \"pickedYear\": \"2008\",\n      \"pickedMonth\": \"1\",\n      \"pickedDay\": \"1\"\n    },\n    \"dateRangeEnd\": {\n      \"pickedYear\": \"2009\",\n      \"pickedMonth\": \"2\",\n      \"pickedDay\": \"3\"\n    }\n  },\n  \"monto\": \"true\",\n  \"processStage\": \"adjudicada\",\n  \"purchaseType\": [\"bien\", \"bien_y_servicio\", \"servicio\", \"farmacos\"],\n  \"timeScale\": \"anual\",\"typeGraph\": 1\n}]", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```

Este endpoint devuelve información detallada sobre procesos SIE, la misma que puede ser usada para calcular la calificación promedio de los procesos considereados en frecuencia anual.

### HTTP Request

`GET http://serverapiSoceIP/api/soce_sie_ic_proceso_detail/{{process_list}}`

### Definición de parámetros

Element | Description
--------- | -----------
process_list | Parámetro en formato JSON.


### Estructura del parámetro JSON

Element | Description
------- | -----------
dateRage | Elemento json para definir un rango de fechas
dateRangeStart | Elemento json para definir la fecha inicial
dateRangeEnd | Elemento json para definir la fecha final
pickedYear | Elemento para definir el año inicial o final
pickedMonth | Elemento para definir el mes inicial o final (el mes debe estar entre 0 y 11, siendo 0 Enero, y 11 Diciembre)
pickedDay | Elemento para definir el día inicial o final
monto | Elemento que puede ser TRUE o FALSE, en el caso de estar marcado se muestra información acerca del monto presupuestado del proceso
processStage | Elemento que contiene el estado del proceso. Para SIE se consideran 21 estados
purchaseType | Elemento array que contiene el tipo de compra que puede contener estos valores: bien, bien_y_servicio, servicio, farmacos
timeScale | Elemento que indica al endpoint la frecuencia en la que se debe recuperar los datos, en este caso, debe tener el valor "anual"
typeGraph | Elemnto que define la forma en la que se recuperan los datos, para este caso, debe tener el valor "1"


>Example JSON parameter:

```json
[{
  "dateRange":{
    "dateRangeStart":{
      "pickedYear": "2008",
      "pickedMonth": "1",
      "pickedDay": "1"
    },
    "dateRangeEnd": {
      "pickedYear": "2009",
      "pickedMonth": "2",
      "pickedDay": "3"
    }
  },
  "monto": "true",
  "processStage": "adjudicada",
  "purchaseType": ["bien", "bien_y_servicio", "servicio", "farmacos"],
  "timeScale": "anual",
  "typeGraph": 1
}]
```

### Estructura de la respuesta JSON

Element | Description
------- | -----------
fecha_publicacion | Elemento que muestra la fecha de publicación, para este caso, el año es el objetivo del análisis
codigo | Elemento que muestra el código del Proceso 
indicador_## | Elemento que muestra el valor de calificación del proceso en cada indicador. Se tienen indicadores desde el 01 hasta el 27, el valor 1 significafa que le proceso cumple con la ficha metodológica de cálculo, es decir es un proceso "bueno", mientras que el valor de 0 indica que ese proceso no cumplió la ficha, por lo tanto su calificación es "mala". Para el caso de valores NULOS o null, significa que el proceso no participó en el análisis o cálculo para un particular indicador.
monto_presupuestado | Elemento que muestra el monto presupuestado de los procesos considerados en el rango de fechas
monto_adjudicacion | Elemento que muestra el monto adjudicado de los procesos considerados en el rango de fechas
monto_contrato | Elemento que muestra el monto de contrato de los procesos considerados en el rango de fechas


>Example Response:

```json
[
  {
    "codigo": "HTP-GRS-003-09",
    "fecha_publicacion": "2009-09-11",
    "indicador_01": 1,
    "indicador_04": 1,
    "indicador_05": 1,
    "indicador_06": null,
    "indicador_09": null,
    "indicador_11": null,
    "indicador_15": 0,
    "indicador_17": 1,
    "indicador_19": null,
    "indicador_22": null,
    "indicador_25": null,
    "indicador_26": null,
    "indicador_27": null,
    "monto_adjudicacion": 2580,
    "monto_contrato": 0,
    "monto_presupuestado": 3699
  },
  {
    "codigo": "SIE-4AN-HDPNG-2-2009",
    "fecha_publicacion": "2009-06-08",
    "indicador_01": 1,
    "indicador_04": 1,
    "indicador_05": 1,
    "indicador_06": null,
    "indicador_09": null,
    "indicador_11": null,
    "indicador_15": 0,
    "indicador_17": 1,
    "indicador_19": null,
    "indicador_22": null,
    "indicador_25": null,
    "indicador_26": null,
    "indicador_27": null,
    "monto_adjudicacion": 2036,
    "monto_contrato": 0,
    "monto_presupuestado": 2100
  },
  {
    "codigo": "SIE-8EC-HDPNG2-2009",
    "fecha_publicacion": "2009-06-11",
    "indicador_01": 1,
    "indicador_04": null,
    "indicador_05": 1,
    "indicador_06": null,
    "indicador_09": null,
    "indicador_11": null,
    "indicador_15": 0,
    "indicador_17": 1,
    "indicador_19": null,
    "indicador_22": null,
    "indicador_25": null,
    "indicador_26": null,
    "indicador_27": null,
    "monto_adjudicacion": 5999,
    "monto_contrato": 0,
    "monto_presupuestado": 8000
  }
]
```

## Get SIE Procesos Riesgo

```python
import http.client

conn = http.client.HTTPSConnection("serverapiSoceIP", 5000)
payload = ''
headers = {}
conn.request("GET", "/api/soce_sie_procesos_riesgo", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))
```

```javascript
var requestOptions = {
  method: 'GET',
  redirect: 'follow'
};

fetch("http://serverapiSoceIP:5000/api/soce_sie_procesos_riesgo", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```

Este endpoint devuelve información sobre procesos SIE, agrupados por nivel de riesgo. No hay parámetros de búsqueda.

### HTTP Request

`GET http://serverapiSoceIP/api/soce_sie_procesos_riesgo`


### Estructura de la respuesta JSON

Element | Description
------- | -----------
total_sie_registros | Elemento que el total de procesos SIE bajados y procesados en el cálculo del índice compuesto
riesgo | Elemento que muestra el nivel de riesgo que puede ser: Muy Alto, Alto, Medio y Bajo
nro_procesos_x_riesgo | Elemento que muestra el número de procesos en cada nivel de riesgo
porcentaje | Elemento que muestra su correspondiente porcentaje respecto al número total de procesos SIE.


>Example Response:

```json
[
  {
    "nro_procesos_x_riesgo": 9230,
    "porcentaje": "2.18",
    "riesgo": "Alto",
    "total_sie_registros": "423967"
  },
  {
    "nro_procesos_x_riesgo": 244172,
    "porcentaje": "57.59",
    "riesgo": "Bajo",
    "total_sie_registros": "423967"
  },
  {
    "nro_procesos_x_riesgo": 170520,
    "porcentaje": "40.22",
    "riesgo": "Medio",
    "total_sie_registros": "423967"
  },
  {
    "nro_procesos_x_riesgo": 45,
    "porcentaje": "0.01",
    "riesgo": "Muy Alto",
    "total_sie_registros": "423967"
  }
]
```


## Get SIE Procesos por Anio

```python
import http.client

conn = http.client.HTTPSConnection("serverapiSoceIP", 5000)
payload = ''
headers = {}
conn.request("GET", "/api/soce_sie_procesos_anio", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))
```

```javascript
var requestOptions = {
  method: 'GET',
  redirect: 'follow'
};

fetch("http://serverapiSoceIP:5000/api/soce_sie_procesos_anio", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```

Este endpoint devuelve información sobre procesos SIE, agrupados por año. No hay parámetros de búsqueda.

### HTTP Request

`GET http://serverapiSoceIP/api/soce_sie_procesos_anio`

### Estructura de la respuesta JSON

Element | Description
------- | -----------
total_sie_registros | Elemento que el total de procesos SIE bajados y procesados en el cálculo del índice compuesto
anio | Elemento que muestra el año en donde se agrupan los procesos
nro_procesos_x_anio | Elemento que muestra el número de procesos por cada año 
porcentaje | Elemento que muestra su correspondiente porcentaje respecto al número total de procesos SIE


>Example Response:

```json
[
  {
    "anio": 2008,
    "nro_procesos_x_anio": 2413,
    "porcentaje": 0.57,
    "total_sie_registros": 423967
  },
  {
    "anio": 2009,
    "nro_procesos_x_anio": 31815,
    "porcentaje": 7.5,
    "total_sie_registros": 423967
  },
  {
    "anio": 2010,
    "nro_procesos_x_anio": 50391,
    "porcentaje": 11.89,
    "total_sie_registros": 423967
  }
]
```

## Get SIE Proceso Patron

```python
import http.client

conn = http.client.HTTPSConnection("serverapiSoceIP", 5000)
payload = ''
headers = {}
conn.request("GET", "/api/soce_sie_ic_proceso/[
  {
    \"typeGraph\": 2,
    \"patron\": \"BASALI\"
  }
]", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))
```

```javascript
var requestOptions = {
  method: 'GET',
  redirect: 'follow'
};

fetch("http://serverapiSoceIP:5000/api/soce_sie_ic_proceso/[\n  {\n    \"typeGraph\": 2,\n    \"patron\": \"BASALI\"\n  }\n]", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```

Este endpoint devuelve información detallada sobre procesos SIE, basados en un patrón o criterio de búsqueda.

### HTTP Request

`GET http://serverapiSoceIP/api/soce_sie_ic_proceso/{{process_list}}`

### Definición de parámetros

Element | Description
--------- | -----------
process_list | Parámetro en formato JSON.


### Estructura del parámetro JSON

Element | Description
------- | -----------
typeGraph | Elemento que indica el tipo de gráfico que se usará, en ete caso siempre es "2"
patron | Elemento que sirve para pasar un patrón o criterio de búsqueda, el mismo que se puede aplicar en: código del proceso, nombre de la entidad contratante, objecto del proceso, nombre y ruc del proveedor


>Example JSON parameter:

```json
[
  {
    "typeGraph": 2,
    "patron": "BASALI"
  }
]
```

### Estructura de la respuesta JSON

Element | Description
------- | -----------
anio | Elemento que muestra el año de la fecha de publicación del proceso
mes | Elemento que muestra el mes de la fecha de publicación del proceso
codigo | Elemento que muestra el código del Proceso 
contract_id | Elemento que muestra un código interno en la DB
entidad | Elemento que muestra el nombre de la entidad contratante
estado_del_proceso | Elemento que muestra el estado del proceso
fecha_publicacion | Elemento que muestra la fecha de publicación del proceso
indicador_## | Elemento que muestra el valor de calificación del proceso en cada indicador. Se tienen indicadores desde el 01 hasta el 27, el valor 1 significafa que le proceso cumple con la ficha metodológica de cálculo, es decir es un proceso "bueno", mientras que el valor de 0 indica que ese proceso no cumplió la ficha, por lo tanto su calificación es "mala". Para el caso de valores NULOS o null, significa que el proceso no participó en el análisis o cálculo para un particular indicador.
objeto_de_proceso | Elemento que muestra una descripción sobre el objeto del proceso
ruc | Elemento que muestra el RUC del proveedor adjudicado, dependiendo de la fase de calificación contracutal, este dato puede o no existir
proveedor | Elemento que muestra el nombre del proveedor adjudicado, dependiendo de la fase de calificación contracutal, este dato puede o no existir
tipo_compra | Elemento que muestra el tipo de compra que puede ser: bien, bien_y_servicio, servicio, farmacos
monto_presupuestado | Elemento que muestra el monto presupuestado de los procesos considerados en el rango de fechas
monto_adjudicacion | Elemento que muestra el monto adjudicado de los procesos considerados en el rango de fechas
monto_contrato | Elemento que muestra el monto de contrato de los procesos considerados en el rango de fechas
promedio | Elemento que muestra la calificación del proceso basado en los valores obtenidos en cada indicador. Recordemos que a nivel de procesos, la calificación puede ser 1 y 0; siendo 1 un proceso "bueno" o que cumple la ficha metedológica de cálculo, y 0 significa que es "malo", o no cumple con la ficha.


>Example Response:

```json
[
  {
    "anio": "2018",
    "codigo": "SIE-BASALI-013-2018",
    "contract_id": "1389384",
    "entidad": "BASE NAVAL DE SALINAS",
    "estado_del_proceso": "Desierta",
    "fecha_publicacion": "21/03/2018",
    "indicador_01": null,
    "indicador_04": null,
    "indicador_05": "1",
    "indicador_06": null,
    "indicador_09": null,
    "indicador_11": null,
    "indicador_15": "0",
    "indicador_17": null,
    "indicador_19": null,
    "indicador_22": null,
    "indicador_25": "1",
    "indicador_26": "1",
    "indicador_27": "1",
    "mes": "3",
    "monto_adjudicacion": "$0.00",
    "monto_contrato": "$0.00",
    "monto_presupuestado": "$0.00",
    "objeto_de_proceso": "MANTENIM. Y ADQUISICION REPUESTOS PARA EMBARCACIONES A VELA ESSUNA",
    "promedio": "0.80",
    "proveedor": null,
    "ruc": null,
    "tipo_compra": "Bien"
  },
  {
    "anio": "2016",
    "codigo": "SIE-BASALI-006-2016",
    "contract_id": "1216909",
    "entidad": "BASE NAVAL DE SALINAS",
    "estado_del_proceso": "Desierta",
    "fecha_publicacion": "04/03/2016",
    "indicador_01": "1",
    "indicador_04": null,
    "indicador_05": "1",
    "indicador_06": null,
    "indicador_09": null,
    "indicador_11": null,
    "indicador_15": "0",
    "indicador_17": null,
    "indicador_19": null,
    "indicador_22": null,
    "indicador_25": "1",
    "indicador_26": "1",
    "indicador_27": "1",
    "mes": "3",
    "monto_adjudicacion": "$0.00",
    "monto_contrato": "$0.00",
    "monto_presupuestado": "$11,696.33",
    "objeto_de_proceso": "ADQUISICION DE PASAJES AEREOS PARA BASALI",
    "promedio": "0.83",
    "proveedor": null,
    "ruc": null,
    "tipo_compra": "Servicio"
  }
]
```


## Get SIE Proceso Patron Detalle

```python
import http.client

conn = http.client.HTTPSConnection("serverapiSoceIP", 5000)
payload = ''
headers = {}
conn.request("GET", "/api/soce_sie_ic_proceso_detail/[
  {
    \"typeGraph\": 2,
    \"patron\": \"BMB VENTAS\"
  }
]", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))
```

```javascript
var requestOptions = {
  method: 'GET',
  redirect: 'follow'
};

fetch("http://serverapiSoceIP:5000/api/soce_sie_ic_proceso_detail/[\n  {\n    \"typeGraph\": 2,\n    \"patron\": \"BMB VENTAS\"\n  }\n]", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```

Este endpoint devuelve información detallada sobre procesos SIE, basados en un patrón o criterio de búsqueda.

### HTTP Request

`GET http://serverapiSoceIP/api/soce_sie_ic_proceso_detail/{{process_list}}`

### Definición de parámetros

Element | Description
--------- | -----------
process_list | Parámetro en formato JSON.


### Estructura del parámetro JSON

Element | Description
------- | -----------
typeGraph | Elemento que indica el tipo de gráfico que se usará, en ete caso siempre es "2"
patron | Elemento que sirve para pasar un patrón o criterio de búsqueda, el mismo que se puede aplicar en: código del proceso, nombre de la entidad contratante, objecto del proceso, nombre y ruc del proveedor



>Example JSON parameter:

```json
[
  {
    "typeGraph": 2,
    "patron": "BMB VENTAS"
  }
]
```

### Estructura de la respuesta JSON

Element | Description
------- | -----------
fecha_publicacion | Elemento que muestra la fecha de publicación del proceso
codigo | Elemento que muestra el código del Proceso 
contract_id | Elemento que muestra un código interno en la DB
entidad | Elemento que muestra el nombre de la entidad contratante
estado_del_proceso | Elemento que muestra el estado del proceso
indicador_## | Elemento que muestra el valor de calificación del proceso en cada indicador. Se tienen indicadores desde el 01 hasta el 27, el valor 1 significafa que le proceso cumple con la ficha metodológica de cálculo, es decir es un proceso "bueno", mientras que el valor de 0 indica que ese proceso no cumplió la ficha, por lo tanto su calificación es "mala". Para el caso de valores NULOS o null, significa que el proceso no participó en el análisis o cálculo para un particular indicador.
objeto_de_proceso | Elemento que muestra una descripción sobre el objeto del proceso
ruc | Elemento que muestra el RUC del proveedor adjudicado, dependiendo de la fase de calificación contracutal, este dato puede o no existir
proveedor | Elemento que muestra el nombre del proveedor adjudicado, dependiendo de la fase de calificación contracutal, este dato puede o no existir
tipo_compra | Elemento que muestra el tipo de compra que puede ser: bien, bien_y_servicio, servicio, farmacos
monto_presupuestado | Elemento que muestra el monto presupuestado de los procesos considerados en el rango de fechas
monto_adjudicacion | Elemento que muestra el monto adjudicado de los procesos considerados en el rango de fechas
monto_contrato | Elemento que muestra el monto de contrato de los procesos considerados en el rango de fechas


>Example Response:

```json
[
  {
    "codigo": "IESS-HN1A-SIE-003-10",
    "entidad": "HOSPITAL BASICO ANCON",
    "estado_del_proceso": "Finalizada",
    "fecha_publicacion": "2010-04-14",
    "indicador_01": 1,
    "indicador_04": 0,
    "indicador_05": 1,
    "indicador_06": null,
    "indicador_09": null,
    "indicador_11": null,
    "indicador_15": 0,
    "indicador_17": 1,
    "indicador_19": null,
    "indicador_22": null,
    "indicador_25": null,
    "indicador_26": 1,
    "indicador_27": 1,
    "monto_adjudicacion": 27250,
    "monto_contrato": 0,
    "monto_presupuestado": 36300,
    "objeto_de_proceso": "Ropería y Lencería",
    "proveedor": "BMB VENTAS Y REPRESENTACIONES",
    "ruc": "0910044155001",
    "tipo_compra": "Bien"
  },
  {
    "codigo": "CP-0006-09-DHG-NDC",
    "entidad": "HOSPITAL GENERAL DR. NAPOLEON DÁVILA CÓRDOVA",
    "estado_del_proceso": "Finalizada",
    "fecha_publicacion": "2009-04-14",
    "indicador_01": 1,
    "indicador_04": null,
    "indicador_05": 1,
    "indicador_06": null,
    "indicador_09": null,
    "indicador_11": null,
    "indicador_15": 0,
    "indicador_17": 1,
    "indicador_19": null,
    "indicador_22": null,
    "indicador_25": null,
    "indicador_26": 1,
    "indicador_27": 1,
    "monto_adjudicacion": 2750,
    "monto_contrato": 0,
    "monto_presupuestado": 3050,
    "objeto_de_proceso": "ADQUISICION DE MATERIALES PARA RAYOS X.",
    "proveedor": "BMB VENTAS Y REPRESENTACIONES",
    "ruc": "0910044155001",
    "tipo_compra": "Bien"
  }
]
```


## Get SIE Entidad Mesual

```python
import http.client

conn = http.client.HTTPSConnection("serverapiSoceIP", 5000)
payload = ''
headers = {}
conn.request("GET", "/api/soce_sie_ic_entidad/[{
  \"dateRange\":{
    \"dateRangeStart\":{
      \"pickedYear\": \"2009\",
      \"pickedMonth\": \"0\",
      \"pickedDay\": \"1\"
    },
    \"dateRangeEnd\": {
      \"pickedYear\": \"2009\",
      \"pickedMonth\": \"11\",
      \"pickedDay\": \"3\"
    }
  },
  \"timeScale\": \"mensual\",
  \"typeGraph\": 1
}]", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))
```

```javascript
var requestOptions = {
  method: 'GET',
  redirect: 'follow'
};

fetch("http://serverapiSoceIP:5000/api/soce_sie_ic_entidad/[{\n  \"dateRange\":{\n    \"dateRangeStart\":{\n      \"pickedYear\": \"2009\",\n      \"pickedMonth\": \"0\",\n      \"pickedDay\": \"1\"\n    },\n    \"dateRangeEnd\": {\n      \"pickedYear\": \"2009\",\n      \"pickedMonth\": \"11\",\n      \"pickedDay\": \"3\"\n    }\n  },\n  \"timeScale\": \"mensual\",\n  \"typeGraph\": 1\n}]", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```

Este endpoint devuelve información sobre procesos SIE agrupados por Entidad, en los cuales se calcularon los indicadores individuales, obteniéndose una calificación promedio que sirve para medir el riesgo en el proceso. Los datos están agrupados mensualmente, por lo tanto, la información se consolida a ese nivel de Entidad y Mes.

### HTTP Request

`GET http://serverapiSoceIP/api/soce_sie_ic_entidad/{{process_list}}`

### Definición de parámetros

Element | Description
--------- | -----------
process_list | Parámetro en formato JSON.


### Estructura del parámetro JSON

Element | Description
------- | -----------
dateRage | Elemento json para definir un rango de fechas
dateRangeStart | Elemento json para definir la fecha inicial
dateRangeEnd | Elemento json para definir la fecha final
pickedYear | Elemento para definir el año inicial o final
pickedMonth | Elemento para definir el mes inicial o final (el mes debe estar entre 0 y 11, siendo 0 Enero, y 11 Diciembre)
pickedDay | Elemento para definir el día inicial o final
timeScale | Elemento que indica al endpoint la frecuencia en la que se debe recuperar los datos, en este caso, debe tener el valor "mensual"
typeGraph | Elemnto que define la forma en la que se recuperan los datos, para este caso, debe tener el valor "1"


>Example JSON parameter:


```json
[{
  "dateRange":{
    "dateRangeStart":{
      "pickedYear": "2009",
      "pickedMonth": "0",
      "pickedDay": "1"
    },
    "dateRangeEnd": {
      "pickedYear": "2009",
      "pickedMonth": "11",
      "pickedDay": "3"
    }
  },
  "timeScale": "mensual",
  "typeGraph": 1
}]
```

### Estructura de la respuesta JSON

Element | Description
------- | -----------
entidad | Elemento que muestra el nombre de la entidad contratante
anio | Elemento que muestra el año relacionado a la fecha de publicación de los procesos
mes | Elemento que muestra el mes relacionado a la fecha de publicación de los procesos
avg_indicadores | Elemento que muestra la calificación promedio para la entidad en el mes analizado
quartiles | Elemento que muestra el valor del cuartil respecto al elemento avg_indicadores
riesgo | Elemento que muestra el nivel de riesgo para la entidad contratante, basado en el cuartil


>Example Response:

```json
[
  {
    "anio": 2009,
    "avg_indicadores": 100,
    "entidad": "Empresa Municipal de Agua Potable y Alcantarillado de Pujili",
    "mes": 1,
    "quartiles": 3,
    "riesgo": "Bajo"
  },
  {
    "anio": 2009,
    "avg_indicadores": 100,
    "entidad": "Comite Permanente de Fiestas de Provincializacion de Sucumbios",
    "mes": 1,
    "quartiles": 3,
    "riesgo": "Bajo"
  },
  {
    "anio": 2009,
    "avg_indicadores": 93.33,
    "entidad": "Municipio de Olmedo",
    "mes": 1,
    "quartiles": 3,
    "riesgo": "Bajo"
  },
  {
    "anio": 2009,
    "avg_indicadores": 87.5,
    "entidad": "CORPORACION NACIONAL DE TELECOMUNICACIONES CNT S.A",
    "mes": 1,
    "quartiles": 3,
    "riesgo": "Bajo"
  },
  {
    "anio": 2009,
    "avg_indicadores": 87.5,
    "entidad": "HIDROAGOYAN S.A",
    "mes": 1,
    "quartiles": 3,
    "riesgo": "Bajo"
  }
]
```


## Get SIE Entidad Mensual Detalle

```python
import http.client

conn = http.client.HTTPSConnection("serverapiSoceIP", 5000)
payload = ''
headers = {}
conn.request("GET", "/api/soce_sie_ic_entidad_detail/[{
  \"dateRange\":{
    \"dateRangeStart\":{
      \"pickedYear\": \"2009\",
      \"pickedMonth\": \"0\",
      \"pickedDay\": \"1\"
    },
    \"dateRangeEnd\": {
      \"pickedYear\": \"2009\",
      \"pickedMonth\": \"11\",
      \"pickedDay\": \"3\"
    }
  },
  \"timeScale\": \"mensual\",
  \"typeGraph\": 1
}]", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))
```

```javascript
var requestOptions = {
  method: 'GET',
  redirect: 'follow'
};

fetch("http://serverapiSoceIP:5000/api/soce_sie_ic_entidad_detail/[{\n  \"dateRange\":{\n    \"dateRangeStart\":{\n      \"pickedYear\": \"2009\",\n      \"pickedMonth\": \"0\",\n      \"pickedDay\": \"1\"\n    },\n    \"dateRangeEnd\": {\n      \"pickedYear\": \"2009\",\n      \"pickedMonth\": \"11\",\n      \"pickedDay\": \"3\"\n    }\n  },\n  \"timeScale\": \"mensual\",\n  \"typeGraph\": 1\n}]", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```

Este endpoint devuelve información detallada sobre procesos SIE agrupados por Entidad, en los cuales se calcularon los indicadores individuales, obteniéndose una calificación promedio que sirve para medir el riesgo en el proceso. Los datos están agrupados mensualmente, por lo tanto, la información se consolida a ese nivel de Entidad y Mes.

### HTTP Request

`GET http://serverapiSoceIP/api/soce_sie_ic_entidad_detail/{{process_list}}`

### Definición de parámetros

Element | Description
--------- | -----------
process_list | Parámetro en formato JSON.


### Estructura del parámetro JSON

Element | Description
------- | -----------
dateRage | Elemento json para definir un rango de fechas
dateRangeStart | Elemento json para definir la fecha inicial
dateRangeEnd | Elemento json para definir la fecha final
pickedYear | Elemento para definir el año inicial o final
pickedMonth | Elemento para definir el mes inicial o final (el mes debe estar entre 0 y 11, siendo 0 Enero, y 11 Diciembre)
pickedDay | Elemento para definir el día inicial o final
timeScale | Elemento que indica al endpoint la frecuencia en la que se debe recuperar los datos, en este caso, debe tener el valor "mensual"
typeGraph | Elemnto que define la forma en la que se recuperan los datos, para este caso, debe tener el valor "1"


>Example JSON parameter:

```json
[{
  "dateRange":{
    "dateRangeStart":{
      "pickedYear": "2009",
      "pickedMonth": "0",
      "pickedDay": "1"
    },
    "dateRangeEnd": {
      "pickedYear": "2009",
      "pickedMonth": "11",
      "pickedDay": "3"
    }
  },
  "timeScale": "mensual",
  "typeGraph": 1
}]
```

### Estructura de la respuesta JSON

Element | Description
------- | -----------
anio | Elemento que muestra el año de la fecha de publicación del proceso, sobre este año se debe agrupar
mes | Elemento que muestra el mes de la fecha de publicación del proceso, sobre este mes se debe agrupar
dia | Elemento que muestra la fecha del inicio del mes en donde se agrupan los datos
entidad | Elemento que muestra el nombre de la entidad contratante
indicador_## | Elemento que muestra el valor de calificación del proceso en cada indicador. Se tienen indicadores desde el 01 hasta el 27, el valor de calificación para cada indicador individual se mide en porcentaje que va del 0 al 100%, siendo 0% una mala calificación, y 100% una buena calificación. Refierase a la metodología de cálculo descrita en el sitio web.


>Example Response:

```json
[
  {
    "anio": 2009,
    "dia": "2009-08-01",
    "entidad": "SECAP",
    "indicador_01": 100,
    "indicador_04": null,
    "indicador_05": 100,
    "indicador_06": null,
    "indicador_09": null,
    "indicador_11": null,
    "indicador_15": null,
    "indicador_17": null,
    "indicador_19": null,
    "indicador_22": null,
    "indicador_25": null,
    "indicador_26": null,
    "indicador_27": null,
    "mes": 8
  },
  {
    "anio": 2009,
    "dia": "2009-11-01",
    "entidad": "CENTRO DE ESPECIALIDADES CENTRAL CUENCA",
    "indicador_01": 100,
    "indicador_04": null,
    "indicador_05": 100,
    "indicador_06": null,
    "indicador_09": null,
    "indicador_11": null,
    "indicador_15": 0,
    "indicador_17": null,
    "indicador_19": null,
    "indicador_22": null,
    "indicador_25": null,
    "indicador_26": null,
    "indicador_27": null,
    "mes": 11
  },
  {
    "anio": 2009,
    "dia": "2009-10-01",
    "entidad": "COLEGIO EXPERIMENTAL AMBATO",
    "indicador_01": 100,
    "indicador_04": 100,
    "indicador_05": 100,
    "indicador_06": null,
    "indicador_09": null,
    "indicador_11": null,
    "indicador_15": 0,
    "indicador_17": 100,
    "indicador_19": null,
    "indicador_22": null,
    "indicador_25": null,
    "indicador_26": null,
    "indicador_27": null,
    "mes": 10
  }
]
```

## Get SIE Entidad Year

```python
import http.client

conn = http.client.HTTPSConnection("serverapiSoceIP", 5000)
payload = ''
headers = {}
conn.request("GET", "/api/soce_sie_ic_entidad/[{
  \"dateRange\":{
    \"dateRangeStart\":{
      \"pickedYear\": \"2008\",
      \"pickedMonth\": \"0\",
      \"pickedDay\": \"1\"
    },
    \"dateRangeEnd\": {
      \"pickedYear\": \"2008\",
      \"pickedMonth\": \"0\",
      \"pickedDay\": \"3\"
    }
  },
  \"timeScale\": \"anual\",
  \"typeGraph\": 1
}]", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))
```

```javascript
var requestOptions = {
  method: 'GET',
  redirect: 'follow'
};

fetch("http://serverapiSoceIP:5000/api/soce_sie_ic_entidad/[{\n  \"dateRange\":{\n    \"dateRangeStart\":{\n      \"pickedYear\": \"2008\",\n      \"pickedMonth\": \"0\",\n      \"pickedDay\": \"1\"\n    },\n    \"dateRangeEnd\": {\n      \"pickedYear\": \"2008\",\n      \"pickedMonth\": \"0\",\n      \"pickedDay\": \"3\"\n    }\n  },\n  \"timeScale\": \"anual\",\n  \"typeGraph\": 1\n}]", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```

Este endpoint devuelve información sobre procesos SIE agrupados por Entidad, en los cuales se calcularon los indicadores individuales, obteniéndose una calificación promedio que sirve para medir el riesgo en el proceso. Los datos están agrupados anualmente, por lo tanto, la información se consolida a ese nivel de Entidad y Año.

### HTTP Request

`GET http://serverapiSoceIP/api/soce_sie_ic_entidad/{{process_list}}`

### Definición de parámetros

Element | Description
--------- | -----------
process_list | Parámetro en formato JSON.


### Estructura del parámetro JSON

Element | Description
------- | -----------
dateRage | Elemento json para definir un rango de fechas
dateRangeStart | Elemento json para definir la fecha inicial
dateRangeEnd | Elemento json para definir la fecha final
pickedYear | Elemento para definir el año inicial o final
pickedMonth | Elemento para definir el mes inicial o final (el mes debe estar entre 0 y 11, siendo 0 Enero, y 11 Diciembre)
pickedDay | Elemento para definir el día inicial o final
timeScale | Elemento que indica al endpoint la frecuencia en la que se debe recuperar los datos, en este caso, debe tener el valor "anual"
typeGraph | Elemnto que define la forma en la que se recuperan los datos, para este caso, debe tener el valor "1"


>Example JSON parameter:

```json
[
  {
  "dateRange":{
    "dateRangeStart":{
      "pickedYear": "2008",
      "pickedMonth": "0",
      "pickedDay": "1"
      },
    "dateRangeEnd": {
      "pickedYear": "2008",
      "pickedMonth": "0",
      "pickedDay": "3"
      }
    },
  "timeScale": "anual",
  "typeGraph": 1
  }
]
```

### Estructura de la respuesta JSON

```python
```

```javascript
```

Element | Description
------- | -----------
entidad | Elemento que muestra el nombre de la entidad contratante
anio | Elemento que muestra el año relacionado a la fecha de publicación de los procesos
avg_indicadores | Elemento que muestra la calificación promedio para la entidad en el año analizado
quartiles | Elemento que muestra el valor del cuartil respecto al elemento avg_indicadores
riesgo | Elemento que muestra el nivel de riesgo para la entidad contratante, basado en el cuartil


>Example Response:

```json
[
  {
    "anio": 2008,
    "avg_indicadores": 100,
    "entidad": "Municipalidad de Guayaquil",
    "quartiles": 3,
    "riesgo": "Bajo"
  },
  {
    "anio": 2008,
    "avg_indicadores": 61.25,
    "entidad": "GOBIERNO AUTONOMO DESCENTRALIZADO MUNICIPALIDAD DE AMBATO",
    "quartiles": 2,
    "riesgo": "Medio"
  }
]
```


## Get SIE Entidad Year Detalle

```python
import http.client

conn = http.client.HTTPSConnection("serverapiSoceIP", 5000)
payload = ''
headers = {}
conn.request("GET", "/api/soce_sie_ic_entidad_detail/[{
  \"dateRange\":{
    \"dateRangeStart\":{
      \"pickedYear\": \"2008\",
      \"pickedMonth\": \"0\",
      \"pickedDay\": \"1\"
    },
    \"dateRangeEnd\": {
      \"pickedYear\": \"2008\",
      \"pickedMonth\": \"0\",
      \"pickedDay\": \"3\"
    }
  },
  \"timeScale\": \"anual\",
  \"typeGraph\": 1
}]", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))
```

```javascript
var requestOptions = {
  method: 'GET',
  redirect: 'follow'
};

fetch("http://serverapiSoceIP:5000/api/soce_sie_ic_entidad_detail/[{\n  \"dateRange\":{\n    \"dateRangeStart\":{\n      \"pickedYear\": \"2008\",\n      \"pickedMonth\": \"0\",\n      \"pickedDay\": \"1\"\n    },\n    \"dateRangeEnd\": {\n      \"pickedYear\": \"2008\",\n      \"pickedMonth\": \"0\",\n      \"pickedDay\": \"3\"\n    }\n  },\n  \"timeScale\": \"anual\",\n  \"typeGraph\": 1\n}]", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```

Este endpoint devuelve información detallada sobre procesos SIE agrupados por Entidad, en los cuales se calcularon los indicadores individuales, obteniéndose una calificación promedio que sirve para medir el riesgo en el proceso. Los datos están agrupados anualmente, por lo tanto, la información se consolida a ese nivel de Entidad y Año.

### HTTP Request

`GET http://serverapiSoceIP/api/soce_sie_ic_entidad_detail/{{process_list}}`

### Definición de parámetros

Element | Description
--------- | -----------
process_list | Parámetro en formato JSON.


### Estructura del parámetro JSON

Element | Description
------- | -----------
dateRage | Elemento json para definir un rango de fechas
dateRangeStart | Elemento json para definir la fecha inicial
dateRangeEnd | Elemento json para definir la fecha final
pickedYear | Elemento para definir el año inicial o final
pickedMonth | Elemento para definir el mes inicial o final (el mes debe estar entre 0 y 11, siendo 0 Enero, y 11 Diciembre)
pickedDay | Elemento para definir el día inicial o final
timeScale | Elemento que indica al endpoint la frecuencia en la que se debe recuperar los datos, en este caso, debe tener el valor "anual"
typeGraph | Elemnto que define la forma en la que se recuperan los datos, para este caso, debe tener el valor "1"


>Example JSON parameter:


```json
[
  {
  "dateRange":{
    "dateRangeStart":{
      "pickedYear": "2008",
      "pickedMonth": "0",
      "pickedDay": "1"
      },
    "dateRangeEnd": {
      "pickedYear": "2008",
      "pickedMonth": "0",
      "pickedDay": "3"
      }
    },
  "timeScale": "anual",
  "typeGraph": 1
  }
]
```

### Estructura de la respuesta JSON

Element | Description
------- | -----------
anio | Elemento que muestra el año de la fecha de publicación del proceso, sobre este año se debe agrupar
entidad | Elemento que muestra el nombre de la entidad contratante
indicador_## | Elemento que muestra el valor de calificación del proceso en cada indicador. Se tienen indicadores desde el 01 hasta el 27, el valor de calificación para cada indicador individual se mide en porcentaje que va del 0 al 100%, siendo 0% una mala calificación, y 100% una buena calificación. Refierase a la metodología de cálculo descrita en el sitio web.



>Example Response:

```json
[
  {
    "anio": 2008,
    "entidad": "Gobierno Provincial del Azuay",
    "indicador_01": 100,
    "indicador_04": 100,
    "indicador_05": 100,
    "indicador_06": null,
    "indicador_09": null,
    "indicador_11": null,
    "indicador_15": 0,
    "indicador_17": 100,
    "indicador_19": null,
    "indicador_22": null,
    "indicador_25": null,
    "indicador_26": 100,
    "indicador_27": 100
  },
  {
    "anio": 2008,
    "entidad": "Universidad Agraria del Ecuador",
    "indicador_01": 100,
    "indicador_04": 100,
    "indicador_05": 100,
    "indicador_06": null,
    "indicador_09": null,
    "indicador_11": null,
    "indicador_15": 0,
    "indicador_17": 100,
    "indicador_19": null,
    "indicador_22": null,
    "indicador_25": null,
    "indicador_26": 100,
    "indicador_27": 100
  },
  {
    "anio": 2008,
    "entidad": "VICEPRESIDENCIA DE LA REPUBLICA",
    "indicador_01": 100,
    "indicador_04": 62.5,
    "indicador_05": 100,
    "indicador_06": 100,
    "indicador_09": null,
    "indicador_11": null,
    "indicador_15": 0,
    "indicador_17": 100,
    "indicador_19": null,
    "indicador_22": null,
    "indicador_25": null,
    "indicador_26": 100,
    "indicador_27": 100
  }
]
```


## Get SIE Entidad patron

```python
import http.client

conn = http.client.HTTPSConnection("serverapiSoceIP", 5000)
payload = ''
headers = {}
conn.request("GET", "/api/soce_sie_ic_entidad/[
  {
    \"dateRange\": {
      \"dateRangeStart\": {
        \"pickedYear\": \"2009\",
        \"pickedMonth\": \"11\",
        \"pickedDay\": \"1\"
      },
      \"dateRangeEnd\": {
        \"pickedYear\": \"2009\",
        \"pickedMonth\": \"11\",
        \"pickedDay\": \"3\"
      }
    },
    \"patron\": \"INSTITUTO ECUATORIANO\",
    \"typeGraph\": 2
  }
]", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))
```

```javascript
var requestOptions = {
  method: 'GET',
  redirect: 'follow'
};

fetch("http://serverapiSoceIP:5000/api/soce_sie_ic_entidad/[\n  {\n    \"dateRange\": {\n      \"dateRangeStart\": {\n        \"pickedYear\": \"2009\",\n        \"pickedMonth\": \"11\",\n        \"pickedDay\": \"1\"\n      },\n      \"dateRangeEnd\": {\n        \"pickedYear\": \"2009\",\n        \"pickedMonth\": \"11\",\n        \"pickedDay\": \"3\"\n      }\n    },\n    \"patron\": \"INSTITUTO ECUATORIANO\",\n    \"typeGraph\": 2\n  }\n]", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```

Este endpoint devuelve información detallada sobre procesos SIE agrupados por Entidad, basados en un patrón o criterio de búsqueda.

### HTTP Request

`GET http://serverapiSoceIP/api/soce_sie_ic_entidad/{{process_list}}`

### Definición de parámetros

Element | Description
--------- | -----------
process_list | Parámetro en formato JSON.


### Estructura del parámetro JSON

Element | Description
------- | -----------
typeGraph  | Elemento que indica el tipo de gráfico que se usará, en ete caso siempre es "2"
patron | Elemento que sirve para pasar un patrón o criterio de búsqueda, el mismo que se puede aplicar en: nombre de la entidad contratante
dateRage | Elemento json para definir un rango de fechas
dateRangeStart | Elemento json para definir la fecha inicial
dateRangeEnd | Elemento json para definir la fecha final
pickedYear | Elemento para definir el año inicial o final
pickedMonth | Elemento para definir el mes inicial o final (el mes debe estar entre 0 y 11, siendo 0 Enero, y 11 Diciembre)
pickedDay | Elemento para definir el día inicial o final



>Example JSON parameter:

```json
[
  {
    "dateRange": {
      "dateRangeStart": {
        "pickedYear": "2009",
        "pickedMonth": "11",
        "pickedDay": "1"
      },
      "dateRangeEnd": {
        "pickedYear": "2009",
        "pickedMonth": "11",
        "pickedDay": "3"
      }
    },
    "patron": "INSTITUTO ECUATORIANO",
    "typeGraph": 2
  }
]
```

### Estructura de la respuesta JSON

Element | Description
------- | -----------
entidad | Elemento que muestra el nombre de la entidad contratante
anio | Elemento que muestra el año de la fecha de publicación del proceso, sobre este año se debe agrupar
mes | Elemento que muestra el mes de la fecha de publicación del proceso, sobre este mes se debe agrupar
promedio | Elemento que muestra la calificación promedio para la entidad en el año y mes analizados


>Example Response:

```json
[
  {
    "anio": "2009",
    "entidad": "INSTITUTO ECUATORIANO DE SEGURIDAD SOCIAL",
    "mes": "12",
    "promedio": "58.28"
  },
  {
    "anio": "2009",
    "entidad": "INSTITUTO ECUATORIANO DE CREDITO EDUCATIVO Y BECAS",
    "mes": "12",
    "promedio": "53.85"
  },
  {
    "anio": "2009",
    "entidad": "Instituto Ecuatoriano de Seguridad Social Dirección General Loja",
    "mes": "12",
    "promedio": "46.15"
  },
  {
    "anio": "2009",
    "entidad": "INSTITUTO ECUATORIANO DE SEGURIDAD SOCIAL DIRECCION GENERAL ESMERALDAS",
    "mes": "12",
    "promedio": "46.15"
  }
]
```


## Get SIE Entidad patron Detalle

```python
import http.client

conn = http.client.HTTPSConnection("serverapiSoceIP", 5000)
payload = ''
headers = {}
conn.request("GET", "/api/soce_sie_ic_entidad_detail/[
  {
    \"dateRange\": {
      \"dateRangeStart\": {
        \"pickedYear\": \"2009\",
        \"pickedMonth\": \"11\",
        \"pickedDay\": \"1\"
      },
      \"dateRangeEnd\": {
        \"pickedYear\": \"2009\",
        \"pickedMonth\": \"11\",
        \"pickedDay\": \"3\"
      }
    },
    \"patron\": \"INSTITUTO ECUATORIANO\",
    \"typeGraph\": 2
  }
]", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))
```

```javascript
var requestOptions = {
  method: 'GET',
  redirect: 'follow'
};

fetch("http://serverapiSoceIP:5000/api/soce_sie_ic_entidad_detail/[\n  {\n    \"dateRange\": {\n      \"dateRangeStart\": {\n        \"pickedYear\": \"2009\",\n        \"pickedMonth\": \"11\",\n        \"pickedDay\": \"1\"\n      },\n      \"dateRangeEnd\": {\n        \"pickedYear\": \"2009\",\n        \"pickedMonth\": \"11\",\n        \"pickedDay\": \"3\"\n      }\n    },\n    \"patron\": \"INSTITUTO ECUATORIANO\",\n    \"typeGraph\": 2\n  }\n]", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```
Este endpoint devuelve información detallada sobre procesos SIE agrupados por Entidad, basados en un patrón o criterio de búsqueda.

### HTTP Request

`GET http://serverapiSoceIP/api/soce_sie_ic_entidad_detail/{{process_list}}`

### Definición de parámetros

Element | Description
--------- | -----------
process_list | Parámetro en formato JSON.


### Estructura del parámetro JSON

Element | Description
------- | -----------
typeGraph  | Elemento que indica el tipo de gráfico que se usará, en ete caso siempre es "2"
patron | Elemento que sirve para pasar un patrón o criterio de búsqueda, el mismo que se puede aplicar en: nombre de la entidad contratante
dateRage | Elemento json para definir un rango de fechas
dateRangeStart | Elemento json para definir la fecha inicial
dateRangeEnd | Elemento json para definir la fecha final
pickedYear | Elemento para definir el año inicial o final
pickedMonth | Elemento para definir el mes inicial o final (el mes debe estar entre 0 y 11, siendo 0 Enero, y 11 Diciembre)
pickedDay | Elemento para definir el día inicial o final


>Example JSON parameter:

```json
[
  {
    "dateRange": {
      "dateRangeStart": {
        "pickedYear": "2009",
        "pickedMonth": "11",
        "pickedDay": "1"
      },
      "dateRangeEnd": {
        "pickedYear": "2009",
        "pickedMonth": "11",
        "pickedDay": "3"
      }
    },
    "patron": "INSTITUTO ECUATORIANO",
    "typeGraph": 2
  }
]
```

### Estructura de la respuesta JSON

Element | Description
------- | -----------
entidad | Elemento que muestra el nombre de la entidad contratante
anio | Elemento que muestra el año de la fecha de publicación del proceso, sobre este año se debe agrupar
mes | Elemento que muestra el mes de la fecha de publicación del proceso, sobre este mes se debe agrupar
dia | Elemento que muestra la fecha del inicio del mes en donde se agrupan los datos
indicador_## | Elemento que muestra el valor de calificación del proceso en cada indicador. Se tienen indicadores desde el 01 hasta el 27, el valor de calificación para cada indicador individual se mide en porcentaje que va del 0 al 100%, siendo 0% una mala calificación, y 100% una buena calificación. Refierase a la metodología de cálculo descrita en el sitio web.


>Example Response:

```json
[
  {
    "anio": 2009,
    "dia": "2009-12-01",
    "entidad": "INSTITUTO ECUATORIANO DE CREDITO EDUCATIVO Y BECAS",
    "indicador_01": 100,
    "indicador_04": 100,
    "indicador_05": 100,
    "indicador_06": 100,
    "indicador_09": null,
    "indicador_11": null,
    "indicador_15": 0,
    "indicador_17": 100,
    "indicador_19": null,
    "indicador_22": null,
    "indicador_25": null,
    "indicador_26": 100,
    "indicador_27": 100,
    "mes": 12
  },
  {
    "anio": 2009,
    "dia": "2009-12-01",
    "entidad": "INSTITUTO ECUATORIANO DE LA PROPIEDAD INTELECTUAL",
    "indicador_01": 100,
    "indicador_04": null,
    "indicador_05": 100,
    "indicador_06": null,
    "indicador_09": null,
    "indicador_11": null,
    "indicador_15": 0,
    "indicador_17": 100,
    "indicador_19": null,
    "indicador_22": null,
    "indicador_25": null,
    "indicador_26": 100,
    "indicador_27": 100,
    "mes": 12
  },
  {
    "anio": 2009,
    "dia": "2009-12-01",
    "entidad": "INSTITUTO ECUATORIANO DE SEGURIDAD SOCIAL DIRECCION GENERAL ESMERALDAS",
    "indicador_01": 100,
    "indicador_04": 100,
    "indicador_05": 100,
    "indicador_06": null,
    "indicador_09": null,
    "indicador_11": null,
    "indicador_15": 0,
    "indicador_17": 100,
    "indicador_19": null,
    "indicador_22": null,
    "indicador_25": null,
    "indicador_26": 100,
    "indicador_27": 100,
    "mes": 12
  }
]
```

## Get SIE Proveedor Mes

```python
import http.client

conn = http.client.HTTPSConnection("serverapiSoceIP", 5000)
payload = ''
headers = {}
conn.request("GET", "/api/soce_sie_ic_proveedor/[{
  \"dateRange\":{
    \"dateRangeStart\":{
      \"pickedYear\": \"2008\",
      \"pickedMonth\": \"1\",
      \"pickedDay\": \"1\"
    },
    \"dateRangeEnd\": {
      \"pickedYear\": \"2008\",
      \"pickedMonth\": \"5\",
      \"pickedDay\": \"3\"
    }
  },
  \"timeScale\": \"mensual\",
  \"typeGraph\": 1
}]", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))
```

```javascript
var requestOptions = {
  method: 'GET',
  redirect: 'follow'
};

fetch("http://serverapiSoceIP:5000/api/soce_sie_ic_proveedor/[{\n  \"dateRange\":{\n    \"dateRangeStart\":{\n      \"pickedYear\": \"2008\",\n      \"pickedMonth\": \"1\",\n      \"pickedDay\": \"1\"\n    },\n    \"dateRangeEnd\": {\n      \"pickedYear\": \"2008\",\n      \"pickedMonth\": \"5\",\n      \"pickedDay\": \"3\"\n    }\n  },\n  \"timeScale\": \"mensual\",\n  \"typeGraph\": 1\n}]", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```

Este endpoint devuelve información sobre procesos SIE agrupados por Proveedor, en los cuales se calcularon los indicadores individuales, obteniéndose una calificación promedio que sirve para medir el riesgo en el proceso. Los datos están agrupados mensualmente, por lo tanto, la información se consolida a ese nivel de Proveedor y Mes.

### HTTP Request

`GET http://serverapiSoceIP/api/soce_sie_ic_proveedor/{{process_list}}`

### Definición de parámetros

Element | Description
--------- | -----------
process_list | Parámetro en formato JSON.


### Estructura del parámetro JSON

Element | Description
------- | -----------
dateRage | Elemento json para definir un rango de fechas
dateRangeStart | Elemento json para definir la fecha inicial
dateRangeEnd | Elemento json para definir la fecha final
pickedYear | Elemento para definir el año inicial o final
pickedMonth | Elemento para definir el mes inicial o final (el mes debe estar entre 0 y 11, siendo 0 Enero, y 11 Diciembre)
pickedDay | Elemento para definir el día inicial o final
timeScale | Elemento que indica al endpoint la frecuencia en la que se debe recuperar los datos, en este caso, debe tener el valor "mensual"
typeGraph | Elemnto que define la forma en la que se recuperan los datos, para este caso, debe tener el valor "1"


>Example JSON parameter:

```json
[
  {
  "dateRange":{
    "dateRangeStart":{
      "pickedYear": "2008",
      "pickedMonth": "1",
      "pickedDay": "1"
      },
    "dateRangeEnd": {
      "pickedYear": "2008",
      "pickedMonth": "5",
      "pickedDay": "3"
      }
    },
  "timeScale": "mensual",
  "typeGraph": 1
  }
]
```

### Estructura de la respuesta JSON

Element | Description
------- | -----------
ruc | Elemento que muestra el ruc del proveedor adjudicado
proveedor | Elemento que muestra el nombre del proveedor adjudicado
anio | Elemento que muestra el año relacionado a la fecha de publicación de los procesos
mes | Elemento que muestra el mes relacionado a la fecha de publicación de los procesos
avg_indicadores | Elemento que muestra la calificación promedio para el proveedor en el mes analizado
quartiles | Elemento que muestra el valor del cuartil respecto al elemento avg_indicadores
riesgo | Elemento que muestra el nivel de riesgo para el proveedor adjudicado, basado en el cuartil


>Example Response:

```json
[
  {
    "anio": 2008,
    "avg_indicadores": 66.67,
    "mes": 3,
    "proveedor": "SINERGY TEAM CIA. LTDA.",
    "quartiles": 2,
    "riesgo": "Medio",
    "ruc": "1791868935001"
  },
  {
    "anio": 2008,
    "avg_indicadores": 66.67,
    "mes": 4,
    "proveedor": "AKROS CIA. LTDA.",
    "quartiles": 2,
    "riesgo": "Medio",
    "ruc": "1791148800001"
  },
  {
    "anio": 2008,
    "avg_indicadores": 85.71,
    "mes": 6,
    "proveedor": "AKROS CIA. LTDA.",
    "quartiles": 3,
    "riesgo": "Bajo",
    "ruc": "1791148800001"
  }
]
```

## Get SIE Proveedor Mes Detalle

```python
import http.client

conn = http.client.HTTPSConnection("serverapiSoceIP", 5000)
payload = ''
headers = {}
conn.request("GET", "/api/soce_sie_ic_proveedor_detail/[{
  \"dateRange\":{
    \"dateRangeStart\":{
      \"pickedYear\": \"2008\",
      \"pickedMonth\": \"1\",
      \"pickedDay\": \"1\"
    },
    \"dateRangeEnd\": {
      \"pickedYear\": \"2008\",
      \"pickedMonth\": \"5\",
      \"pickedDay\": \"3\"
    }
  },
  \"timeScale\": \"mensual\",
  \"typeGraph\": 1
}]", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))
```

```javascript
var requestOptions = {
  method: 'GET',
  redirect: 'follow'
};

fetch("http://serverapiSoceIP:5000/api/soce_sie_ic_proveedor_detail/[{\n  \"dateRange\":{\n    \"dateRangeStart\":{\n      \"pickedYear\": \"2008\",\n      \"pickedMonth\": \"1\",\n      \"pickedDay\": \"1\"\n    },\n    \"dateRangeEnd\": {\n      \"pickedYear\": \"2008\",\n      \"pickedMonth\": \"5\",\n      \"pickedDay\": \"3\"\n    }\n  },\n  \"timeScale\": \"mensual\",\n  \"typeGraph\": 1\n}]", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```

Este endpoint devuelve información detallada sobre procesos SIE agrupados por Proveedor, en los cuales se calcularon los indicadores individuales, obteniéndose una calificación promedio que sirve para medir el riesgo en el proceso. Los datos están agrupados mensualmente, por lo tanto, la información se consolida a ese nivel de Proveedor y Mes.

### HTTP Request

`GET http://serverapiSoceIP/api/soce_sie_ic_proveedor_detail/{{process_list}}`

### Definición de parámetros

Element | Description
--------- | -----------
process_list | Parámetro en formato JSON.


### Estructura del parámetro JSON

Element | Description
------- | -----------
dateRage | Elemento json para definir un rango de fechas
dateRangeStart | Elemento json para definir la fecha inicial
dateRangeEnd | Elemento json para definir la fecha final
pickedYear | Elemento para definir el año inicial o final
pickedMonth | Elemento para definir el mes inicial o final (el mes debe estar entre 0 y 11, siendo 0 Enero, y 11 Diciembre)
pickedDay | Elemento para definir el día inicial o final
timeScale | Elemento que indica al endpoint la frecuencia en la que se debe recuperar los datos, en este caso, debe tener el valor "mensual"
typeGraph | Elemnto que define la forma en la que se recuperan los datos, para este caso, debe tener el valor "1"


>Example JSON parameter:

```json
[
  {
  "dateRange":{
    "dateRangeStart":{
      "pickedYear": "2008",
      "pickedMonth": "1",
      "pickedDay": "1"
      },
    "dateRangeEnd": {
      "pickedYear": "2008",
      "pickedMonth": "5",
      "pickedDay": "3"
      }
    },
  "timeScale": "mensual",
  "typeGraph": 1
  }
]

```

### Estructura de la respuesta JSON

Element | Description
------- | -----------
anio | Elemento que muestra el año de la fecha de publicación del proceso, sobre este año se debe agrupar
mes | Elemento que muestra el mes de la fecha de publicación del proceso, sobre este mes se debe agrupar
dia | Elemento que muestra la fecha del inicio del mes en donde se agrupan los datos
ruc | Elemento que muestra el ruc del proveedor adjudicado
proveedor | Elemento que muestra el nombre del proveedor adjudicado
indicador_## | Elemento que muestra el valor de calificación del proceso en cada indicador. Se tienen indicadores desde el 01 hasta el 27, el valor de calificación para cada indicador individual se mide en porcentaje que va del 0 al 100%, siendo 0% una mala calificación, y 100% una buena calificación. Refierase a la metodología de cálculo descrita en el sitio web.


>Example Response:

```json
[
  {
    "anio": 2008,
    "dia": "2008-06-01",
    "indicador_01": null,
    "indicador_04": null,
    "indicador_05": 100,
    "indicador_06": 100,
    "indicador_09": null,
    "indicador_11": null,
    "indicador_15": 0,
    "indicador_17": 100,
    "indicador_19": null,
    "indicador_22": null,
    "indicador_25": null,
    "indicador_26": null,
    "indicador_27": null,
    "mes": 6,
    "proveedor": "ECUATRAN SA",
    "ruc": "1890061385001"
  },
  {
    "anio": 2008,
    "dia": "2008-06-01",
    "indicador_01": null,
    "indicador_04": null,
    "indicador_05": 100,
    "indicador_06": 100,
    "indicador_09": null,
    "indicador_11": null,
    "indicador_15": 0,
    "indicador_17": 0,
    "indicador_19": null,
    "indicador_22": null,
    "indicador_25": null,
    "indicador_26": 100,
    "indicador_27": 100,
    "mes": 6,
    "proveedor": "PETROLDYG",
    "ruc": "1791730046001"
  },
  {
    "anio": 2008,
    "dia": "2008-06-01",
    "indicador_01": null,
    "indicador_04": null,
    "indicador_05": 100,
    "indicador_06": 100,
    "indicador_09": null,
    "indicador_11": null,
    "indicador_15": 0,
    "indicador_17": 100,
    "indicador_19": null,
    "indicador_22": null,
    "indicador_25": null,
    "indicador_26": 100,
    "indicador_27": 100,
    "mes": 6,
    "proveedor": "DIGITEC S.A.",
    "ruc": "1790224716001"
  }
]
```

## Get SIE Proveedor Year

```python
import http.client

conn = http.client.HTTPSConnection("serverapiSoceIP", 5000)
payload = ''
headers = {}
conn.request("GET", "/api/soce_sie_ic_proveedor/[{
  \"dateRange\":{
    \"dateRangeStart\":{
      \"pickedYear\": \"2008\",
      \"pickedMonth\": \"1\",
      \"pickedDay\": \"1\"
    },
    \"dateRangeEnd\": {
      \"pickedYear\": \"2008\",
      \"pickedMonth\": \"2\",
      \"pickedDay\": \"3\"
    }
  },
  \"timeScale\": \"anual\",
  \"typeGraph\": 1
}]", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))
```

```javascript
var requestOptions = {
  method: 'GET',
  redirect: 'follow'
};

fetch("http://serverapiSoceIP:5000/api/soce_sie_ic_proveedor/[{\n  \"dateRange\":{\n    \"dateRangeStart\":{\n      \"pickedYear\": \"2008\",\n      \"pickedMonth\": \"1\",\n      \"pickedDay\": \"1\"\n    },\n    \"dateRangeEnd\": {\n      \"pickedYear\": \"2008\",\n      \"pickedMonth\": \"2\",\n      \"pickedDay\": \"3\"\n    }\n  },\n  \"timeScale\": \"anual\",\n  \"typeGraph\": 1\n}]", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```
Este endpoint devuelve información sobre procesos SIE agrupados por Proveedor, en los cuales se calcularon los indicadores individuales, obteniéndose una calificación promedio que sirve para medir el riesgo en el proceso. Los datos están agrupados anualmente, por lo tanto, la información se consolida a ese nivel de Proveedor y Año.

### HTTP Request

`GET http://serverapiSoceIP/api/soce_sie_ic_proveedor/{{process_list}}`

### Definición de parámetros

Element | Description
--------- | -----------
process_list | Parámetro en formato JSON.


### Estructura del parámetro JSON

Element | Description
------- | -----------
dateRage | Elemento json para definir un rango de fechas
dateRangeStart | Elemento json para definir la fecha inicial
dateRangeEnd | Elemento json para definir la fecha final
pickedYear | Elemento para definir el año inicial o final
pickedMonth | Elemento para definir el mes inicial o final (el mes debe estar entre 0 y 11, siendo 0 Enero, y 11 Diciembre)
pickedDay | Elemento para definir el día inicial o final
timeScale | Elemento que indica al endpoint la frecuencia en la que se debe recuperar los datos, en este caso, debe tener el valor "anual"
typeGraph | Elemnto que define la forma en la que se recuperan los datos, para este caso, debe tener el valor "1"


>Example JSON parameter:

```json
[
  {
  "dateRange":{
    "dateRangeStart":{
      "pickedYear": "2008",
      "pickedMonth": "1",
      "pickedDay": "1"
      },
    "dateRangeEnd": {
      "pickedYear": "2008",
      "pickedMonth": "2",
      "pickedDay": "3"
      }
    },
  "timeScale": "anual",
  "typeGraph": 1
  }
]
```

### Estructura de la respuesta JSON

Element | Description
------- | -----------
ruc | Elemento que muestra el ruc del proveedor adjudicado
proveedor | Elemento que muestra el nombre del proveedor adjudicado
anio | Elemento que muestra el año relacionado a la fecha de publicación de los procesos
avg_indicadores | Elemento que muestra la calificación promedio para el proveedor en el mes analizado
quartiles | Elemento que muestra el valor del cuartil respecto al elemento avg_indicadores
riesgo | Elemento que muestra el nivel de riesgo para el proveedor adjudicado, basado en el cuartil


>Example Response:

```json
[
  {
    "anio": 2008,
    "avg_indicadores": 85.71,
    "proveedor": "MOSQUERA DOMINGUEZ ROBERTO CARLOS",
    "quartiles": 3,
    "riesgo": "Bajo",
    "ruc": "1709039877001"
  },
  {
    "anio": 2008,
    "avg_indicadores": 68.75,
    "proveedor": "PALACIOS ASITIMBAYA RAFAEL ENRIQUE",
    "quartiles": 2,
    "riesgo": "Medio",
    "ruc": "1711159796001"
  },
  {
    "anio": 2008,
    "avg_indicadores": 40,
    "proveedor": "ASSINFILT CIA. LTDA.",
    "quartiles": 1,
    "riesgo": "Alto",
    "ruc": "1791139747001"
  }
]
```

## Get SIE Proveedor Year Detalle

```python
import http.client

conn = http.client.HTTPSConnection("serverapiSoceIP", 5000)
payload = ''
headers = {}
conn.request("GET", "/api/soce_sie_ic_proveedor_detail/[{
  \"dateRange\":{
    \"dateRangeStart\":{
      \"pickedYear\": \"2008\",
      \"pickedMonth\": \"1\",
      \"pickedDay\": \"1\"
    },
    \"dateRangeEnd\": {
      \"pickedYear\": \"2008\",
      \"pickedMonth\": \"2\",
      \"pickedDay\": \"3\"
    }
  },
  \"timeScale\": \"anual\",
  \"typeGraph\": 1
}]", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))
```

```javascript
var requestOptions = {
  method: 'GET',
  redirect: 'follow'
};

fetch("http://serverapiSoceIP:5000/api/soce_sie_ic_proveedor_detail/[{\n  \"dateRange\":{\n    \"dateRangeStart\":{\n      \"pickedYear\": \"2008\",\n      \"pickedMonth\": \"1\",\n      \"pickedDay\": \"1\"\n    },\n    \"dateRangeEnd\": {\n      \"pickedYear\": \"2008\",\n      \"pickedMonth\": \"2\",\n      \"pickedDay\": \"3\"\n    }\n  },\n  \"timeScale\": \"anual\",\n  \"typeGraph\": 1\n}]", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```
Este endpoint devuelve información detallada sobre procesos SIE agrupados por Proveedor, en los cuales se calcularon los indicadores individuales, obteniéndose una calificación promedio que sirve para medir el riesgo en el proceso. Los datos están agrupados anualmente, por lo tanto, la información se consolida a ese nivel de Proveedor y Año.

### HTTP Request

`GET http://serverapiSoceIP/api/soce_sie_ic_proveedor_detail/{{process_list}}`

### Definición de parámetros

Element | Description
--------- | -----------
process_list | Parámetro en formato JSON.


### Estructura del parámetro JSON

Element | Description
------- | -----------
dateRage | Elemento json para definir un rango de fechas
dateRangeStart | Elemento json para definir la fecha inicial
dateRangeEnd | Elemento json para definir la fecha final
pickedYear | Elemento para definir el año inicial o final
pickedMonth | Elemento para definir el mes inicial o final (el mes debe estar entre 0 y 11, siendo 0 Enero, y 11 Diciembre)
pickedDay | Elemento para definir el día inicial o final
timeScale | Elemento que indica al endpoint la frecuencia en la que se debe recuperar los datos, en este caso, debe tener el valor "anual"
typeGraph | Elemnto que define la forma en la que se recuperan los datos, para este caso, debe tener el valor "1"


>Example JSON parameter:

```json
[
  {
  "dateRange":{
    "dateRangeStart":{
      "pickedYear": "2008",
      "pickedMonth": "1",
      "pickedDay": "1"
      },
    "dateRangeEnd": {
      "pickedYear": "2008",
      "pickedMonth": "2",
      "pickedDay": "3"
      }
    },
  "timeScale": "anual",
  "typeGraph": 1
  }
]
```

### Estructura de la respuesta JSON

Element | Description
------- | -----------
anio | Elemento que muestra el año de la fecha de publicación del proceso, sobre este año se debe agrupar
ruc | Elemento que muestra el ruc del proveedor adjudicado
proveedor | Elemento que muestra el nombre del proveedor adjudicado
indicador_## | Elemento que muestra el valor de calificación del proceso en cada indicador. Se tienen indicadores desde el 01 hasta el 27, el valor de calificación para cada indicador individual se mide en porcentaje que va del 0 al 100%, siendo 0% una mala calificación, y 100% una buena calificación. Refierase a la metodología de cálculo descrita en el sitio web.


>Example Response:

```json
[
  {
    "anio": 2008,
    "indicador_01": 100,
    "indicador_04": 0,
    "indicador_05": 100,
    "indicador_06": 50,
    "indicador_09": null,
    "indicador_11": null,
    "indicador_15": 0,
    "indicador_17": 0,
    "indicador_19": null,
    "indicador_22": null,
    "indicador_25": null,
    "indicador_26": null,
    "indicador_27": null,
    "proveedor": "IASA S.A.",
    "ruc": "0990011109001"
  },
  {
    "anio": 2008,
    "indicador_01": 100,
    "indicador_04": 0,
    "indicador_05": 100,
    "indicador_06": 100,
    "indicador_09": null,
    "indicador_11": null,
    "indicador_15": 0,
    "indicador_17": 100,
    "indicador_19": null,
    "indicador_22": null,
    "indicador_25": null,
    "indicador_26": null,
    "indicador_27": null,
    "proveedor": "IMPORTADORA PAREDES VELASCO IMPARVE S.A",
    "ruc": "1791769198001"
  },
  {
    "anio": 2008,
    "indicador_01": 100,
    "indicador_04": 2,
    "indicador_05": 100,
    "indicador_06": 0,
    "indicador_09": null,
    "indicador_11": null,
    "indicador_15": 0,
    "indicador_17": 100,
    "indicador_19": null,
    "indicador_22": null,
    "indicador_25": null,
    "indicador_26": null,
    "indicador_27": null,
    "proveedor": "AUTOMEKANO CIA. LTDA.",
    "ruc": "1891715664001"
  }
]
```

## Get SIE Proveedor Patron

```python
import http.client

conn = http.client.HTTPSConnection("serverapiSoceIP", 5000)
payload = ''
headers = {}
conn.request("GET", "/api/soce_sie_ic_proveedor/[
  {
    \"dateRange\": {
      \"dateRangeStart\": {
        \"pickedYear\": \"2009\",
        \"pickedMonth\": \"11\",
        \"pickedDay\": \"1\"
      },
      \"dateRangeEnd\": {
        \"pickedYear\": \"2010\",
        \"pickedMonth\": \"0\",
        \"pickedDay\": \"3\"
      }
    },
    \"patron\": \"JIMENEZ\",
    \"typeGraph\": 2
  }
]", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))
```

```javascript
var requestOptions = {
  method: 'GET',
  redirect: 'follow'
};

fetch("http://serverapiSoceIP:5000/api/soce_sie_ic_proveedor/[\n  {\n    \"dateRange\": {\n      \"dateRangeStart\": {\n        \"pickedYear\": \"2009\",\n        \"pickedMonth\": \"11\",\n        \"pickedDay\": \"1\"\n      },\n      \"dateRangeEnd\": {\n        \"pickedYear\": \"2010\",\n        \"pickedMonth\": \"0\",\n        \"pickedDay\": \"3\"\n      }\n    },\n    \"patron\": \"JIMENEZ\",\n    \"typeGraph\": 2\n  }\n]", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```

Este endpoint devuelve información detallada sobre procesos SIE agrupados por Proveedor, basados en un patrón o criterio de búsqueda.

### HTTP Request

`GET http://serverapiSoceIP/api/soce_sie_ic_proveedor/{{process_list}}`

### Definición de parámetros

Element | Description
--------- | -----------
process_list | Parámetro en formato JSON.


### Estructura del parámetro JSON

Element | Description
------- | -----------
typeGraph | Elemento que indica el tipo de gráfico que se usará, en ete caso siempre es "2"
patron | Elemento que sirve para pasar un patrón o criterio de búsqueda, el mismo que se puede aplicar en: ruc o nombre del proveedor
dateRage | Elemento json para definir un rango de fechas
dateRangeStart | Elemento json para definir la fecha inicial
dateRangeEnd | Elemento json para definir la fecha final
pickedYear | Elemento para definir el año inicial o final
pickedMonth | Elemento para definir el mes inicial o final (el mes debe estar entre 0 y 11, siendo 0 Enero, y 11 Diciembre)
pickedDay | Elemento para definir el día inicial o final


>Example JSON parameter:

```json
[
  {
    "dateRange": {
      "dateRangeStart": {
        "pickedYear": "2009",
        "pickedMonth": "11",
        "pickedDay": "1"
      },
      "dateRangeEnd": {
        "pickedYear": "2010",
        "pickedMonth": "0",
        "pickedDay": "3"
      }
    },
    "patron": "JIMENEZ",
    "typeGraph": 2
  }
]
```

### Estructura de la respuesta JSON

Element | Description
------- | -----------
ruc | Elemento que muestra el ruc del proveedor adjudicado
proveedor | Elemento que muestra el nombre del proveedor adjudicado
anio | Elemento que muestra el año de la fecha de publicación del proceso, sobre este año se debe agrupar
mes | Elemento que muestra el mes de la fecha de publicación del proceso, sobre este mes se debe agrupar
promedio | Elemento que muestra la calificación promedio para el proveedor en el año y mes analizados


>Example Response:

```json
[
  {
    "anio": "2009",
    "mes": "12",
    "promedio": "44.23",
    "proveedor": "JIMENEZ AGUIRRE RUBEN DARIO",
    "ruc": "1710534874001"
  },
  {
    "anio": "2009",
    "mes": "12",
    "promedio": "28.85",
    "proveedor": "JIMENEZ ORTIZ MARINA ISABEL",
    "ruc": "0103264792001"
  },
  {
    "anio": "2010",
    "mes": "1",
    "promedio": "44.31",
    "proveedor": "JIMENEZ AGUIRRE RUBEN DARIO",
    "ruc": "1710534874001"
  },
  {
    "anio": "2010",
    "mes": "1",
    "promedio": "44.23",
    "proveedor": "JIMENEZ BRITO LUIS CESARIO",
    "ruc": "0700910284001"
  }
]
```


## Get SIE Proveedor Patron Detalle

```python
import http.client

conn = http.client.HTTPSConnection("serverapiSoceIP", 5000)
payload = ''
headers = {}
conn.request("GET", "/api/soce_sie_ic_proveedor_detail/[
  {
    \"dateRange\": {
      \"dateRangeStart\": {
        \"pickedYear\": \"2009\",
        \"pickedMonth\": \"11\",
        \"pickedDay\": \"1\"
      },
      \"dateRangeEnd\": {
        \"pickedYear\": \"2010\",
        \"pickedMonth\": \"0\",
        \"pickedDay\": \"3\"
      }
    },
    \"patron\": \"JIMENEZ\",
    \"typeGraph\": 2
  }
]", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))
```

```javascript
var requestOptions = {
  method: 'GET',
  redirect: 'follow'
};

fetch("http://serverapiSoceIP:5000/api/soce_sie_ic_proveedor_detail/[\n  {\n    \"dateRange\": {\n      \"dateRangeStart\": {\n        \"pickedYear\": \"2009\",\n        \"pickedMonth\": \"11\",\n        \"pickedDay\": \"1\"\n      },\n      \"dateRangeEnd\": {\n        \"pickedYear\": \"2010\",\n        \"pickedMonth\": \"0\",\n        \"pickedDay\": \"3\"\n      }\n    },\n    \"patron\": \"JIMENEZ\",\n    \"typeGraph\": 2\n  }\n]", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```

Este endpoint devuelve información detallada sobre procesos SIE agrupados por Proveedor, basados en un patrón o criterio de búsqueda.

### HTTP Request

`GET http://serverapiSoceIP/api/soce_sie_ic_proveedor_detail/{{process_list}}`

### Definición de parámetros

Element | Description
--------- | -----------
process_list | Parámetro en formato JSON.


### Estructura del parámetro JSON

Element | Description
------- | -----------
typeGraph | Elemento que indica el tipo de gráfico que se usará, en ete caso siempre es "2"
patron | Elemento que sirve para pasar un patrón o criterio de búsqueda, el mismo que se puede aplicar en: ruc o nombre del proveedor
dateRage | Elemento json para definir un rango de fechas
dateRangeStart | Elemento json para definir la fecha inicial
dateRangeEnd | Elemento json para definir la fecha final
pickedYear | Elemento para definir el año inicial o final
pickedMonth | Elemento para definir el mes inicial o final (el mes debe estar entre 0 y 11, siendo 0 Enero, y 11 Diciembre)
pickedDay | Elemento para definir el día inicial o final


>Example JSON parameter:

```json
[
  {
    "dateRange": {
      "dateRangeStart": {
        "pickedYear": "2009",
        "pickedMonth": "11",
        "pickedDay": "1"
      },
      "dateRangeEnd": {
        "pickedYear": "2010",
        "pickedMonth": "0",
        "pickedDay": "3"
      }
    },
    "patron": "JIMENEZ",
    "typeGraph": 2
  }
]
```

### Estructura de la respuesta JSON

Element | Description
------- | -----------
ruc | Elemento que muestra el ruc del proveedor adjudicado
proveedor | Elemento que muestra el nombre del proveedor adjudicado
anio | Elemento que muestra el año de la fecha de publicación del proceso, sobre este año se debe agrupar
mes | Elemento que muestra el mes de la fecha de publicación del proceso, sobre este mes se debe agrupar
dia | Elemento que muestra la fecha del inicio del mes en donde se agrupan los datos
indicador_## | Elemento que muestra el valor de calificación del proceso en cada indicador. Se tienen indicadores desde el 01 hasta el 27, el valor de calificación para cada indicador individual se mide en porcentaje que va del 0 al 100%, siendo 0% una mala calificación, y 100% una buena calificación. Refierase a la metodología de cálculo descrita en el sitio web.


>Example Response:

```json
[
  {
    "anio": 2009,
    "dia": "2009-12-01",
    "indicador_01": 100,
    "indicador_04": 0,
    "indicador_05": 100,
    "indicador_06": 75,
    "indicador_09": null,
    "indicador_11": null,
    "indicador_15": 0,
    "indicador_17": 100,
    "indicador_19": null,
    "indicador_22": null,
    "indicador_25": null,
    "indicador_26": null,
    "indicador_27": null,
    "mes": 12,
    "proveedor": "JIMENEZ ORTIZ MARINA ISABEL",
    "ruc": "0103264792001"
  },
  {
    "anio": 2010,
    "dia": "2010-01-01",
    "indicador_01": 100,
    "indicador_04": 0,
    "indicador_05": 100,
    "indicador_06": 75,
    "indicador_09": null,
    "indicador_11": null,
    "indicador_15": 0,
    "indicador_17": 100,
    "indicador_19": null,
    "indicador_22": null,
    "indicador_25": null,
    "indicador_26": 100,
    "indicador_27": 100,
    "mes": 1,
    "proveedor": "JIMENEZ BRITO LUIS CESARIO",
    "ruc": "0700910284001"
  },
  {
    "anio": 2009,
    "dia": "2009-12-01",
    "indicador_01": 100,
    "indicador_04": null,
    "indicador_05": 100,
    "indicador_06": 75,
    "indicador_09": null,
    "indicador_11": null,
    "indicador_15": 0,
    "indicador_17": 100,
    "indicador_19": null,
    "indicador_22": null,
    "indicador_25": null,
    "indicador_26": 100,
    "indicador_27": 100,
    "mes": 12,
    "proveedor": "JIMENEZ AGUIRRE RUBEN DARIO",
    "ruc": "1710534874001"
  },
  {
    "anio": 2010,
    "dia": "2010-01-01",
    "indicador_01": 100,
    "indicador_04": 1,
    "indicador_05": 100,
    "indicador_06": 75,
    "indicador_09": null,
    "indicador_11": null,
    "indicador_15": 0,
    "indicador_17": 100,
    "indicador_19": null,
    "indicador_22": null,
    "indicador_25": null,
    "indicador_26": 100,
    "indicador_27": 100,
    "mes": 1,
    "proveedor": "JIMENEZ AGUIRRE RUBEN DARIO",
    "ruc": "1710534874001"
  }
]
```


# GEN

## Get GEN Proceso Diariamente

```python
import http.client

conn = http.client.HTTPSConnection("serverapiSoceIP", 5000)
payload = ''
headers = {}
conn.request("GET", "/api/soce_gen_ic_proceso/[
  {
    \"dateRange\": {
      \"dateRangeStart\": {
        \"pickedYear\": \"2008\",
        \"pickedMonth\": \"3\",
        \"pickedDay\": \"24\"
      },
      \"dateRangeEnd\": {
        \"pickedYear\": \"2008\",
        \"pickedMonth\": \"5\",
        \"pickedDay\": \"14\"
      }
    },
    \"monto\": \"true\",
    \"processStage\": \"adjudicada\",
    \"purchaseType\": [
      \"bien\",
      \"bien_y_servicio\",
      \"servicio\",
      \"farmacos\"
    ],
    \"timeScale\": \"diario\",
    \"typeGraph\": 1
  }
]", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))
```

```javascript
var requestOptions = {
  method: 'GET',
  redirect: 'follow'
};

fetch("http://serverapiSoceIP:5000/api/soce_gen_ic_proceso/[\n  {\n    \"dateRange\": {\n      \"dateRangeStart\": {\n        \"pickedYear\": \"2008\",\n        \"pickedMonth\": \"3\",\n        \"pickedDay\": \"24\"\n      },\n      \"dateRangeEnd\": {\n        \"pickedYear\": \"2008\",\n        \"pickedMonth\": \"5\",\n        \"pickedDay\": \"14\"\n      }\n    },\n    \"monto\": \"true\",\n    \"processStage\": \"adjudicada\",\n    \"purchaseType\": [\n      \"bien\",\n      \"bien_y_servicio\",\n      \"servicio\",\n      \"farmacos\"\n    ],\n    \"timeScale\": \"diario\",\n    \"typeGraph\": 1\n  }\n]", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```

Este endpoint entrega procesos GEN, en los cuales se calcularon los indicadores individuales, obteniéndose una calificación promedio que sirve para medir el riesgo en el proceso. Los datos están agrupados diariamente, por lo tanto, la información se consolida a ese nivel de agrupación.

### HTTP Request

`GET http://serverapiSoceIP/api/soce_gen_ic_proceso/{{process_list}}`

### Definición de parámetros

Element | Description
--------- | -----------
process_list | Parámetro en formato JSON.


### Estructura del parámetro JSON

Element | Description
------- | -----------
dateRage | Elemento json para definir un rango de fechas
dateRangeStart | Elemento json para definir la fecha inicial
dateRangeEnd | Elemento json para definir la fecha final
pickedYear | Elemento para definir el año inicial o final
pickedMonth | Elemento para definir el mes inicial o final (el mes debe estar entre 0 y 11, siendo 0 Enero, y 11 Diciembre)
pickedDay | Elemento para definir el día inicial o final
monto | Elemento que puede ser TRUE o FALSE, en el caso de estar marcado se muestra información acerca del monto presupuestado del proceso
processStage | Elemento que contiene el estado del proceso. 
purchaseType | Elemento array que contiene el tipo de compra que puede contener estos valores: bien, bien_y_servicio, servicio, farmacos
timeScale | Elemento que indica al endpoint la frecuencia en la que se debe recuperar los datos, en este caso, debe tener el valor "diario"
typeGraph | Elemnto que define la forma en la que se recuperan los datos, para este caso, debe tener el valor "1"


>Example JSON parameter:

```json
[
  {
    "dateRange": {
      "dateRangeStart": {
        "pickedYear": "2008",
        "pickedMonth": "3",
        "pickedDay": "24"
      },
      "dateRangeEnd": {
        "pickedYear": "2008",
        "pickedMonth": "5",
        "pickedDay": "14"
      }
    },
    "monto": "true",
    "processStage": "adjudicada",
    "purchaseType": [
      "bien",
      "bien_y_servicio",
      "servicio",
      "farmacos"
    ],
    "timeScale": "diario",
    "typeGraph": 1
  }
]
```

### Estructura de la respuesta JSON

Element | Description
------- | -----------
fecha_publicacion | Elemento que muestra la fecha de publicación
avg_indicadores | Elemento que muestra la calificación para esa fecha, este es el valor del índice compuesto que se calcula sacando el promedio por cada proceso y sus respectivos indicadores individuales, este valor está entre 0 y 100%
quartiles | Elemento que indica el valor del cuartial relacionado a la calificación obtenida en el rango de fechas
riesgo | Elemento que indica el riesgo para la fecha de publicación en la que se agrupan los resultados
std_monto_presupuestado | Elemento que muestra la desviación estándar sobre el monto presupuestado de los procesos considerados en el rango de fechas


>Example Response:

```json
[
  {
    "avg_indicadores": 48.5,
    "fecha_publicacion": "2008-04-24",
    "quartiles": 1,
    "riesgo": "Alto",
    "std_monto_presupuestado": 3961.97
  },
  {
    "avg_indicadores": 40.5,
    "fecha_publicacion": "2008-04-25",
    "quartiles": 1,
    "riesgo": "Alto",
    "std_monto_presupuestado": 274.15
  },
  {
    "avg_indicadores": 50.14,
    "fecha_publicacion": "2008-04-28",
    "quartiles": 2,
    "riesgo": "Medio",
    "std_monto_presupuestado": 55283.37
  },
  {
    "avg_indicadores": 49.1,
    "fecha_publicacion": "2008-04-29",
    "quartiles": 1,
    "riesgo": "Alto",
    "std_monto_presupuestado": 1282.18
  }
]
```


## Get GEN Proceso Diariamente Detalle

```python
import http.client

conn = http.client.HTTPSConnection("serverapiSoceIP", 5000)
payload = ''
headers = {}
conn.request("GET", "/api/soce_gen_ic_proceso_detail/[
  {
    \"dateRange\": {
      \"dateRangeStart\": {
        \"pickedYear\": \"2008\",
        \"pickedMonth\": \"3\",
        \"pickedDay\": \"24\"
      },
      \"dateRangeEnd\": {
        \"pickedYear\": \"2008\",
        \"pickedMonth\": \"5\",
        \"pickedDay\": \"14\"
      }
    },
    \"monto\": \"true\",
    \"processStage\": \"adjudicada\",
    \"purchaseType\": [
      \"bien\",
      \"bien_y_servicio\",
      \"servicio\",
      \"farmacos\"
    ],
    \"timeScale\": \"diario\",
    \"typeGraph\": 1
  }
]", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))
```

```javascript
var requestOptions = {
  method: 'GET',
  redirect: 'follow'
};

fetch("http://serverapiSoceIP:5000/api/soce_gen_ic_proceso_detail/[\n  {\n    \"dateRange\": {\n      \"dateRangeStart\": {\n        \"pickedYear\": \"2008\",\n        \"pickedMonth\": \"3\",\n        \"pickedDay\": \"24\"\n      },\n      \"dateRangeEnd\": {\n        \"pickedYear\": \"2008\",\n        \"pickedMonth\": \"5\",\n        \"pickedDay\": \"14\"\n      }\n    },\n    \"monto\": \"true\",\n    \"processStage\": \"adjudicada\",\n    \"purchaseType\": [\n      \"bien\",\n      \"bien_y_servicio\",\n      \"servicio\",\n      \"farmacos\"\n    ],\n    \"timeScale\": \"diario\",\n    \"typeGraph\": 1\n  }\n]", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```
Este endpoint entrega procesos GEN con información detallada, la misma que puede ser usada para calcular la calificación promedio del proceso, basada en la calificación de cada indicador individual.

### HTTP Request

`GET http://serverapiSoceIP/api/soce_gen_ic_proceso_detail/{{process_list}}`

### Definición de parámetros

Element | Description
--------- | -----------
process_list | Parámetro en formato JSON.


### Estructura del parámetro JSON

Element | Description
------- | -----------
dateRage | Elemento json para definir un rango de fechas
dateRangeStart | Elemento json para definir la fecha inicial
dateRangeEnd | Elemento json para definir la fecha final
pickedYear | Elemento para definir el año inicial o final
pickedMonth | Elemento para definir el mes inicial o final (el mes debe estar entre 0 y 11, siendo 0 Enero, y 11 Diciembre)
pickedDay | Elemento para definir el día inicial o final
monto | Elemento que puede ser TRUE o FALSE, en el caso de estar marcado se muestra información acerca del monto presupuestado del proceso
processStage | Elemento que contiene el estado del proceso. 
purchaseType | Elemento array que contiene el tipo de compra que puede contener estos valores: bien, bien_y_servicio, servicio, farmacos
timeScale | Elemento que indica al endpoint la frecuencia en la que se debe recuperar los datos, en este caso, debe tener el valor "diario"
typeGraph | Elemnto que define la forma en la que se recuperan los datos, para este caso, debe tener el valor "1"


>Example JSON parameter:

```json
[
  {
    "dateRange": {
      "dateRangeStart": {
        "pickedYear": "2008",
        "pickedMonth": "3",
        "pickedDay": "24"
      },
      "dateRangeEnd": {
        "pickedYear": "2008",
        "pickedMonth": "5",
        "pickedDay": "14"
      }
    },
    "monto": "true",
    "processStage": "adjudicada",
    "purchaseType": [
      "bien",
      "bien_y_servicio",
      "servicio",
      "farmacos"
    ],
    "timeScale": "diario",
    "typeGraph": 1
  }
]
```

### Estructura de la respuesta JSON

Element | Description
------- | -----------
fecha_publicacion | Elemento que muestra la fecha de publicación
indicador_## | Elemento que muestra el valor de calificación del proceso en cada indicador. Se tienen indicadores desde el 01 hasta el 29, el valor 1 significafa que le proceso cumple con la ficha metodológica de cálculo, es decir es un proceso "bueno", mientras que el valor de 0 indica que ese proceso no cumplió la ficha, por lo tanto su calificación es "mala". Para el caso de valores NULOS o null, significa que el proceso no participó en el análisis o cálculo para un particular indicador.
monto_presupuestado | Elemento que muestra el monto presupuestado de los procesos considerados en el rango de fechas


>Example Response:

```json
[
  {
    "codigo": "COM-SEPT-075-2008",
    "fecha_publicacion": "2008-05-08",
    "indicador_01": 0,
    "indicador_02": 0,
    "indicador_04": null,
    "indicador_05": 1,
    "indicador_06": null,
    "indicador_09": 0,
    "indicador_11": 0,
    "indicador_12": 1,
    "indicador_18": 1,
    "indicador_19": null,
    "indicador_22": 0,
    "indicador_25": 0,
    "indicador_27": null,
    "indicador_28": 1,
    "indicador_29": 1,
    "monto_presupuestado": 512
  },
  {
    "codigo": "COM-SEP-076-2008",
    "fecha_publicacion": "2008-04-25",
    "indicador_01": 0,
    "indicador_02": 0,
    "indicador_04": null,
    "indicador_05": 1,
    "indicador_06": null,
    "indicador_09": 0,
    "indicador_11": 0,
    "indicador_12": 0,
    "indicador_18": 1,
    "indicador_19": null,
    "indicador_22": 0,
    "indicador_25": 0,
    "indicador_27": null,
    "indicador_28": 1,
    "indicador_29": 1,
    "monto_presupuestado": 528
  }
]
```

## Get GEN Proceso Mesual

```python
import http.client

conn = http.client.HTTPSConnection("serverapiSoceIP", 5000)
payload = ''
headers = {}
conn.request("GET", "/api/soce_gen_ic_proceso/[
  {
    \"dateRange\": {
      \"dateRangeStart\": {
        \"pickedYear\": \"2008\",
        \"pickedMonth\": \"3\",
        \"pickedDay\": \"24\"
      },
      \"dateRangeEnd\": {
        \"pickedYear\": \"2008\",
        \"pickedMonth\": \"5\",
        \"pickedDay\": \"14\"
      }
    },
    \"monto\": \"true\",
    \"processStage\": \"adjudicada\",
    \"purchaseType\": [
      \"bien\",
      \"bien_y_servicio\",
      \"servicio\",
      \"farmacos\"
    ],
    \"timeScale\": \"mensual\",
    \"typeGraph\": 1
  }
]", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))
```

```javascript
var requestOptions = {
  method: 'GET',
  redirect: 'follow'
};

fetch("http://serverapiSoceIP:5000/api/soce_gen_ic_proceso/[\n  {\n    \"dateRange\": {\n      \"dateRangeStart\": {\n        \"pickedYear\": \"2008\",\n        \"pickedMonth\": \"3\",\n        \"pickedDay\": \"24\"\n      },\n      \"dateRangeEnd\": {\n        \"pickedYear\": \"2008\",\n        \"pickedMonth\": \"5\",\n        \"pickedDay\": \"14\"\n      }\n    },\n    \"monto\": \"true\",\n    \"processStage\": \"adjudicada\",\n    \"purchaseType\": [\n      \"bien\",\n      \"bien_y_servicio\",\n      \"servicio\",\n      \"farmacos\"\n    ],\n    \"timeScale\": \"mensual\",\n    \"typeGraph\": 1\n  }\n]", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```
Este endpoint devuelve información sobre procesos GEN, en los cuales se calcularon los indicadores individuales, obteniéndose una calificación promedio que sirve para medir el riesgo en el proceso. Los datos están agrupados mensualmente, por lo tanto, la información se consolida a ese nivel de agrupación.

### HTTP Request

`GET http://serverapiSoceIP/api/soce_gen_ic_proceso/{{process_list}}`

### Definición de parámetros

Element | Description
--------- | -----------
process_list | Parámetro en formato JSON.


### Estructura del parámetro JSON

Element | Description
------- | -----------
dateRage | Elemento json para definir un rango de fechas
dateRangeStart | Elemento json para definir la fecha inicial
dateRangeEnd | Elemento json para definir la fecha final
pickedYear | Elemento para definir el año inicial o final
pickedMonth | Elemento para definir el mes inicial o final (el mes debe estar entre 0 y 11, siendo 0 Enero, y 11 Diciembre)
pickedDay | Elemento para definir el día inicial o final
monto | Elemento que puede ser TRUE o FALSE, en el caso de estar marcado se muestra información acerca del monto presupuestado del proceso
processStage | Elemento que contiene el estado del proceso. 
purchaseType | Elemento array que contiene el tipo de compra que puede contener estos valores: bien, bien_y_servicio, servicio, farmacos
timeScale | Elemento que indica al endpoint la frecuencia en la que se debe recuperar los datos, en este caso, debe tener el valor "mensual"
typeGraph | Elemnto que define la forma en la que se recuperan los datos, para este caso, debe tener el valor "1"


>Example JSON parameter:

```json
[
  {
    "dateRange": {
      "dateRangeStart": {
        "pickedYear": "2008",
        "pickedMonth": "3",
        "pickedDay": "24"
      },
      "dateRangeEnd": {
        "pickedYear": "2008",
        "pickedMonth": "5",
        "pickedDay": "14"
      }
    },
    "monto": "true",
    "processStage": "adjudicada",
    "purchaseType": [
      "bien",
      "bien_y_servicio",
      "servicio",
      "farmacos"
    ],
    "timeScale": "mensual",
    "typeGraph": 1
  }
]
```

### Estructura de la respuesta JSON

Element | Description
------- | -----------
anio | Elemento que muestra el año de la fecha de publicación de un proceso
mes | Elemento que muestra el mes de la fecha de publicación de un proceso
avg_indicadores | Elemento que muestra la calificación para ese mes, este es el valor del índice compuesto que se calcula sacando el promedio por cada proceso y sus respectivos indicadores individuales, este valor está entre 0 y 100%
quartiles | Elemento que indica el valor del cuartial relacionado a la calificación obtenida en el rango de fechas
riesgo | Elemento que indica el riesgo para mes en la que se agrupan los resultados
std_monto_presupuestado | Elemento que muestra el monto presupuestado de los procesos considerados en el rango de fechas


>Example Response:

```json
[
  {
    "anio": 2008,
    "avg_indicadores": 49.17,
    "mes": 4,
    "quartiles": 1,
    "riesgo": "Alto",
    "std_monto_presupuestado": 18618.53
  },
  {
    "anio": 2008,
    "avg_indicadores": 47.68,
    "mes": 5,
    "quartiles": 1,
    "riesgo": "Alto",
    "std_monto_presupuestado": 5560.76
  },
  {
    "anio": 2008,
    "avg_indicadores": 47.06,
    "mes": 6,
    "quartiles": 1,
    "riesgo": "Alto",
    "std_monto_presupuestado": 5653.17
  }
]
```


## Get GEN Proceso Mesual Detalle

```python
import http.client

conn = http.client.HTTPSConnection("serverapiSoceIP", 5000)
payload = ''
headers = {}
conn.request("GET", "/api/soce_gen_ic_proceso_detail/[
  {
    \"dateRange\": {
      \"dateRangeStart\": {
        \"pickedYear\": \"2008\",
        \"pickedMonth\": \"3\",
        \"pickedDay\": \"24\"
      },
      \"dateRangeEnd\": {
        \"pickedYear\": \"2008\",
        \"pickedMonth\": \"5\",
        \"pickedDay\": \"14\"
      }
    },
    \"monto\": \"true\",
    \"processStage\": \"adjudicada\",
    \"purchaseType\": [
      \"bien\",
      \"bien_y_servicio\",
      \"servicio\",
      \"farmacos\"
    ],
    \"timeScale\": \"mensual\",
    \"typeGraph\": 1
  }
]", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))
```

```javascript
var requestOptions = {
  method: 'GET',
  redirect: 'follow'
};

fetch("http://serverapiSoceIP:5000/api/soce_gen_ic_proceso_detail/[\n  {\n    \"dateRange\": {\n      \"dateRangeStart\": {\n        \"pickedYear\": \"2008\",\n        \"pickedMonth\": \"3\",\n        \"pickedDay\": \"24\"\n      },\n      \"dateRangeEnd\": {\n        \"pickedYear\": \"2008\",\n        \"pickedMonth\": \"5\",\n        \"pickedDay\": \"14\"\n      }\n    },\n    \"monto\": \"true\",\n    \"processStage\": \"adjudicada\",\n    \"purchaseType\": [\n      \"bien\",\n      \"bien_y_servicio\",\n      \"servicio\",\n      \"farmacos\"\n    ],\n    \"timeScale\": \"mensual\",\n    \"typeGraph\": 1\n  }\n]", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```
Este endpoint devuelve información detallada sobre procesos GEN, la misma que puede ser usada para calcular la calificación promedio de los procesos considereados en frecuencia mensual.

### HTTP Request

`GET http://serverapiSoceIP/api/soce_gen_ic_proceso_detail/{{process_list}}`

### Definición de parámetros

Element | Description
--------- | -----------
process_list | Parámetro en formato JSON.


### Estructura del parámetro JSON

Element | Description
------- | -----------
dateRage | Elemento json para definir un rango de fechas
dateRangeStart | Elemento json para definir la fecha inicial
dateRangeEnd | Elemento json para definir la fecha final
pickedYear | Elemento para definir el año inicial o final
pickedMonth | Elemento para definir el mes inicial o final (el mes debe estar entre 0 y 11, siendo 0 Enero, y 11 Diciembre)
pickedDay | Elemento para definir el día inicial o final
monto | Elemento que puede ser TRUE o FALSE, en el caso de estar marcado se muestra información acerca del monto presupuestado del proceso
processStage | Elemento que contiene el estado del proceso. 
purchaseType | Elemento array que contiene el tipo de compra que puede contener estos valores: bien, bien_y_servicio, servicio, farmacos
timeScale | Elemento que indica al endpoint la frecuencia en la que se debe recuperar los datos, en este caso, debe tener el valor "mensual"
typeGraph | Elemnto que define la forma en la que se recuperan los datos, para este caso, debe tener el valor "1"


>Example JSON parameter:

```json
[
  {
    "dateRange": {
      "dateRangeStart": {
        "pickedYear": "2008",
        "pickedMonth": "3",
        "pickedDay": "24"
      },
      "dateRangeEnd": {
        "pickedYear": "2008",
        "pickedMonth": "5",
        "pickedDay": "14"
      }
    },
    "monto": "true",
    "processStage": "adjudicada",
    "purchaseType": [
      "bien",
      "bien_y_servicio",
      "servicio",
      "farmacos"
    ],
    "timeScale": "mensual",
    "typeGraph": 1
  }
]
```

### Estructura de la respuesta JSON

Element | Description
------- | -----------
fecha_publicacion | Elemento que muestra la fecha de publicación, para este caso, el mes es el objetivo del análisis
codigo | Elemento que muestra el código del Proceso
indicador_## | Elemento que muestra el valor de calificación del proceso en cada indicador. Se tienen indicadores desde el 01 hasta el 29, el valor 1 significafa que le proceso cumple con la ficha metodológica de cálculo, es decir es un proceso "bueno", mientras que el valor de 0 indica que ese proceso no cumplió la ficha, por lo tanto su calificación es "mala". Para el caso de valores NULOS o null, significa que el proceso no participó en el análisis o cálculo para un particular indicador.
monto_presupuestado | Elemento que muestra el monto presupuestado de los procesos considerados en el rango de fechas


>Example Response:

```json
[
  {
    "codigo": "COM-SEPT-075-2008",
    "fecha_publicacion": "2008-05-08",
    "indicador_01": 0,
    "indicador_02": 0,
    "indicador_04": null,
    "indicador_05": 1,
    "indicador_06": null,
    "indicador_09": 0,
    "indicador_11": 0,
    "indicador_12": 1,
    "indicador_18": 1,
    "indicador_19": null,
    "indicador_22": 0,
    "indicador_25": 0,
    "indicador_27": null,
    "indicador_28": 1,
    "indicador_29": 1,
    "monto_presupuestado": 512
  },
  {
    "codigo": "COM-SEP-076-2008",
    "fecha_publicacion": "2008-04-25",
    "indicador_01": 0,
    "indicador_02": 0,
    "indicador_04": null,
    "indicador_05": 1,
    "indicador_06": null,
    "indicador_09": 0,
    "indicador_11": 0,
    "indicador_12": 0,
    "indicador_18": 1,
    "indicador_19": null,
    "indicador_22": 0,
    "indicador_25": 0,
    "indicador_27": null,
    "indicador_28": 1,
    "indicador_29": 1,
    "monto_presupuestado": 528
  }
]
```

## Get GEN Proceso Year

```python
import http.client

conn = http.client.HTTPSConnection("serverapiSoceIP", 5000)
payload = ''
headers = {}
conn.request("GET", "/api/soce_gen_ic_proceso/[
  {
    \"dateRange\": {
      \"dateRangeStart\": {
        \"pickedYear\": \"2008\",
        \"pickedMonth\": \"3\",
        \"pickedDay\": \"24\"
      },
      \"dateRangeEnd\": {
        \"pickedYear\": \"2010\",
        \"pickedMonth\": \"5\",
        \"pickedDay\": \"14\"
      }
    },
    \"monto\": \"true\",
    \"processStage\": \"adjudicada\",
    \"purchaseType\": [
      \"bien\",
      \"bien_y_servicio\",
      \"servicio\",
      \"farmacos\"
    ],
    \"timeScale\": \"anual\",
    \"typeGraph\": 1
  }
]", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))
```

```javascript
var requestOptions = {
  method: 'GET',
  redirect: 'follow'
};

fetch("http://serverapiSoceIP:5000/api/soce_gen_ic_proceso/[\n  {\n    \"dateRange\": {\n      \"dateRangeStart\": {\n        \"pickedYear\": \"2008\",\n        \"pickedMonth\": \"3\",\n        \"pickedDay\": \"24\"\n      },\n      \"dateRangeEnd\": {\n        \"pickedYear\": \"2010\",\n        \"pickedMonth\": \"5\",\n        \"pickedDay\": \"14\"\n      }\n    },\n    \"monto\": \"true\",\n    \"processStage\": \"adjudicada\",\n    \"purchaseType\": [\n      \"bien\",\n      \"bien_y_servicio\",\n      \"servicio\",\n      \"farmacos\"\n    ],\n    \"timeScale\": \"anual\",\n    \"typeGraph\": 1\n  }\n]", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```
Este endpoint devuelve información sobre procesos GEN, en los cuales se calcularon los indicadores individuales, obteniéndose una calificación promedio que sirve para medir el riesgo en el proceso. Los datos están agrupados anualmente, por lo tanto, la información se consolida a ese nivel de agrupación.

### HTTP Request

`GET http://serverapiSoceIP/api/soce_gen_ic_proceso/{{process_list}}`

### Definición de parámetros

Element | Description
--------- | -----------
process_list | Parámetro en formato JSON.


### Estructura del parámetro JSON

Element | Description
------- | -----------
dateRage | Elemento json para definir un rango de fechas
dateRangeStart | Elemento json para definir la fecha inicial
dateRangeEnd | Elemento json para definir la fecha final
pickedYear | Elemento para definir el año inicial o final
pickedMonth | Elemento para definir el mes inicial o final (el mes debe estar entre 0 y 11, siendo 0 Enero, y 11 Diciembre)
pickedDay | Elemento para definir el día inicial o final
monto | Elemento que puede ser TRUE o FALSE, en el caso de estar marcado se muestra información acerca del monto presupuestado del proceso
processStage | Elemento que contiene el estado del proceso. 
purchaseType | Elemento array que contiene el tipo de compra que puede contener estos valores: bien, bien_y_servicio, servicio, farmacos
timeScale | Elemento que indica al endpoint la frecuencia en la que se debe recuperar los datos, en este caso, debe tener el valor "anual"
typeGraph | Elemnto que define la forma en la que se recuperan los datos, para este caso, debe tener el valor "1"


>Example JSON parameter:

```json
[
  {
    "dateRange": {
      "dateRangeStart": {
        "pickedYear": "2008",
        "pickedMonth": "3",
        "pickedDay": "24"
      },
      "dateRangeEnd": {
        "pickedYear": "2010",
        "pickedMonth": "5",
        "pickedDay": "14"
      }
    },
    "monto": "true",
    "processStage": "adjudicada",
    "purchaseType": [
      "bien",
      "bien_y_servicio",
      "servicio",
      "farmacos"
    ],
    "timeScale": "anual",
    "typeGraph": 1
  }
]
```

### Estructura de la respuesta JSON

Element | Description
------- | -----------
anio | Elemento que muestra la año de la fecha de publicación de un proceso
avg_indicadores | Elemento que muestra la calificación para ese año, este es el valor del índice compuesto que se calcula sacando el promedio por cada proceso y sus respectivos indicadores individuales, este valor está entre 0 y 100%
quartiles | Elemento que indica el valor del cuartial relacionado a la calificación obtenida en el rango de fechas
riesgo | Elemento que indica el riesgo para mes en la que se agrupan los resultados
std_monto_presupuestado | Elemento que muestra el monto presupuestado de los procesos considerados en el rango de fechas


>Example Response:

```json
[
  {
    "anio": 2008,
    "avg_indicadores": 44.21,
    "quartiles": 1,
    "riesgo": "Alto",
    "std_monto_presupuestado": 258138.25
  },
  {
    "anio": 2009,
    "avg_indicadores": 44.06,
    "quartiles": 1,
    "riesgo": "Alto",
    "std_monto_presupuestado": 7034775.21
  },
  {
    "anio": 2010,
    "avg_indicadores": 43.08,
    "quartiles": 1,
    "riesgo": "Alto",
    "std_monto_presupuestado": 1127310.38
  }
]
```

## Get GEN Proceso Year Detalle

```python
import http.client

conn = http.client.HTTPSConnection("serverapiSoceIP", 5000)
payload = ''
headers = {}
conn.request("GET", "/api/soce_gen_ic_proceso_detail/[
  {
    \"dateRange\": {
      \"dateRangeStart\": {
        \"pickedYear\": \"2008\",
        \"pickedMonth\": \"3\",
        \"pickedDay\": \"24\"
      },
      \"dateRangeEnd\": {
        \"pickedYear\": \"2008\",
        \"pickedMonth\": \"5\",
        \"pickedDay\": \"14\"
      }
    },
    \"monto\": \"true\",
    \"processStage\": \"adjudicada\",
    \"purchaseType\": [
      \"bien\",
      \"bien_y_servicio\",
      \"servicio\",
      \"farmacos\"
    ],
    \"timeScale\": \"anual\",
    \"typeGraph\": 1
  }
]", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))
```

```javascript
var requestOptions = {
  method: 'GET',
  redirect: 'follow'
};

fetch("http://serverapiSoceIP:5000/api/soce_gen_ic_proceso_detail/[\n  {\n    \"dateRange\": {\n      \"dateRangeStart\": {\n        \"pickedYear\": \"2008\",\n        \"pickedMonth\": \"3\",\n        \"pickedDay\": \"24\"\n      },\n      \"dateRangeEnd\": {\n        \"pickedYear\": \"2008\",\n        \"pickedMonth\": \"5\",\n        \"pickedDay\": \"14\"\n      }\n    },\n    \"monto\": \"true\",\n    \"processStage\": \"adjudicada\",\n    \"purchaseType\": [\n      \"bien\",\n      \"bien_y_servicio\",\n      \"servicio\",\n      \"farmacos\"\n    ],\n    \"timeScale\": \"anual\",\n    \"typeGraph\": 1\n  }\n]", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```

Este endpoint devuelve información detallada sobre procesos GEN, la misma que puede ser usada para calcular la calificación promedio de los procesos considereados en frecuencia anual.

### HTTP Request

`GET http://serverapiSoceIP/api/soce_gen_ic_proceso_detail/{{process_list}}`

### Definición de parámetros

Element | Description
--------- | -----------
process_list | Parámetro en formato JSON.


### Estructura del parámetro JSON

Element | Description
------- | -----------
dateRage | Elemento json para definir un rango de fechas
dateRangeStart | Elemento json para definir la fecha inicial
dateRangeEnd | Elemento json para definir la fecha final
pickedYear | Elemento para definir el año inicial o final
pickedMonth | Elemento para definir el mes inicial o final (el mes debe estar entre 0 y 11, siendo 0 Enero, y 11 Diciembre)
pickedDay | Elemento para definir el día inicial o final
monto | Elemento que puede ser TRUE o FALSE, en el caso de estar marcado se muestra información acerca del monto presupuestado del proceso
processStage | Elemento que contiene el estado del proceso. 
purchaseType | Elemento array que contiene el tipo de compra que puede contener estos valores: bien, bien_y_servicio, servicio, farmacos
timeScale | Elemento que indica al endpoint la frecuencia en la que se debe recuperar los datos, en este caso, debe tener el valor "anual"
typeGraph | Elemnto que define la forma en la que se recuperan los datos, para este caso, debe tener el valor "1"


>Example JSON parameter:

```json
[
  {
    "dateRange": {
      "dateRangeStart": {
        "pickedYear": "2008",
        "pickedMonth": "3",
        "pickedDay": "24"
      },
      "dateRangeEnd": {
        "pickedYear": "2008",
        "pickedMonth": "5",
        "pickedDay": "14"
      }
    },
    "monto": "true",
    "processStage": "adjudicada",
    "purchaseType": [
      "bien",
      "bien_y_servicio",
      "servicio",
      "farmacos"
    ],
    "timeScale": "anual",
    "typeGraph": 1
  }
]
```

### Estructura de la respuesta JSON

Element | Description
------- | -----------
fecha_publicacion | Elemento que muestra la fecha de publicación, para este caso, el año es el objetivo del análisis
codigo | Elemento que muestra el código del Proceso
indicador_## | Elemento que muestra el valor de calificación del proceso en cada indicador. Se tienen indicadores desde el 01 hasta el 29, el valor 1 significafa que le proceso cumple con la ficha metodológica de cálculo, es decir es un proceso "bueno", mientras que el valor de 0 indica que ese proceso no cumplió la ficha, por lo tanto su calificación es "mala". Para el caso de valores NULOS o null, significa que el proceso no participó en el análisis o cálculo para un particular indicador.
monto_presupuestado | Elemento que muestra el monto presupuestado de los procesos considerados en el rango de fechas


>Example Response:

```json
[
  {
    "codigo": "ms 002 08",
    "fecha_publicacion": "2008-08-01",
    "indicador_01": 0,
    "indicador_02": 0,
    "indicador_04": null,
    "indicador_05": 1,
    "indicador_06": null,
    "indicador_09": 0,
    "indicador_11": 0,
    "indicador_12": 0,
    "indicador_18": 0,
    "indicador_19": null,
    "indicador_22": 0,
    "indicador_25": 1,
    "indicador_27": null,
    "indicador_28": 1,
    "indicador_29": 1,
    "monto_presupuestado": 3643.5
  },
  {
    "codigo": "EEMCA-217-08",
    "fecha_publicacion": "2008-08-14",
    "indicador_01": 0,
    "indicador_02": 1,
    "indicador_04": null,
    "indicador_05": 1,
    "indicador_06": null,
    "indicador_09": null,
    "indicador_11": null,
    "indicador_12": 0,
    "indicador_18": 1,
    "indicador_19": null,
    "indicador_22": 0,
    "indicador_25": 1,
    "indicador_27": null,
    "indicador_28": 1,
    "indicador_29": 1,
    "monto_presupuestado": 5488
  },
  {
    "codigo": "PUB-331-EMAC-DAF2009",
    "fecha_publicacion": "2008-11-17",
    "indicador_01": 0,
    "indicador_02": 0,
    "indicador_04": null,
    "indicador_05": 1,
    "indicador_06": null,
    "indicador_09": 0,
    "indicador_11": 0,
    "indicador_12": 0,
    "indicador_18": 1,
    "indicador_19": null,
    "indicador_22": 0,
    "indicador_25": 0,
    "indicador_27": null,
    "indicador_28": 1,
    "indicador_29": 1,
    "monto_presupuestado": 7467.13
  }
]
```

## Get GEN Procesos Riesgo

```python
import http.client

conn = http.client.HTTPSConnection("serverapiSoceIP", 5000)
payload = ''
headers = {}
conn.request("GET", "/api/soce_gen_procesos_riesgo", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))
```

```javascript
var requestOptions = {
  method: 'GET',
  redirect: 'follow'
};

fetch("http://serverapiSoceIP:5000/api/soce_gen_procesos_riesgo", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```

Este endpoint devuelve información sobre procesos GEN, agrupados por nivel de riesgo. No hay parámetros de búsqueda.

### HTTP Request

`GET http://serverapiSoceIP/api/soce_gen_procesos_riesgo`

### Estructura de la respuesta JSON

Element | Description
------- | -----------
total_gen_registros | Elemento que el total de procesos GEN bajados y procesados en el cálculo del índice compuesto
riesgo | Elemento que muestra el nivel de riesgo que puede ser: Muy Alto, Alto, Medio y Bajo
nro_procesos_x_riesgo | Elemento que muestra el número de procesos en cada nivel de riesgo
porcentaje | Elemento que muestra su correspondiente porcentaje respecto al número total de procesos GEN.


>Example Response:

```json
[
  {
    "nro_procesos_x_riesgo": 286646,
    "porcentaje": "71.32",
    "riesgo": "Alto",
    "total_gen_registros": "401942"
  },
  {
    "nro_procesos_x_riesgo": 10072,
    "porcentaje": "2.51",
    "riesgo": "Bajo",
    "total_gen_registros": "401942"
  },
  {
    "nro_procesos_x_riesgo": 104895,
    "porcentaje": "26.10",
    "riesgo": "Medio",
    "total_gen_registros": "401942"
  },
  {
    "nro_procesos_x_riesgo": 329,
    "porcentaje": "0.08",
    "riesgo": "Muy Alto",
    "total_gen_registros": "401942"
  }
]
```


## Get GEN Procesos por Anio

```python
import http.client

conn = http.client.HTTPSConnection("serverapiSoceIP", 5000)
payload = ''
headers = {}
conn.request("GET", "/api/soce_gen_procesos_anio", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))
```

```javascript
var requestOptions = {
  method: 'GET',
  redirect: 'follow'
};

fetch("http://serverapiSoceIP:5000/api/soce_gen_procesos_anio", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```
Este endpoint devuelve información sobre procesos GEN, agrupados por año. No hay parámetros de búsqueda.

### HTTP Request

`GET http://serverapiSoceIP/api/soce_gen_procesos_anio`

### Estructura de la respuesta JSON

Element | Description
------- | -----------
total_gen_registros | Elemento que el total de procesos GEN bajados y procesados en el cálculo del índice compuesto
anio | Elemento que muestra el año en donde se agrupan los procesos
nro_procesos_x_anio | Elemento que muestra el número de procesos por cada año
porcentaje | Elemento que muestra su correspondiente porcentaje respecto al número total de procesos GEN


>Example Response:

```json
[  
  {
    "anio": 2008,
    "nro_procesos_x_anio": 38986,
    "porcentaje": 9.7,
    "total_gen_registros": 401942
  },
  {
    "anio": 2009,
    "nro_procesos_x_anio": 139427,
    "porcentaje": 34.69,
    "total_gen_registros": 401942
  },
  {
    "anio": 2010,
    "nro_procesos_x_anio": 129575,
    "porcentaje": 32.24,
    "total_gen_registros": 401942
  },
  {
    "anio": 2011,
    "nro_procesos_x_anio": 14459,
    "porcentaje": 3.6,
    "total_gen_registros": 401942
  }
]
```

## Get GEN Proceso Patron

```python
import http.client

conn = http.client.HTTPSConnection("serverapiSoceIP", 5000)
payload = ''
headers = {}
conn.request("GET", "/api/soce_gen_ic_proceso/[
  {
    \"typeGraph\": 2,
    \"patron\": \"BASALI\"
  }
]", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))
```

```javascript
var requestOptions = {
  method: 'GET',
  redirect: 'follow'
};

fetch("http://serverapiSoceIP:5000/api/soce_gen_ic_proceso/[\n  {\n    \"typeGraph\": 2,\n    \"patron\": \"BASALI\"\n  }\n]", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```
Este endpoint devuelve información detallada sobre procesos GEN, basados en un patrón o criterio de búsqueda.

### HTTP Request

`GET http://serverapiSoceIP/api/soce_gen_ic_proceso/{{process_list}}`

### Definición de parámetros

Element | Description
--------- | -----------
process_list | Parámetro en formato JSON.


### Estructura del parámetro JSON

Element | Description
------- | -----------
typeGraph | Elemento que indica el tipo de gráfico que se usará, en ete caso siempre es "2"
patron | Elemento que sirve para pasar un patrón o criterio de búsqueda, el mismo que se puede aplicar en: código del proceso, nombre de la entidad contratante, objecto del proceso, nombre y ruc del proveedor


>Example JSON parameter:

```json
[
  {
    "typeGraph": 2,
    "patron": "BASALI"
  }
]
```

### Estructura de la respuesta JSON

Element | Description
------- | -----------
anio | Elemento que muestra el año de la fecha de publicación del proceso
mes | Elemento que muestra el mes de la fecha de publicación del proceso
codigo | Elemento que muestra el código del Proceso
contract_id | Elemento que muestra un código interno en la DB
entidad | Elemento que muestra el nombre de la entidad contratante
estado_del_proceso | Elemento que muestra el estado del proceso
fecha_publicacion | Elemento que muestra la fecha de publicación del proceso
indicador_## | Elemento que muestra el valor de calificación del proceso en cada indicador. Se tienen indicadores desde el 01 hasta el 29, el valor 1 significafa que le proceso cumple con la ficha metodológica de cálculo, es decir es un proceso "bueno", mientras que el valor de 0 indica que ese proceso no cumplió la ficha, por lo tanto su calificación es "mala". Para el caso de valores NULOS o null, significa que el proceso no participó en el análisis o cálculo para un particular indicador.
objeto_de_proceso | Elemento que muestra una descripción sobre el objeto del proceso
ruc | Elemento que muestra el RUC del proveedor adjudicado, dependiendo de la fase de calificación contracutal, este dato puede o no existir
proveedor | Elemento que muestra el nombre del proveedor adjudicado, dependiendo de la fase de calificación contracutal, este dato puede o no existir
tipo_compra | Elemento que muestra el tipo de compra que puede ser: bien, bien_y_servicio, servicio, farmacos
monto_presupuestado | Elemento que muestra el monto presupuestado de los procesos considerados en el rango de fechas
promedio | Elemento que muestra la calificación del proceso basado en los valores obtenidos en cada indicador. Recordemos que a nivel de procesos, la calificación puede ser 1 y 0; siendo 1 un proceso "bueno" o que cumple la ficha metedológica de cálculo, y 0 significa que es "malo", o no cumple con la ficha.


>Example Response:

```json
[
  {
    "anio": "2010",
    "codigo": "PE-BASALI-07-10-10",
    "contract_id": "503178",
    "entidad": "BASE NAVAL DE SALINAS",
    "estado_del_proceso": "Finalizada",
    "fecha_publicacion": "05/10/2010",
    "indicador_01": "0",
    "indicador_02": "0",
    "indicador_04": null,
    "indicador_05": "1",
    "indicador_06": null,
    "indicador_09": "0",
    "indicador_11": "0",
    "indicador_12": "0",
    "indicador_18": "1",
    "indicador_19": "0",
    "indicador_22": "0",
    "indicador_25": "0",
    "indicador_27": null,
    "indicador_28": "1",
    "indicador_29": "1",
    "mes": "10",
    "monto_presupuestado": "$826.94",
    "objeto_de_proceso": "HORAS CLASES",
    "promedio": "0.33",
    "proveedor": null,
    "ruc": null,
    "tipo_compra": "Servicio"
  },
  {
    "anio": "2010",
    "codigo": "PE-BASALI-09-06-2010",
    "contract_id": "449460",
    "entidad": "BASE NAVAL DE SALINAS",
    "estado_del_proceso": "Finalizada",
    "fecha_publicacion": "24/06/2010",
    "indicador_01": "0",
    "indicador_02": "0",
    "indicador_04": null,
    "indicador_05": "1",
    "indicador_06": null,
    "indicador_09": "0",
    "indicador_11": "0",
    "indicador_12": "0",
    "indicador_18": "0",
    "indicador_19": "0",
    "indicador_22": "0",
    "indicador_25": "0",
    "indicador_27": null,
    "indicador_28": "1",
    "indicador_29": "1",
    "mes": "6",
    "monto_presupuestado": "$4,423.80",
    "objeto_de_proceso": "ADQUISICION DE SUMINISTROS",
    "promedio": "0.25",
    "proveedor": null,
    "ruc": null,
    "tipo_compra": "Bien"
  }
]
```

## Get GEN Proceso Patron Detalle

```python
import http.client

conn = http.client.HTTPSConnection("serverapiSoceIP", 5000)
payload = ''
headers = {}
conn.request("GET", "/api/soce_gen_ic_proceso_detail/[
  {
    \"typeGraph\": 2,
    \"patron\": \"BASALI\"
  }
]", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))
```

```javascript
var requestOptions = {
  method: 'GET',
  redirect: 'follow'
};

fetch("http://serverapiSoceIP:5000/api/soce_gen_ic_proceso_detail/[\n  {\n    \"typeGraph\": 2,\n    \"patron\": \"BASALI\"\n  }\n]", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```

Este endpoint devuelve información detallada sobre procesos GEN, basados en un patrón o criterio de búsqueda.

### HTTP Request

`GET http://serverapiSoceIP/api/soce_gen_ic_proceso_detail/{{process_list}}`

### Definición de parámetros

Element | Description
--------- | -----------
process_list | Parámetro en formato JSON.


### Estructura del parámetro JSON

Element | Description
------- | -----------
typeGraph | Elemento que indica el tipo de gráfico que se usará, en ete caso siempre es "2"
patron | Elemento que sirve para pasar un patrón o criterio de búsqueda, el mismo que se puede aplicar en: código del proceso, nombre de la entidad contratante, objecto del proceso, nombre y ruc del proveedor


>Example JSON parameter:

```json
[
  {
    "typeGraph": 2,
    "patron": "BASALI"
  }
]
```

### Estructura de la respuesta JSON

Element | Description
------- | -----------
fecha_publicacion | Elemento que muestra la fecha de publicación del proceso
codigo | Elemento que muestra el código del Proceso
entidad | Elemento que muestra el nombre de la entidad contratante
estado_del_proceso | Elemento que muestra el estado del proceso
indicador_## | Elemento que muestra el valor de calificación del proceso en cada indicador. Se tienen indicadores desde el 01 hasta el 29, el valor 1 significafa que le proceso cumple con la ficha metodológica de cálculo, es decir es un proceso "bueno", mientras que el valor de 0 indica que ese proceso no cumplió la ficha, por lo tanto su calificación es "mala". Para el caso de valores NULOS o null, significa que el proceso no participó en el análisis o cálculo para un particular indicador.
objeto_de_proceso | Elemento que muestra una descripción sobre el objeto del proceso
ruc | Elemento que muestra el RUC del proveedor adjudicado, dependiendo de la fase de calificación contracutal, este dato puede o no existir
proveedor | Elemento que muestra el nombre del proveedor adjudicado, dependiendo de la fase de calificación contracutal, este dato puede o no existir
tipo_compra | Elemento que muestra el tipo de compra que puede ser: bien, bien_y_servicio, servicio, farmacos
monto_presupuestado | Elemento que muestra el monto presupuestado de los procesos considerados en el rango de fechas


>Example Response:

```json
[
  {
    "codigo": "PE-BASALI-07-10-10",
    "entidad": "BASE NAVAL DE SALINAS",
    "estado_del_proceso": "Finalizada",
    "fecha_publicacion": "2010-10-05",
    "indicador_01": 0,
    "indicador_02": 0,
    "indicador_04": null,
    "indicador_05": 1,
    "indicador_06": null,
    "indicador_09": 0,
    "indicador_11": 0,
    "indicador_12": 0,
    "indicador_18": 1,
    "indicador_19": 0,
    "indicador_22": 0,
    "indicador_25": 0,
    "indicador_27": null,
    "indicador_28": 1,
    "indicador_29": 1,
    "monto_presupuestado": 826.94,
    "objeto_de_proceso": "HORAS CLASES",
    "proveedor": null,
    "ruc": null,
    "tipo_compra": "Servicio"
  },
  {
    "codigo": "PE-BASALI-09-06-2010",
    "entidad": "BASE NAVAL DE SALINAS",
    "estado_del_proceso": "Finalizada",
    "fecha_publicacion": "2010-06-24",
    "indicador_01": 0,
    "indicador_02": 0,
    "indicador_04": null,
    "indicador_05": 1,
    "indicador_06": null,
    "indicador_09": 0,
    "indicador_11": 0,
    "indicador_12": 0,
    "indicador_18": 0,
    "indicador_19": 0,
    "indicador_22": 0,
    "indicador_25": 0,
    "indicador_27": null,
    "indicador_28": 1,
    "indicador_29": 1,
    "monto_presupuestado": 4423.8,
    "objeto_de_proceso": "ADQUISICION DE SUMINISTROS",
    "proveedor": null,
    "ruc": null,
    "tipo_compra": "Bien"
  }
]
```


## Get GEN Enditad Mensual

```python
import http.client

conn = http.client.HTTPSConnection("serverapiSoceIP", 5000)
payload = ''
headers = {}
conn.request("GET", "/api/soce_gen_ic_entidad/[{
  \"dateRange\":{
    \"dateRangeStart\":{
      \"pickedYear\": \"2009\",
      \"pickedMonth\": \"0\",
      \"pickedDay\": \"1\"
    },
    \"dateRangeEnd\": {
      \"pickedYear\": \"2009\",
      \"pickedMonth\": \"11\",
      \"pickedDay\": \"3\"
    }
  },
  \"timeScale\": \"mensual\",
  \"typeGraph\": 1
}]", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))
```

```javascript
var requestOptions = {
  method: 'GET',
  redirect: 'follow'
};

fetch("http://serverapiSoceIP:5000/api/soce_gen_ic_entidad/[{\n  \"dateRange\":{\n    \"dateRangeStart\":{\n      \"pickedYear\": \"2009\",\n      \"pickedMonth\": \"0\",\n      \"pickedDay\": \"1\"\n    },\n    \"dateRangeEnd\": {\n      \"pickedYear\": \"2009\",\n      \"pickedMonth\": \"11\",\n      \"pickedDay\": \"3\"\n    }\n  },\n  \"timeScale\": \"mensual\",\n  \"typeGraph\": 1\n}]", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```

Este endpoint devuelve información sobre procesos GEN agrupados por Entidad, en los cuales se calcularon los indicadores individuales, obteniéndose una calificación promedio que sirve para medir el riesgo en el proceso. Los datos están agrupados mensualmente, por lo tanto, la información se consolida a ese nivel de Entidad y Mes.

### HTTP Request

`GET http://serverapiSoceIP/api/soce_gen_ic_entidad//{{process_list}}`

### Definición de parámetros

Element | Description
--------- | -----------
process_list | Parámetro en formato JSON.


### Estructura del parámetro JSON

Element | Description
------- | -----------
dateRage | Elemento json para definir un rango de fechas
dateRangeStart | Elemento json para definir la fecha inicial
dateRangeEnd | Elemento json para definir la fecha final
pickedYear | Elemento para definir el año inicial o final
pickedMonth | Elemento para definir el mes inicial o final (el mes debe estar entre 0 y 11, siendo 0 Enero, y 11 Diciembre)
pickedDay | Elemento para definir el día inicial o final
timeScale | Elemento que indica al endpoint la frecuencia en la que se debe recuperar los datos, en este caso, debe tener el valor "mensual"
typeGraph | Elemnto que define la forma en la que se recuperan los datos, para este caso, debe tener el valor "1"


>Example JSON parameter:

```json
[
  {
  "dateRange":{
    "dateRangeStart":{
      "pickedYear": "2009",
      "pickedMonth": "0",
      "pickedDay": "1"
      },
    "dateRangeEnd": {
      "pickedYear": "2009",
      "pickedMonth": "11",
      "pickedDay": "3"
      }
    },
  "timeScale": "mensual",
  "typeGraph": 1
  }
]
```

### Estructura de la respuesta JSON

Element | Description
------- | -----------
entidad | Elemento que muestra el nombre de la entidad contratante
anio | Elemento que muestra el año relacionado a la fecha de publicación de los procesos
mes | Elemento que muestra el mes relacionado a la fecha de publicación de los procesos
avg_indicadores | Elemento que muestra la calificación promedio para la entidad en el mes analizado
quartiles | Elemento que muestra el valor del cuartil respecto al elemento avg_indicadores
riesgo | Elemento que muestra el nivel de riesgo para la entidad contratante, basado en el cuartil


>Example Response:

```json
[
  {
    "anio": 2009,
    "avg_indicadores": 79.55,
    "entidad": "GOBIERNO AUTONOMO DESCENTRALIZADO MUNICIPAL DE SANTO DOMINGO",
    "mes": 1,
    "quartiles": 3,
    "riesgo": "Bajo"
  },  
  {
    "anio": 2009,
    "avg_indicadores": 70.59,
    "entidad": "GOBIERNO AUTONOMO DESCENTRALIZADO MUNICIPAL DEL CANTON TISALEO",
    "mes": 1,
    "quartiles": 2,
    "riesgo": "Medio"
  },
  {
    "anio": 2009,
    "avg_indicadores": 68.67,
    "entidad": "GOBIERNO AUTONOMO DESCENTRALIZADO MUNICIPAL DE SIGCHOS",
    "mes": 1,
    "quartiles": 2,
    "riesgo": "Medio"
  }
]
```


## Get GEN Enditad Mensual Detalle

```python
import http.client

conn = http.client.HTTPSConnection("serverapiSoceIP", 5000)
payload = ''
headers = {}
conn.request("GET", "/api/soce_gen_ic_entidad_detail/[{
  \"dateRange\":{
    \"dateRangeStart\":{
      \"pickedYear\": \"2009\",
      \"pickedMonth\": \"0\",
      \"pickedDay\": \"1\"
    },
    \"dateRangeEnd\": {
      \"pickedYear\": \"2009\",
      \"pickedMonth\": \"3\",
      \"pickedDay\": \"3\"
    }
  },
  \"timeScale\": \"mensual\",
  \"typeGraph\": 1
}]", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))
```

```javascript
var requestOptions = {
  method: 'GET',
  redirect: 'follow'
};

fetch("http://serverapiSoceIP:5000/api/soce_gen_ic_entidad_detail/[{\n  \"dateRange\":{\n    \"dateRangeStart\":{\n      \"pickedYear\": \"2009\",\n      \"pickedMonth\": \"0\",\n      \"pickedDay\": \"1\"\n    },\n    \"dateRangeEnd\": {\n      \"pickedYear\": \"2009\",\n      \"pickedMonth\": \"3\",\n      \"pickedDay\": \"3\"\n    }\n  },\n  \"timeScale\": \"mensual\",\n  \"typeGraph\": 1\n}]", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```

Este endpoint devuelve información detallada sobre procesos GEN agrupados por Entidad, en los cuales se calcularon los indicadores individuales, obteniéndose una calificación promedio que sirve para medir el riesgo en el proceso. Los datos están agrupados mensualmente, por lo tanto, la información se consolida a ese nivel de Entidad y Mes.

### HTTP Request

`GET http://serverapiSoceIP/api/soce_gen_ic_entidad_detail/{{process_list}}`

### Definición de parámetros

Element | Description
--------- | -----------
process_list | Parámetro en formato JSON.


### Estructura del parámetro JSON

Element | Description
------- | -----------
dateRage | Elemento json para definir un rango de fechas
dateRangeStart | Elemento json para definir la fecha inicial
dateRangeEnd | Elemento json para definir la fecha final
pickedYear | Elemento para definir el año inicial o final
pickedMonth | Elemento para definir el mes inicial o final (el mes debe estar entre 0 y 11, siendo 0 Enero, y 11 Diciembre)
pickedDay | Elemento para definir el día inicial o final
timeScale | Elemento que indica al endpoint la frecuencia en la que se debe recuperar los datos, en este caso, debe tener el valor "mensual"
typeGraph | Elemnto que define la forma en la que se recuperan los datos, para este caso, debe tener el valor "1"


>Example JSON parameter:


```json
[
  {
  "dateRange":{
    "dateRangeStart":{
      "pickedYear": "2009",
      "pickedMonth": "0",
      "pickedDay": "1"
      },
    "dateRangeEnd": {
      "pickedYear": "2009",
      "pickedMonth": "3",
      "pickedDay": "3"
      }
    },
  "timeScale": "mensual",
  "typeGraph": 1
  }
]
```

### Estructura de la respuesta JSON

Element | Description
------- | -----------
anio | Elemento que muestra el año de la fecha de publicación del proceso, sobre este año se debe agrupar
mes | Elemento que muestra el mes de la fecha de publicación del proceso, sobre este mes se debe agrupar
dia | Elemento que muestra la fecha del inicio del mes en donde se agrupan los datos
entidad | Elemento que muestra el nombre de la entidad contratante
indicador_## | Elemento que muestra el valor de calificación del proceso en cada indicador. Se tienen indicadores desde el 01 hasta el 29, el valor de calificación para cada indicador individual se mide en porcentaje que va del 0 al 100%, siendo 0% una mala calificación, y 100% una buena calificación. Refierase a la metodología de cálculo descrita en el sitio web.


>Example Response:

```json
[
  {
    "dia": "2009-02-01",
    "entidad": "Colegio Toacaso",
    "indicador_01": 0,
    "indicador_02": 0,
    "indicador_04": null,
    "indicador_05": 100,
    "indicador_06": null,
    "indicador_09": null,
    "indicador_11": null,
    "indicador_12": 100,
    "indicador_18": null,
    "indicador_19": null,
    "indicador_22": null,
    "indicador_25": null,
    "indicador_27": null,
    "indicador_28": null,
    "indicador_29": null
  },
  {
    "dia": "2009-03-01",
    "entidad": "EMAPASD",
    "indicador_01": 0,
    "indicador_02": 0,
    "indicador_04": null,
    "indicador_05": 100,
    "indicador_06": null,
    "indicador_09": 0,
    "indicador_11": 0,
    "indicador_12": 100,
    "indicador_18": 100,
    "indicador_19": null,
    "indicador_22": 25,
    "indicador_25": 99,
    "indicador_27": null,
    "indicador_28": 100,
    "indicador_29": 100
  }
]
```

## Get GEN Entidad Year

```python
import http.client

conn = http.client.HTTPSConnection("serverapiSoceIP", 5000)
payload = ''
headers = {}
conn.request("GET", "/api/soce_gen_ic_entidad/[{
  \"dateRange\":{
    \"dateRangeStart\":{
      \"pickedYear\": \"2009\",
      \"pickedMonth\": \"0\",
      \"pickedDay\": \"1\"
    },
    \"dateRangeEnd\": {
      \"pickedYear\": \"2009\",
      \"pickedMonth\": \"11\",
      \"pickedDay\": \"3\"
    }
  },
  \"timeScale\": \"anual\",
  \"typeGraph\": 1
}]", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))
```

```javascript
var requestOptions = {
  method: 'GET',
  redirect: 'follow'
};

fetch("http://serverapiSoceIP:5000/api/soce_gen_ic_entidad/[{\n  \"dateRange\":{\n    \"dateRangeStart\":{\n      \"pickedYear\": \"2009\",\n      \"pickedMonth\": \"0\",\n      \"pickedDay\": \"1\"\n    },\n    \"dateRangeEnd\": {\n      \"pickedYear\": \"2009\",\n      \"pickedMonth\": \"11\",\n      \"pickedDay\": \"3\"\n    }\n  },\n  \"timeScale\": \"anual\",\n  \"typeGraph\": 1\n}]", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```

Este endpoint devuelve información sobre procesos GEN agrupados por Entidad, en los cuales se calcularon los indicadores individuales, obteniéndose una calificación promedio que sirve para medir el riesgo en el proceso. Los datos están agrupados anualmente, por lo tanto, la información se consolida a ese nivel de Entidad y Año.

### HTTP Request

`GET http://serverapiSoceIP/api/soce_gen_ic_entidad/{{process_list}}`

### Definición de parámetros

Element | Description
--------- | -----------
process_list | Parámetro en formato JSON.


### Estructura del parámetro JSON

Element | Description
------- | -----------
dateRage | Elemento json para definir un rango de fechas
dateRangeStart | Elemento json para definir la fecha inicial
dateRangeEnd | Elemento json para definir la fecha final
pickedYear | Elemento para definir el año inicial o final
pickedMonth | Elemento para definir el mes inicial o final (el mes debe estar entre 0 y 11, siendo 0 Enero, y 11 Diciembre)
pickedDay | Elemento para definir el día inicial o final
timeScale | Elemento que indica al endpoint la frecuencia en la que se debe recuperar los datos, en este caso, debe tener el valor "anual"
typeGraph | Elemnto que define la forma en la que se recuperan los datos, para este caso, debe tener el valor "1"


>Example JSON parameter:

```json
[
  {
  "dateRange":{
    "dateRangeStart":{
      "pickedYear": "2009",
      "pickedMonth": "0",
      "pickedDay": "1"
      },
    "dateRangeEnd": {
      "pickedYear": "2009",
      "pickedMonth": "11",
      "pickedDay": "3"
      }
    },
  "timeScale": "anual",
  "typeGraph": 1
  }
]
```

### Estructura de la respuesta JSON

Element | Description
------- | -----------
entidad | Elemento que muestra el nombre de la entidad contratante
anio | Elemento que muestra el año relacionado a la fecha de publicación de los procesos
avg_indicadores | Elemento que muestra la calificación promedio para la entidad en el año analizado
quartiles | Elemento que muestra el valor del cuartil respecto al elemento avg_indicadores
riesgo | Elemento que muestra el nivel de riesgo para la entidad contratante, basado en el cuartil


>Example Response:

```json
[
  {
    "anio": 2009,
    "avg_indicadores": 80,
    "entidad": "SECRETARIA COMISION TECNICA UNEMI",
    "quartiles": 3,
    "riesgo": "Bajo"
  },  
  {
    "anio": 2009,
    "avg_indicadores": 72.92,
    "entidad": "GOBIERNO MUNICIPAL DE FLAVIO ALFARO",
    "quartiles": 2,
    "riesgo": "Medio"
  },
  {
    "anio": 2009,
    "avg_indicadores": 49.99,
    "entidad": "Grupo T Esmeraldas",
    "quartiles": 1,
    "riesgo": "Alto"
  },
  {
    "anio": 2009,
    "avg_indicadores": 49.99,
    "entidad": "COLEGIO TENIENTE HUGO ORTIZ GARCES",
    "quartiles": 1,
    "riesgo": "Alto"
  }
]
```

## Get GEN Entidad Year Detalle

```python
import http.client

conn = http.client.HTTPSConnection("serverapiSoceIP", 5000)
payload = ''
headers = {}
conn.request("GET", "/api/soce_gen_ic_entidad_detail/[{
  \"dateRange\":{
    \"dateRangeStart\":{
      \"pickedYear\": \"2009\",
      \"pickedMonth\": \"0\",
      \"pickedDay\": \"1\"
    },
    \"dateRangeEnd\": {
      \"pickedYear\": \"2009\",
      \"pickedMonth\": \"11\",
      \"pickedDay\": \"3\"
    }
  },
  \"timeScale\": \"anual\",
  \"typeGraph\": 1
}]", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))
```

```javascript
var requestOptions = {
  method: 'GET',
  redirect: 'follow'
};

fetch("http://serverapiSoceIP:5000/api/soce_gen_ic_entidad_detail/[{\n  \"dateRange\":{\n    \"dateRangeStart\":{\n      \"pickedYear\": \"2009\",\n      \"pickedMonth\": \"0\",\n      \"pickedDay\": \"1\"\n    },\n    \"dateRangeEnd\": {\n      \"pickedYear\": \"2009\",\n      \"pickedMonth\": \"11\",\n      \"pickedDay\": \"3\"\n    }\n  },\n  \"timeScale\": \"anual\",\n  \"typeGraph\": 1\n}]", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```

Este endpoint devuelve información detallada sobre procesos GEN agrupados por Entidad, en los cuales se calcularon los indicadores individuales, obteniéndose una calificación promedio que sirve para medir el riesgo en el proceso. Los datos están agrupados anualmente, por lo tanto, la información se consolida a ese nivel de Entidad y Año.

### HTTP Request

`GET http://serverapiSoceIP/api/soce_gen_ic_entidad_detail/{{process_list}}`

### Definición de parámetros

Element | Description
--------- | -----------
process_list | Parámetro en formato JSON.


### Estructura del parámetro JSON

Element | Description
------- | -----------
dateRage | Elemento json para definir un rango de fechas
dateRangeStart | Elemento json para definir la fecha inicial
dateRangeEnd | Elemento json para definir la fecha final
pickedYear | Elemento para definir el año inicial o final
pickedMonth | Elemento para definir el mes inicial o final (el mes debe estar entre 0 y 11, siendo 0 Enero, y 11 Diciembre)
pickedDay | Elemento para definir el día inicial o final
timeScale | Elemento que indica al endpoint la frecuencia en la que se debe recuperar los datos, en este caso, debe tener el valor "anual"
typeGraph | Elemnto que define la forma en la que se recuperan los datos, para este caso, debe tener el valor "1"


>Example JSON parameter:

```json
[
  {
  "dateRange":{
    "dateRangeStart":{
      "pickedYear": "2009",
      "pickedMonth": "0",
      "pickedDay": "1"
      },
    "dateRangeEnd": {
      "pickedYear": "2009",
      "pickedMonth": "11",
      "pickedDay": "3"
      }
    },
  "timeScale": "anual",
  "typeGraph": 1
  }
]
```

### Estructura de la respuesta JSON

Element | Description
------- | -----------
dia | Elemento que muestra una fecha de agrupación , aquí nos interesa extraer el año, por el que se debe agrupar
entidad | Elemento que muestra el nombre de la entidad contratante
indicador_## | Elemento que muestra el valor de calificación del proceso en cada indicador. Se tienen indicadores desde el 01 hasta el 29, el valor de calificación para cada indicador individual se mide en porcentaje que va del 0 al 100%, siendo 0% una mala calificación, y 100% una buena calificación. Refierase a la metodología de cálculo descrita en el sitio web.


>Example Response:

```json
[
  {
    "dia": "2009-06-01",
    "entidad": "BASE DE MOVILIZACION NORTE",
    "indicador_01": 50,
    "indicador_02": 0,
    "indicador_04": null,
    "indicador_05": 100,
    "indicador_06": null,
    "indicador_09": null,
    "indicador_11": null,
    "indicador_12": 0,
    "indicador_18": null,
    "indicador_19": null,
    "indicador_22": null,
    "indicador_25": null,
    "indicador_27": null,
    "indicador_28": null,
    "indicador_29": null
  },
  {
    "dia": "2009-02-01",
    "entidad": "Colegio Toacaso",
    "indicador_01": 0,
    "indicador_02": 0,
    "indicador_04": null,
    "indicador_05": 100,
    "indicador_06": null,
    "indicador_09": null,
    "indicador_11": null,
    "indicador_12": 100,
    "indicador_18": null,
    "indicador_19": null,
    "indicador_22": null,
    "indicador_25": null,
    "indicador_27": null,
    "indicador_28": null,
    "indicador_29": null
  }
]
```

## Get GEN Entidad patron

```python
import http.client

conn = http.client.HTTPSConnection("serverapiSoceIP", 5000)
payload = ''
headers = {}
conn.request("GET", "/api/soce_gen_ic_entidad/[
  {
    \"dateRange\": {
      \"dateRangeStart\": {
        \"pickedYear\": \"2009\",
        \"pickedMonth\": \"11\",
        \"pickedDay\": \"1\"
      },
      \"dateRangeEnd\": {
        \"pickedYear\": \"2009\",
        \"pickedMonth\": \"11\",
        \"pickedDay\": \"3\"
      }
    },
    \"patron\": \"INSTITUTO ECUATORIANO\",
    \"typeGraph\": 2
  }
]", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))
```

```javascript
var requestOptions = {
  method: 'GET',
  redirect: 'follow'
};

fetch("http://serverapiSoceIP:5000/api/soce_gen_ic_entidad/[\n  {\n    \"dateRange\": {\n      \"dateRangeStart\": {\n        \"pickedYear\": \"2009\",\n        \"pickedMonth\": \"11\",\n        \"pickedDay\": \"1\"\n      },\n      \"dateRangeEnd\": {\n        \"pickedYear\": \"2009\",\n        \"pickedMonth\": \"11\",\n        \"pickedDay\": \"3\"\n      }\n    },\n    \"patron\": \"INSTITUTO ECUATORIANO\",\n    \"typeGraph\": 2\n  }\n]", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```

Este endpoint devuelve información detallada sobre procesos GEN agrupados por Entidad, basados en un patrón o criterio de búsqueda.

### HTTP Request

`GET http://serverapiSoceIP/api/soce_gen_ic_entidad/{{process_list}}`

### Definición de parámetros

Element | Description
--------- | -----------
process_list | Parámetro en formato JSON.


### Estructura del parámetro JSON

Element | Description
------- | -----------
typeGraph | Elemento que indica el tipo de gráfico que se usará, en ete caso siempre es "2"
patron | Elemento que sirve para pasar un patrón o criterio de búsqueda, el mismo que se puede aplicar en: nombre de la entidad contratante
dateRage | Elemento json para definir un rango de fechas
dateRangeStart | Elemento json para definir la fecha inicial
dateRangeEnd | Elemento json para definir la fecha final
pickedYear | Elemento para definir el año inicial o final
pickedMonth | Elemento para definir el mes inicial o final (el mes debe estar entre 0 y 11, siendo 0 Enero, y 11 Diciembre)
pickedDay | Elemento para definir el día inicial o final


>Example JSON parameter:


```json
[
  {
    "dateRange": {
      "dateRangeStart": {
        "pickedYear": "2009",
        "pickedMonth": "11",
        "pickedDay": "1"
      },
      "dateRangeEnd": {
        "pickedYear": "2009",
        "pickedMonth": "11",
        "pickedDay": "3"
      }
    },
    "patron": "INSTITUTO ECUATORIANO",
    "typeGraph": 2
  }
]
```

### Estructura de la respuesta JSON

Element | Description
------- | -----------
entidad | Elemento que muestra el nombre de la entidad contratante
anio | Elemento que muestra el año de la fecha de publicación del proceso, sobre este año se debe agrupar
mes | Elemento que muestra el mes de la fecha de publicación del proceso, sobre este mes se debe agrupar
promedio | Elemento que muestra la calificación promedio para la entidad en el año y mes analizados


>Example Response:

```json
[
  {
    "anio": "2009",
    "entidad": "Instituto Ecuatoriano de Seguridad Social Direccion Provincial del Guayas",
    "mes": "12",
    "promedio": "41.82"
  },
  {
    "anio": "2009",
    "entidad": "INSTITUTO ECUATORIANO DE SEGURIDAD SOCIAL SUBDIRECCIÓN SISTEMA DE PENSIONES",
    "mes": "12",
    "promedio": "38.27"
  },
  {
    "anio": "2009",
    "entidad": "INSTITUTO ECUATORIANO DE SEGURIDAD SOCIAL",
    "mes": "12",
    "promedio": "35.98"
  },
  {
    "anio": "2009",
    "entidad": "INSTITUTO ECUATORIANO DE SEGURIDAD SOCIAL DIRECCION GENERAL PUERTO AYORA",
    "mes": "12",
    "promedio": "34.93"
  }
]
```

## Get GEN Entidad patron Detalle

```python
import http.client

conn = http.client.HTTPSConnection("serverapiSoceIP", 5000)
payload = ''
headers = {}
conn.request("GET", "/api/soce_gen_ic_entidad_detail/[
  {
    \"dateRange\": {
      \"dateRangeStart\": {
        \"pickedYear\": \"2009\",
        \"pickedMonth\": \"11\",
        \"pickedDay\": \"1\"
      },
      \"dateRangeEnd\": {
        \"pickedYear\": \"2009\",
        \"pickedMonth\": \"11\",
        \"pickedDay\": \"3\"
      }
    },
    \"patron\": \"INSTITUTO ECUATORIANO\",
    \"typeGraph\": 2
  }
]", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))
```

```javascript
var requestOptions = {
  method: 'GET',
  redirect: 'follow'
};

fetch("http://serverapiSoceIP:5000/api/soce_gen_ic_entidad_detail/[\n  {\n    \"dateRange\": {\n      \"dateRangeStart\": {\n        \"pickedYear\": \"2009\",\n        \"pickedMonth\": \"11\",\n        \"pickedDay\": \"1\"\n      },\n      \"dateRangeEnd\": {\n        \"pickedYear\": \"2009\",\n        \"pickedMonth\": \"11\",\n        \"pickedDay\": \"3\"\n      }\n    },\n    \"patron\": \"INSTITUTO ECUATORIANO\",\n    \"typeGraph\": 2\n  }\n]", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```

Este endpoint devuelve información detallada sobre procesos GEN agrupados por Entidad, basados en un patrón o criterio de búsqueda.

### HTTP Request

`GET http://serverapiSoceIP/api/soce_gen_ic_entidad_detail/{{process_list}}`

### Definición de parámetros

Element | Description
--------- | -----------
process_list | Parámetro en formato JSON.


### Estructura del parámetro JSON

Element | Description
------- | -----------
typeGraph | Elemento que indica el tipo de gráfico que se usará, en ete caso siempre es "2"
patron | Elemento que sirve para pasar un patrón o criterio de búsqueda, el mismo que se puede aplicar en: nombre de la entidad contratante
dateRage | Elemento json para definir un rango de fechas
dateRangeStart | Elemento json para definir la fecha inicial
dateRangeEnd | Elemento json para definir la fecha final
pickedYear | Elemento para definir el año inicial o final
pickedMonth | Elemento para definir el mes inicial o final (el mes debe estar entre 0 y 11, siendo 0 Enero, y 11 Diciembre)
pickedDay | Elemento para definir el día inicial o final


>Example JSON parameter:

```json
[
  {
    "dateRange": {
      "dateRangeStart": {
        "pickedYear": "2009",
        "pickedMonth": "11",
        "pickedDay": "1"
      },
      "dateRangeEnd": {
        "pickedYear": "2009",
        "pickedMonth": "11",
        "pickedDay": "3"
      }
    },
    "patron": "INSTITUTO ECUATORIANO",
    "typeGraph": 2
  }
]
```

### Estructura de la respuesta JSON

Element | Description
------- | -----------
entidad | Elemento que muestra el nombre de la entidad contratante
dia | Elemento que muestra la fecha del inicio del mes en un año, aquí extraemos año y mes para fines de agrupamiento
indicador_## | Elemento que muestra el valor de calificación del proceso en cada indicador. Se tienen indicadores desde el 01 hasta el 29, el valor de calificación para cada indicador individual se mide en porcentaje que va del 0 al 100%, siendo 0% una mala calificación, y 100% una buena calificación. Refierase a la metodología de cálculo descrita en el sitio web.


>Example Response:

```json
[
  {
    "dia": "2009-12-01",
    "entidad": "INSTITUTO ECUATORIANO DE SEGURIDAD SOCIAL",
    "indicador_01": 49.6894409938,
    "indicador_02": 6.2111801242,
    "indicador_04": null,
    "indicador_05": 100,
    "indicador_06": null,
    "indicador_09": 0,
    "indicador_11": 0,
    "indicador_12": 42.8571428571,
    "indicador_18": 16.7701863354,
    "indicador_19": 0,
    "indicador_22": 25,
    "indicador_25": 99.1055900621,
    "indicador_27": null,
    "indicador_28": 100,
    "indicador_29": 100
  },
  {
    "dia": "2009-12-01",
    "entidad": "INSTITUTO ECUATORIANO DE SEGURIDAD SOCIAL DIRECCION GENERAL PUERTO AYORA",
    "indicador_01": 0,
    "indicador_02": 0,
    "indicador_04": null,
    "indicador_05": 100,
    "indicador_06": null,
    "indicador_09": 0,
    "indicador_11": 0,
    "indicador_12": 0,
    "indicador_18": 0,
    "indicador_19": 100,
    "indicador_22": 25,
    "indicador_25": 99,
    "indicador_27": null,
    "indicador_28": 100,
    "indicador_29": 100
  }
]
```

## Get GEN Proveedor Mes

```python
import http.client

conn = http.client.HTTPSConnection("serverapiSoceIP", 5000)
payload = ''
headers = {}
conn.request("GET", "/api/soce_gen_ic_proveedor/[{
  \"dateRange\":{
    \"dateRangeStart\":{
      \"pickedYear\": \"2008\",
      \"pickedMonth\": \"1\",
      \"pickedDay\": \"1\"
    },
    \"dateRangeEnd\": {
      \"pickedYear\": \"2008\",
      \"pickedMonth\": \"2\",
      \"pickedDay\": \"3\"
    }
  },
  \"timeScale\": \"mensual\",
  \"typeGraph\": 1
}]", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))
```

```javascript
var requestOptions = {
  method: 'GET',
  redirect: 'follow'
};

fetch("http://serverapiSoceIP:5000/api/soce_gen_ic_proveedor/[{\n  \"dateRange\":{\n    \"dateRangeStart\":{\n      \"pickedYear\": \"2008\",\n      \"pickedMonth\": \"1\",\n      \"pickedDay\": \"1\"\n    },\n    \"dateRangeEnd\": {\n      \"pickedYear\": \"2008\",\n      \"pickedMonth\": \"2\",\n      \"pickedDay\": \"3\"\n    }\n  },\n  \"timeScale\": \"mensual\",\n  \"typeGraph\": 1\n}]", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```

Este endpoint devuelve información sobre procesos GEN agrupados por Proveedor, en los cuales se calcularon los indicadores individuales, obteniéndose una calificación promedio que sirve para medir el riesgo en el proceso. Los datos están agrupados mensualmente, por lo tanto, la información se consolida a ese nivel de Proveedor y Mes.

### HTTP Request

`GET http://serverapiSoceIP/api/soce_gen_ic_proveedor/{{process_list}}`

### Definición de parámetros

Element | Description
--------- | -----------
process_list | Parámetro en formato JSON.


### Estructura del parámetro JSON

Element | Description
------- | -----------
dateRage | Elemento json para definir un rango de fechas
dateRangeStart | Elemento json para definir la fecha inicial
dateRangeEnd | Elemento json para definir la fecha final
pickedYear | Elemento para definir el año inicial o final
pickedMonth | Elemento para definir el mes inicial o final (el mes debe estar entre 0 y 11, siendo 0 Enero, y 11 Diciembre)
pickedDay | Elemento para definir el día inicial o final
timeScale | Elemento que indica al endpoint la frecuencia en la que se debe recuperar los datos, en este caso, debe tener el valor "mensual"
typeGraph | Elemnto que define la forma en la que se recuperan los datos, para este caso, debe tener el valor "1"


>Example JSON parameter:

```json
[
  {
  "dateRange":{
    "dateRangeStart":{
      "pickedYear": "2008",
      "pickedMonth": "1",
      "pickedDay": "1"
      },
    "dateRangeEnd": {
      "pickedYear": "2008",
      "pickedMonth": "2",
      "pickedDay": "3"
      }
    },
  "timeScale": "mensual",
  "typeGraph": 1
  }
]
```

### Estructura de la respuesta JSON

Element | Description
------- | -----------
ruc | Elemento que muestra el ruc del proveedor adjudicado
proveedor | Elemento que muestra el nombre del proveedor adjudicado
anio | Elemento que muestra el año relacionado a la fecha de publicación de los procesos
mes | lemento que muestra el mes relacionado a la fecha de publicación de los procesos
avg_indicadores | Elemento que muestra la calificación promedio para el proveedor en el mes analizado
quartiles | Elemento que muestra el valor del cuartil respecto al elemento avg_indicadores
riesgo | Elemento que muestra el nivel de riesgo para el proveedor adjudicado, basado en el cuartil


>Example Response:

```json
[
  {
    "anio": 2008,
    "avg_indicadores": 83.33,
    "mes": 2,
    "proveedor": "TECMAN CIA LTDA.",
    "quartiles": 3,
    "riesgo": "Bajo",
    "ruc": "1791258924001"
  },
  {
    "anio": 2008,
    "avg_indicadores": 70,
    "mes": 2,
    "proveedor": "SANTANDER CASTELLANOS FAUSTO",
    "quartiles": 2,
    "riesgo": "Medio",
    "ruc": "1709225542001"
  },  
  {
    "anio": 2008,
    "avg_indicadores": 49.92,
    "mes": 2,
    "proveedor": "METAL MACANICA SAN RAFAEL",
    "quartiles": 1,
    "riesgo": "Alto",
    "ruc": "1701174391001"
  }
]
```


## Get GEN Proveedor Mes Detalle

```python
import http.client

conn = http.client.HTTPSConnection("serverapiSoceIP", 5000)
payload = ''
headers = {}
conn.request("GET", "/api/soce_gen_ic_proveedor_detail/[{
  \"dateRange\":{
    \"dateRangeStart\":{
      \"pickedYear\": \"2008\",
      \"pickedMonth\": \"1\",
      \"pickedDay\": \"1\"
    },
    \"dateRangeEnd\": {
      \"pickedYear\": \"2008\",
      \"pickedMonth\": \"2\",
      \"pickedDay\": \"3\"
    }
  },
  \"timeScale\": \"mensual\",
  \"typeGraph\": 1
}]", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))
```

```javascript
var requestOptions = {
  method: 'GET',
  redirect: 'follow'
};

fetch("http://serverapiSoceIP:5000/api/soce_gen_ic_proveedor_detail/[{\n  \"dateRange\":{\n    \"dateRangeStart\":{\n      \"pickedYear\": \"2008\",\n      \"pickedMonth\": \"1\",\n      \"pickedDay\": \"1\"\n    },\n    \"dateRangeEnd\": {\n      \"pickedYear\": \"2008\",\n      \"pickedMonth\": \"2\",\n      \"pickedDay\": \"3\"\n    }\n  },\n  \"timeScale\": \"mensual\",\n  \"typeGraph\": 1\n}]", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```

Este endpoint devuelve información detallada sobre procesos GEN agrupados por Proveedor, en los cuales se calcularon los indicadores individuales, obteniéndose una calificación promedio que sirve para medir el riesgo en el proceso. Los datos están agrupados mensualmente, por lo tanto, la información se consolida a ese nivel de Proveedor y Mes.

### HTTP Request

`GET http://serverapiSoceIP/api/soce_gen_ic_proveedor_detail/{{process_list}}`

### Definición de parámetros

Element | Description
--------- | -----------
process_list | Parámetro en formato JSON.


### Estructura del parámetro JSON

Element | Description
------- | -----------
dateRage | Elemento json para definir un rango de fechas
dateRangeStart | Elemento json para definir la fecha inicial
dateRangeEnd | Elemento json para definir la fecha final
pickedYear | Elemento para definir el año inicial o final
pickedMonth | Elemento para definir el mes inicial o final (el mes debe estar entre 0 y 11, siendo 0 Enero, y 11 Diciembre)
pickedDay | Elemento para definir el día inicial o final
timeScale | Elemento que indica al endpoint la frecuencia en la que se debe recuperar los datos, en este caso, debe tener el valor "mensual"
typeGraph | Elemnto que define la forma en la que se recuperan los datos, para este caso, debe tener el valor "1"


>Example JSON parameter:


```json
[
  {
  "dateRange":{
    "dateRangeStart":{
      "pickedYear": "2008",
      "pickedMonth": "1",
      "pickedDay": "1"
      },
    "dateRangeEnd": {
      "pickedYear": "2008",
      "pickedMonth": "2",
      "pickedDay": "3"
      }
    },
  "timeScale": "mensual",
  "typeGraph": 1
  }
]
```

### Estructura de la respuesta JSON

Element | Description
------- | -----------
dia | Elemento que muestra la fecha del inicio del mes en un año, aquí extraemos año y mes para fines de agrupamiento
ruc | Elemento que muestra el ruc del proveedor adjudicado
proveedor | Elemento que muestra el nombre del proveedor adjudicado
indicador_## | Elemento que muestra el valor de calificación del proceso en cada indicador. Se tienen indicadores desde el 01 hasta el 29, el valor de calificación para cada indicador individual se mide en porcentaje que va del 0 al 100%, siendo 0% una mala calificación, y 100% una buena calificación. Refierase a la metodología de cálculo descrita en el sitio web.


>Example Response:

```json
[
  {
    "dia": "2008-03-01",
    "indicador_01": 0,
    "indicador_02": 0,
    "indicador_04": null,
    "indicador_05": 100,
    "indicador_06": null,
    "indicador_09": 0,
    "indicador_11": 0,
    "indicador_12": 100,
    "indicador_18": 0,
    "indicador_19": 50,
    "indicador_22": 50,
    "indicador_25": 99,
    "indicador_27": null,
    "indicador_28": 100,
    "indicador_29": 100,
    "proveedor": "ASTUDILLO PACHECO RICARDO LAURENCIO",
    "ruc": "0100109800001"
  },
  {
    "dia": "2008-03-01",
    "indicador_01": 0,
    "indicador_02": 0,
    "indicador_04": null,
    "indicador_05": 100,
    "indicador_06": null,
    "indicador_09": 0,
    "indicador_11": 0,
    "indicador_12": 0,
    "indicador_18": 0,
    "indicador_19": 50,
    "indicador_22": 50,
    "indicador_25": 99,
    "indicador_27": null,
    "indicador_28": 100,
    "indicador_29": 100,
    "proveedor": "OCHOA ROBLES MARCO VINICIO",
    "ruc": "0102119146001"
  }
]
```


## Get GEN Proveedor Year

```python
import http.client

conn = http.client.HTTPSConnection("serverapiSoceIP", 5000)
payload = ''
headers = {}
conn.request("GET", "/api/soce_gen_ic_proveedor/[{
  \"dateRange\":{
    \"dateRangeStart\":{
      \"pickedYear\": \"2008\",
      \"pickedMonth\": \"1\",
      \"pickedDay\": \"1\"
    },
    \"dateRangeEnd\": {
      \"pickedYear\": \"2008\",
      \"pickedMonth\": \"2\",
      \"pickedDay\": \"3\"
    }
  },
  \"timeScale\": \"anual\",
  \"typeGraph\": 1
}]", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))
```

```javascript
var requestOptions = {
  method: 'GET',
  redirect: 'follow'
};

fetch("http://serverapiSoceIP:5000/api/soce_gen_ic_proveedor/[{\n  \"dateRange\":{\n    \"dateRangeStart\":{\n      \"pickedYear\": \"2008\",\n      \"pickedMonth\": \"1\",\n      \"pickedDay\": \"1\"\n    },\n    \"dateRangeEnd\": {\n      \"pickedYear\": \"2008\",\n      \"pickedMonth\": \"2\",\n      \"pickedDay\": \"3\"\n    }\n  },\n  \"timeScale\": \"anual\",\n  \"typeGraph\": 1\n}]", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```
Este endpoint devuelve información sobre procesos GEN agrupados por Proveedor, en los cuales se calcularon los indicadores individuales, obteniéndose una calificación promedio que sirve para medir el riesgo en el proceso. Los datos están agrupados anualmente, por lo tanto, la información se consolida a ese nivel de Proveedor y Año.

### HTTP Request

`GET http://serverapiSoceIP/api/soce_gen_ic_proveedor/{{process_list}}`

### Definición de parámetros

Element | Description
--------- | -----------
process_list | Parámetro en formato JSON.


### Estructura del parámetro JSON

Element | Description
------- | -----------
dateRage | Elemento json para definir un rango de fechas
dateRangeStart | Elemento json para definir la fecha inicial
dateRangeEnd | Elemento json para definir la fecha final
pickedYear | Elemento para definir el año inicial o final
pickedMonth | Elemento para definir el mes inicial o final (el mes debe estar entre 0 y 11, siendo 0 Enero, y 11 Diciembre)
pickedDay | Elemento para definir el día inicial o final
timeScale | Elemento que indica al endpoint la frecuencia en la que se debe recuperar los datos, en este caso, debe tener el valor "anual"
typeGraph | Elemnto que define la forma en la que se recuperan los datos, para este caso, debe tener el valor "1"


>Example JSON parameter:

```json
[
  {
  "dateRange":{
    "dateRangeStart":{
      "pickedYear": "2008",
      "pickedMonth": "1",
      "pickedDay": "1"
      },
    "dateRangeEnd": {
      "pickedYear": "2008",
      "pickedMonth": "2",
      "pickedDay": "3"
      }
    },
  "timeScale": "anual",
  "typeGraph": 1
  }
]
```

### Estructura de la respuesta JSON

Element | Description
------- | -----------
ruc | Elemento que muestra el ruc del proveedor adjudicado
proveedor | Elemento que muestra el nombre del proveedor adjudicado
anio | Elemento que muestra el año relacionado a la fecha de publicación de los procesos
avg_indicadores | Elemento que muestra la calificación promedio para el proveedor en el mes analizado
quartiles | Elemento que muestra el valor del cuartil respecto al elemento avg_indicadores
riesgo | Elemento que muestra el nivel de riesgo para el proveedor adjudicado, basado en el cuartil


>Example Response:

```json
[
  {
    "anio": 2008,
    "avg_indicadores": 95,
    "proveedor": "MENDOZA ARAGONEZ RENE ANDRES",
    "quartiles": 3,
    "riesgo": "Bajo",
    "ruc": "908605553001"
  },  
  {
    "anio": 2008,
    "avg_indicadores": 74.92,
    "proveedor": "ECUAMUEBLES AMBATO",
    "quartiles": 2,
    "riesgo": "Medio",
    "ruc": "1801594886001"
  },
  {
    "anio": 2008,
    "avg_indicadores": 49.92,
    "proveedor": "GALLARDO CARRASCO MARTHA",
    "quartiles": 1,
    "riesgo": "Alto",
    "ruc": "0912261732001"
  }
]
```



## Get GEN Proveedor Year Detalle

```python
import http.client

conn = http.client.HTTPSConnection("serverapiSoceIP", 5000)
payload = ''
headers = {}
conn.request("GET", "/api/soce_gen_ic_proveedor_detail/[{
  \"dateRange\":{
    \"dateRangeStart\":{
      \"pickedYear\": \"2021\",
      \"pickedMonth\": \"1\",
      \"pickedDay\": \"1\"
    },
    \"dateRangeEnd\": {
      \"pickedYear\": \"2021\",
      \"pickedMonth\": \"2\",
      \"pickedDay\": \"3\"
    }
  },
  \"timeScale\": \"anual\",
  \"typeGraph\": 1
}]", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))
```

```javascript
var requestOptions = {
  method: 'GET',
  redirect: 'follow'
};

fetch("http://serverapiSoceIP:5000/api/soce_gen_ic_proveedor_detail/[{\n  \"dateRange\":{\n    \"dateRangeStart\":{\n      \"pickedYear\": \"2021\",\n      \"pickedMonth\": \"1\",\n      \"pickedDay\": \"1\"\n    },\n    \"dateRangeEnd\": {\n      \"pickedYear\": \"2021\",\n      \"pickedMonth\": \"2\",\n      \"pickedDay\": \"3\"\n    }\n  },\n  \"timeScale\": \"anual\",\n  \"typeGraph\": 1\n}]", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```

Este endpoint devuelve información detallada sobre procesos GEN agrupados por Proveedor, en los cuales se calcularon los indicadores individuales, obteniéndose una calificación promedio que sirve para medir el riesgo en el proceso. Los datos están agrupados anualmente, por lo tanto, la información se consolida a ese nivel de Proveedor y Año.

### HTTP Request

`GET http://serverapiSoceIP/api/soce_gen_ic_proveedor_detail/{{process_list}}`

### Definición de parámetros

Element | Description
--------- | -----------
process_list | Parámetro en formato JSON.


### Estructura del parámetro JSON

Element | Description
------- | -----------
dateRage | Elemento json para definir un rango de fechas
dateRangeStart | Elemento json para definir la fecha inicial
dateRangeEnd | Elemento json para definir la fecha final
pickedYear | Elemento para definir el año inicial o final
pickedMonth | Elemento para definir el mes inicial o final (el mes debe estar entre 0 y 11, siendo 0 Enero, y 11 Diciembre)
pickedDay | Elemento para definir el día inicial o final
timeScale | Elemento que indica al endpoint la frecuencia en la que se debe recuperar los datos, en este caso, debe tener el valor "anual"
typeGraph | Elemnto que define la forma en la que se recuperan los datos, para este caso, debe tener el valor "1"

>Example JSON parameter:

```json
[
  {
  "dateRange":{
    "dateRangeStart":{
      "pickedYear": "2021",
      "pickedMonth": "1",
      "pickedDay": "1"
      },
    "dateRangeEnd": {
      "pickedYear": "2021",
      "pickedMonth": "2",
      "pickedDay": "3"
      }
    },
  "timeScale": "anual",
  "typeGraph": 1
  }
]
```

### Estructura de la respuesta JSON

Element | Description
------- | -----------
dia | Elemento que muestra la fecha del inicio del mes en un año, aquí extraemos el año para fines de agrupamiento
ruc | Elemento que muestra el ruc del proveedor adjudicado
proveedor | Elemento que muestra el nombre del proveedor adjudicado
indicador_## | Elemento que muestra el valor de calificación del proceso en cada indicador. Se tienen indicadores desde el 01 hasta el 29, el valor de calificación para cada indicador individual se mide en porcentaje que va del 0 al 100%, siendo 0% una mala calificación, y 100% una buena calificación. Refierase a la metodología de cálculo descrita en el sitio web.


>Example Response:

```json
[
  {
    "dia": "2021-03-01",
    "indicador_01": 0,
    "indicador_02": 0,
    "indicador_04": null,
    "indicador_05": 100,
    "indicador_06": null,
    "indicador_09": 0,
    "indicador_11": 0,
    "indicador_12": 100,
    "indicador_18": 0,
    "indicador_19": 25,
    "indicador_22": 25,
    "indicador_25": 100,
    "indicador_27": null,
    "indicador_28": 100,
    "indicador_29": 100,
    "proveedor": "INCHACAPE",
    "ruc": "1200003004722"
  },
  {
    "dia": "2021-12-01",
    "indicador_01": 0,
    "indicador_02": 100,
    "indicador_04": null,
    "indicador_05": 100,
    "indicador_06": null,
    "indicador_09": 0,
    "indicador_11": 0,
    "indicador_12": 0,
    "indicador_18": 0,
    "indicador_19": 100,
    "indicador_22": 25,
    "indicador_25": 99,
    "indicador_27": null,
    "indicador_28": 100,
    "indicador_29": 100,
    "proveedor": "VIBARCAL",
    "ruc": "0992839856001"
  }
]
```

## Get GEN Proveedor patron

```python
import http.client

conn = http.client.HTTPSConnection("serverapiSoceIP", 5000)
payload = ''
headers = {}
conn.request("GET", "/api/soce_gen_ic_proveedor/[
  {
    \"dateRange\": {
      \"dateRangeStart\": {
        \"pickedYear\": \"2009\",
        \"pickedMonth\": \"11\",
        \"pickedDay\": \"1\"
      },
      \"dateRangeEnd\": {
        \"pickedYear\": \"2010\",
        \"pickedMonth\": \"0\",
        \"pickedDay\": \"3\"
      }
    },
    \"patron\": \"CASTRO\",
    \"typeGraph\": 2
  }
]", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))
```

```javascript
var requestOptions = {
  method: 'GET',
  redirect: 'follow'
};

fetch("http://serverapiSoceIP:5000/api/soce_gen_ic_proveedor/[\n  {\n    \"dateRange\": {\n      \"dateRangeStart\": {\n        \"pickedYear\": \"2009\",\n        \"pickedMonth\": \"11\",\n        \"pickedDay\": \"1\"\n      },\n      \"dateRangeEnd\": {\n        \"pickedYear\": \"2010\",\n        \"pickedMonth\": \"0\",\n        \"pickedDay\": \"3\"\n      }\n    },\n    \"patron\": \"CASTRO\",\n    \"typeGraph\": 2\n  }\n]", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```

Este endpoint devuelve información detallada sobre procesos GEN agrupados por Proveedor, basados en un patrón o criterio de búsqueda.

### HTTP Request

`GET http://serverapiSoceIP/api/soce_gen_ic_proveedor/{{process_list}}`

### Definición de parámetros

Element | Description
--------- | -----------
process_list | Parámetro en formato JSON.


### Estructura del parámetro JSON

Element | Description
------- | -----------
typeGraph | Elemento que indica el tipo de gráfico que se usará, en ete caso siempre es "2"
patron | Elemento que sirve para pasar un patrón o criterio de búsqueda, el mismo que se puede aplicar en: ruc o nombre del proveedor
dateRage | Elemento json para definir un rango de fechas
dateRangeStart | Elemento json para definir la fecha inicial
dateRangeEnd | Elemento json para definir la fecha final
pickedYear | Elemento para definir el año inicial o final
pickedMonth | Elemento para definir el mes inicial o final (el mes debe estar entre 0 y 11, siendo 0 Enero, y 11 Diciembre)
pickedDay | Elemento para definir el día inicial o final


>Example JSON parameter:

```json
[
  {
    "dateRange": {
      "dateRangeStart": {
        "pickedYear": "2009",
        "pickedMonth": "11",
        "pickedDay": "1"
      },
      "dateRangeEnd": {
        "pickedYear": "2010",
        "pickedMonth": "0",
        "pickedDay": "3"
      }
    },
    "patron": "CASTRO",
    "typeGraph": 2
  }
]
```

### Estructura de la respuesta JSON

Element | Description
------- | -----------
ruc | Elemento que muestra el ruc del proveedor adjudicado
proveedor | Elemento que muestra el nombre del proveedor adjudicado
anio | Elemento que muestra el año de la fecha de publicación del proceso, sobre este año se debe agrupar
mes | Elemento que muestra el mes de la fecha de publicación del proceso, sobre este mes se debe agrupar
promedio | Elemento que muestra la calificación promedio para el proveedor en el año y mes analizados


>Example Response:

```json
[
  {
    "anio": "2009",
    "mes": "12",
    "promedio": "53.27",
    "proveedor": "CASTRO PAZMIÑO ELINA GENOVEVA",
    "ruc": "1801395508001"
  },
  {
    "anio": "2009",
    "mes": "12",
    "promedio": "49.93",
    "proveedor": "Castro Sanchez JOse",
    "ruc": "0904756079001"
  },
  {
    "anio": "2009",
    "mes": "12",
    "promedio": "49.93",
    "proveedor": "Castro Sanchez Jose",
    "ruc": "0904756079001"
  },
  {
    "anio": "2009",
    "mes": "12",
    "promedio": "46.60",
    "proveedor": "Castro Moyano JAmes",
    "ruc": "0909996803001"
  }
]
```

## Get GEN Proveedor patron Detalle

```python
import http.client

conn = http.client.HTTPSConnection("serverapiSoceIP", 5000)
payload = ''
headers = {}
conn.request("GET", "/api/soce_gen_ic_proveedor_detail/[
  {
    \"dateRange\": {
      \"dateRangeStart\": {
        \"pickedYear\": \"2009\",
        \"pickedMonth\": \"11\",
        \"pickedDay\": \"1\"
      },
      \"dateRangeEnd\": {
        \"pickedYear\": \"2010\",
        \"pickedMonth\": \"0\",
        \"pickedDay\": \"3\"
      }
    },
    \"patron\": \"CASTRO\",
    \"typeGraph\": 2
  }
]", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))
```

```javascript
var requestOptions = {
  method: 'GET',
  redirect: 'follow'
};

fetch("http://serverapiSoceIP:5000/api/soce_gen_ic_proveedor_detail/[\n  {\n    \"dateRange\": {\n      \"dateRangeStart\": {\n        \"pickedYear\": \"2009\",\n        \"pickedMonth\": \"11\",\n        \"pickedDay\": \"1\"\n      },\n      \"dateRangeEnd\": {\n        \"pickedYear\": \"2010\",\n        \"pickedMonth\": \"0\",\n        \"pickedDay\": \"3\"\n      }\n    },\n    \"patron\": \"CASTRO\",\n    \"typeGraph\": 2\n  }\n]", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```

Este endpoint devuelve información detallada sobre procesos GEN agrupados por Proveedor, basados en un patrón o criterio de búsqueda.

### HTTP Request

`GET http://serverapiSoceIP/api/soce_gen_ic_proveedor_detail/{{process_list}}`

### Definición de parámetros

Element | Description
--------- | -----------
process_list | Parámetro en formato JSON.


### Estructura del parámetro JSON

Element | Description
------- | -----------
typeGraph | Elemento que indica el tipo de gráfico que se usará, en ete caso siempre es "2"
patron | Elemento que sirve para pasar un patrón o criterio de búsqueda, el mismo que se puede aplicar en: ruc o nombre del proveedor
dateRage | Elemento json para definir un rango de fechas
dateRangeStart | Elemento json para definir la fecha inicial
dateRangeEnd | Elemento json para definir la fecha final
pickedYear | Elemento para definir el año inicial o final
pickedMonth | Elemento para definir el mes inicial o final (el mes debe estar entre 0 y 11, siendo 0 Enero, y 11 Diciembre)
pickedDay | Elemento para definir el día inicial o final


>Example JSON parameter:


```json
[
  {
    "dateRange": {
      "dateRangeStart": {
        "pickedYear": "2009",
        "pickedMonth": "11",
        "pickedDay": "1"
      },
      "dateRangeEnd": {
        "pickedYear": "2010",
        "pickedMonth": "0",
        "pickedDay": "3"
      }
    },
    "patron": "CASTRO",
    "typeGraph": 2
  }
]
```

### Estructura de la respuesta JSON

Element | Description
------- | -----------
ruc | Elemento que muestra el ruc del proveedor adjudicado
proveedor | Elemento que muestra el nombre del proveedor adjudicado
dia | Elemento que muestra la fecha del inicio del mes en un año, aquí extraemos el año y mes para fines de agrupamiento
indicador_## | Elemento que muestra el valor de calificación del proceso en cada indicador. Se tienen indicadores desde el 01 hasta el 29, el valor de calificación para cada indicador individual se mide en porcentaje que va del 0 al 100%, siendo 0% una mala calificación, y 100% una buena calificación. Refierase a la metodología de cálculo descrita en el sitio web.

>Example Response:

```json
[
  {
    "dia": "2009-12-01",
    "indicador_01": 0,
    "indicador_02": 0,
    "indicador_04": null,
    "indicador_05": 100,
    "indicador_06": null,
    "indicador_09": 0,
    "indicador_11": 0,
    "indicador_12": 0,
    "indicador_18": 0,
    "indicador_19": 50,
    "indicador_22": 50,
    "indicador_25": 99,
    "indicador_27": null,
    "indicador_28": 100,
    "indicador_29": 100,
    "proveedor": "CASTRO RIVERA WILSON",
    "ruc": "0101538312001"
  },
  {
    "dia": "2009-12-01",
    "indicador_01": 0,
    "indicador_02": 0,
    "indicador_04": null,
    "indicador_05": 100,
    "indicador_06": null,
    "indicador_09": 0,
    "indicador_11": 0,
    "indicador_12": 0,
    "indicador_18": 0,
    "indicador_19": 25,
    "indicador_22": 50,
    "indicador_25": 99,
    "indicador_27": null,
    "indicador_28": 100,
    "indicador_29": 100,
    "proveedor": "CASTRO RIVERA WILSON CARLOS",
    "ruc": "0101538312001"
  }
]
```



# SIE-GEN

## SIE-GEN process

```python
import http.client

conn = http.client.HTTPSConnection("serverapiSoceIP", 5000)
payload = ''
headers = {}
conn.request("GET", "/api/soce_sie_gen_year_num_process", payload, headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))
```

```javascript
var requestOptions = {
  method: 'GET',
  redirect: 'follow'
};

fetch("http://serverapiSoceIP:5000/api/soce_sie_gen_year_num_process", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
```

Este endpoint devuelve información sobre el número de procesos SIE y GEN por año.

### HTTP Request

`GET http://serverapiSoceIP/api/soce_sie_gen_year_num_process`

### Estructura de la respuesta JSON

Element | Description
------- | -----------
gen | Elemento que muestra el número de procesos GEN
sie | Elemento que muestra el número de procesos SIE
year | Elemento que muestra el año de agrupamiento


>Example Response:

```json
[  
  {
    "gen": 38986,
    "sie": 2413,
    "year": 2008
  },
  {
    "gen": 139427,
    "sie": 31815,
    "year": 2009
  },
  {
    "gen": 129575,
    "sie": 50391,
    "year": 2010
  },
  {
    "gen": 14459,
    "sie": 12088,
    "year": 2011
  },
  {
    "gen": 29811,
    "sie": 31812,
    "year": 2012
  }
]
```

