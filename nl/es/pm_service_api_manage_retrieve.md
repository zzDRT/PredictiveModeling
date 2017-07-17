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

# Recuperación de una lista de todos los modelos desplegados actualmente


GET http://{PA Bluemix load balancer
URL}/pm/v1/model?accesskey={access_key for this bound
application}

Recupere un resumen de todos los modelos actualmente desplegados en esta instancia del servicio.

Ejemplo de solicitud: 

```
    Content-Type: */*
    Parámetros:
        Parámetros de consulta:
            accesskey: access_key de env.VCAP_SERVICES
```
{: codeblock}

Respuesta cuando la solicitud de resumen de un modelo desplegado se ejecuta correctamente: 

```
    Content-Type: application/json
    Status code: 200
    body: una matriz vacía si no se ha desplegado ningún modelo o un resumen de los modelos desplegados...
        [
            {
                "stream":"neural_net1.str",
                "id":"1"
            },
            {
                "stream":"neural_net2.str",
                "id":"2"
            },
            ...
        ]
```
{: codeblock}

Respuesta cuando la solicitud de resumen de un modelo desplegado falla: 

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
