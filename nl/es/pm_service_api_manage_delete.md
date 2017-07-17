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

# Supresión de un modelo de predicción desplegado


DELETE http://{service
instance}/pm/v1/model/{contextId}?accesskey={access_key for this
bound application}

Utilice esta llamada a la API para suprimir el modelo de predicción de la instancia del servicio Machine Learning. Después esta llamada, el modelo de predicción dejará de estar disponible para su descarga o para puntuar datos en las aplicaciones.

Ejemplo de solicitud: 

```
    Content-Type: */*
    Parámetros:
        Parámetros de vía de acceso:
            contextId: el identificador del modelo desplegado cuyo despliegue desea anular y que desea suprimir
        Parámetros de consulta:
            accesskey: access_key de env.VCAP_SERVICES
```
{: codeblock}

Respuesta cuando la eliminación del despliegue se ejecuta correctamente:

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":true, 
           "message":"detailed information"  
         }
```
{: codeblock}

Respuesta cuando la eliminación del despliegue falla: 

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
