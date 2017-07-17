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

# Desarrollo de aplicaciones que aprovechan los modelos SPSS desplegados


*  Puntuación con un modelo de predicción desplegado

*  Recuperación de metadatos correspondientes a un modelo de predicción

*  Recuperación del resumen de Web Application Description Language (WADL) de este servicio

PUNTUACIÓN CON UN MODELO PREDICTIVO DESPLEGADO

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

RECUPERACIÓN DE METADATOS PARA UN MODELO PREDICTIVO DESPLEGADO

GET http://{service
instance}/pm/v1/metadata/{contextId}?accesskey={access_key for
this bound application}&metadatatype=score

Utilice esta llamada a la API para recuperar metadatos correspondientes al sistema de puntuación de una secuencia de IBM SPSS Modeler desplegada. No especifique el cuerpo de la solicitud con este método.

Ejemplo de solicitud: 

```
    Content-Type: application/json;charset=UTF-8
    Parámetros:
        Parámetros de vía de acceso:
            contextId: el identificador del modelo desplegado que se va a utilizar para procesar esta solicitud de recuperación de metadatos
        Parámetros de consulta:
            accesskey: access_key de env.VCAP_SERVICES
```
{: codeblock}

Ejemplo de una respuesta correcta a la solicitud anterior:

```
    Content-Type: application/json;charset=UTF-8
    Status code: 200
    body: los metadatos (formateados en este documento para una mejor legibilidad) en
          la rama de puntuación, por ejemplo.
    [
      {
        flag: true
        message: 
          "Model Score Input Metadata: 
          <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
          <metadata xmlns="http://spss.ibm.com/meta/internal">
          <table xsi:type="nodeImpl" tag="id574QKQ8NL6E" name="baskrule_input.csv" 
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
            <field measurementLevel="CATEGORICAL" storageType="STRING" name="gender"/>
            <field measurementLevel="CONTINUOUS" storageType="LONG" name="income"/>
          </table>
          </metadata> 
          Model Score Output Metadata:
          <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
          <metadata xmlns="http://spss.ibm.com/meta/internal">
          <table xsi:type="nodeImpl" tag="id32CPAJBGJFG" name="Flat File" 
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
            <field measurementLevel="CATEGORICAL" storageType="STRING" name="gender"/>
            <field measurementLevel="CONTINUOUS" storageType="LONG" name="income"/>
            <field measurementLevel="FLAG" storageType="STRING" name="$C-beer_beans_pizza"/>
            <field measurementLevel="CONTINUOUS" storageType="DOUBLE" name="$CC-beer_beans_pizza"/>
          </table>
          </metadata>"
      }
    ]
```
{: codeblock}

Respuesta cuando una solicitud de puntuación falla:

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

RECUPERACIÓN DEL RESUMEN DEL LENGUAJE DE DESCRIPCIÓN DE APLICACIÓN WEB (WADL) DE ESTE SERVICIO

OPCIONES http://{PA Bluemix load balancer URL}/pm/v1/wadl

Utilice esta llamada a la API para recuperar el WADL correspondiente a este servicio.

Ejemplo de solicitud: 

```
    Content-Type: */*
```
{: codeblock}

Respuesta cuando la solicitud WADL se ejecuta correctamente: 

```
    Content-Type: application/vnd.sun.wadl+xml
    Status code: 200
    body: the WADL XML ,eg.
        <?xml version="1.0" encoding="UTF-8" standalone="yes" ?>
            <application>
                <resources base="http://{PA Bluemix load balancer URL}/pm/v1/">
                    <resource path="score">
                        <doc title="Score using the model with given context ID" />
                        <resource path="{id}">
                            <param style="template" name="id" />
                            <method name="POST">
                                <request>
                                    <param style="query" name="accesskey" />
                                    <representation mediaType="application/json;charset=UTF-8" />
                                </request>
                                <response>
                                    <representation mediaType="application/json;charset=UTF-8" />
                                </response>
                            </method>
                        </resource>
                     </resource>
                     ...

             </resources>
        </application>
```
{: codeblock}

Respuesta cuando la solicitud WADL falla:

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
