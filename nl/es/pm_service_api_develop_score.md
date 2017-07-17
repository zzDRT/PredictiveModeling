---

copyright:
  years: 2016, 2017
lastupdated: "2017-06-23"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Puntuación con un modelo de predicción desplegado


POST http://{PA Bluemix load balancer
URL}/pm/v1/score/{contextId}?accesskey={access_key for this bound
application}

Utilice esta llamada a la API que publicar los datos de entrada que utilizará el modelo de desplegado para generar y devolver el análisis predictivo en los resultados de la puntuación.

Ejemplo de solicitud: 

```
    Content-Type: application/json;charset=UTF-8
    Parámetros:
        Parámetros de vía de acceso:
            contextId: el identificador del modelo desplegado que se va a utilizar para procesar esta solicitud de puntuación
        Parámetros de consulta:
            accesskey: access_key de env.VCAP_SERVICES
        Cuerpo: datos de entrada, serie json, ...
            {
                "tablename":"DRUG1n.sav", 
                "header":["Age", "Sex", "BP", "Cholesterol", "Na", "K", "Drug"], 
                "data":[[43.0, "M", "LOW", "NORMAL", 0.526102, 0.027164, "drugY"]]
            }   
```
{: codeblock}

Ejemplo de una respuesta correcta a la solicitud anterior:

```
    Content-Type: application/json;charset=UTF-8
    Status code: 200
    body: el resultado de una puntuación, matriz json, ...
        [
            {
                "header":["Age","Sex","BP","Cholesterol","Na""K","Drug","$N-Drug","$NC-Drug"], 
                "data":[[23.0,"M","NORMAL","NORMAL",0.78452,0.055959,"drugX","drugX",0.9892564426956728]]
            }
        ]
```
{: codeblock}

Respuesta cuando la solicitud de puntuación falla:

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":false, 
           "message":"reason"
        }  
```
{: codeblock}
