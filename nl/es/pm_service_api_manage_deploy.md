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

# Despliegue o renovación de un modelo de predicción


PUT http://{PA Bluemix load balancer
URL}/pm/v1/model/{contextId}?accesskey={access_key for this bound
application}

Utilice esta llamada a la API para cargar un archivo que contenga el sistema de puntuación desarrollado por IBM SPSS Modeler que desea desplegar.
Está disponible para puntuar los datos de las aplicaciones. A cada archivo del modelo se le asigna un ID de contexto, que sirve como alias para hacer referencia al modelo desplegado en las siguientes llamadas al servicio. Si un modelo ya existe para un ID de contexto, se sustituye por esta llamada PUT como método de renovar el análisis predictivo que utilizan las aplicaciones.

Ejemplo de solicitud: 

```
    Content-Type: multipart/form-data
    Parámetros:
        Parámetros de formulario:
            model_file: archivo de modelo a cargar
        Parámetros de vía de acceso:
            contextId: identificador único que se asigna a su modelo o una referencia al modelo desplegado para renovar
        Parámetros de consulta:
            accesskey: access_key de env.VCAP_SERVICES
```
{: codeblock}

Respuesta cuando el despliegue se ejecuta correctamente: 

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

Respuesta cuando el despliegue falla: 

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
