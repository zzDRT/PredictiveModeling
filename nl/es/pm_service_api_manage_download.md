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

# Descarga una copia de un determinado archivo del modelo desplegado


GET http://{PA Bluemix load balancer
URL}/pm/v1/model/{contextId}?accesskey={access_key for this bound
application}

Utilice esta llamada a la API para descargar una copia de un determinado archivo del modelo desplegado.

Ejemplo de solicitud: 

```
    Content-Type: */*
    Parámetros:
        Parámetros de vía de acceso:
            contextId: el identificador del modelo de predicción desplegado que desea descargar
        Parámetros de consulta:
            accesskey: access_key de env.VCAP_SERVICES
```
{: codeblock}

Respuesta cuando la solicitud de descarga se ejecuta correctamente: 

```
    Content-Type: application/octet-stream
    Status code: 200
    Header:
        "Content-Disposition":"attachment; filename=model_file_name.str"
```
{: codeblock}

Respuesta cuando la solicitud de descarga falla:

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":false, 
           "message":String // failure reason 
        }
```
{: codeblock}
