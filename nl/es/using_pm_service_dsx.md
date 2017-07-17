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

# Utilización del servicio Machine Learning con modelos Spark y Python


Recuerde la información importante que se proporciona a continuación sobre el servicio Machine Learning:

*  Infraestructuras de Machine Learning soportadas: 

  *  Spark 2.0 MLlib
  *  Python 3 con scikit-learn 0.17

*  No se da soporte a los modelos no supervisados.

*  Actualmente no se da soporte al despliegue continuo y por lotes para los modelos Python scikit-learn 

*  El soporte al despliegue continuo y por lotes para modelos Spark está en una fase beta. Si le interesa participar, póngase en lista de espera. Para obtener más información, consulte [https://www.ibm.biz/mlwaitlist](https://www.ibm.biz/mlwaitlist).

Puede descargar el código de ejemplo Node.js para probar el servicio Machine Learning. Siga los pasos siguientes para crear una aplicación Bluemix y enlazar el servicio Machine Learning. En estos ejemplos se utiliza Node.js porque es un tiempo de ejecución muy usado. Se pueden utilizar otros, como por ejemplo iOS, Ruby, Perl o
        Java.

Tenga en cuenta que también puede realizar los pasos 1- 3 utilizando la interfaz gráfica de Bluemix en lugar de la herramienta Cloud Foundry (cf).

1. Utilice el mandato cf create-service para crear una instancia del servicio:

   ```
   cf create-service pm-20 Free {local naming}
   ```
{: codeblock}

   Por ejemplo:

   ```
   cf create-service pm-20 Free my_wml_free
   ```
{: codeblock}

   Este mandato crear una instancia de servicio de Machine Learning con un plan Free denominado ```my_wml_free``` en su espacio de Bluemix. 

2. Utilice el mandato cf create-service-key para crear credenciales de servicio:

   ```
   cf create-service-key "{service instance name}" {vcap key name}
   ```
{: codeblock}

   Por ejemplo:

   ```
   cf create-service-key "IBM Watson Machine Learning - my instance" Credentials-1
   ```
{: codeblock}

      Este mandato crea credenciales de servicio Machine Learning.


3. Utilice el mandato cf bind-service para enlazar la instancia de servicio 
   ```my_wml_free``` to your application.

   ```
   cf bind-service {AppName} my_wml_free
   ```
{: codeblock}

   Por ejemplo:

   ```
   cf bind-service my_app1 my_wml_free
   ```
{: codeblock}

   Este mandato enlaza la instancia del servicio my_wml_free de Machine Learning a la aplicación de Bluemix my_app1.



Credenciales de Machine Learning: 

   Después de enlazar la instancia del servicio Machine Learning a la aplicación Bluemix, las credenciales de Machine Learning se añaden a la variable de entorno VCAP_SERVICES: 

```
    {
     "pm-20-dev": [
       {
         "credentials": {
           "url":
           "https://ibm-watson-ml.mybluemix.net",
           "access_key": "**********",
           "username": "**********",
           "password": "**********",
           "instance_id": "**********"
         },
           "label": "pm-20-dev",
           "plan": "Free",
           "name": "IBM Watson Machine Learning”
       }
     ]
    }
```
{: codeblock}

   La variable de entorno VCAP_SERVICES incluye la siguiente información: 

   * plan - El plan de Machine Learning que se utiliza en el suministro del servicio.

   * url - La dirección de la instancia del servicio Machine Learning.

   * acces_key - Solo para secuencias de SPSS Modeler.

   * instance_id - El identificador exclusivo de la instancia del servicio Machine Learning. 

   * username, password - Autorización básica necesaria para generar una señal de acceso que se pasará en todas las solicitudes a esta instancia de servicio. Por ejemplo, puede generar una señal de acceso utilizando un curl como este:

Ejemplo de solicitud: 

```
       curl --basic --user username:password https://ibm-watson-ml.mybluemix.net/v2/identity/token

       Ejemplo de salida:

       {"token":"**********"}
```
{: codeblock}

   El siguiente código Node.js es un ejemplo de cómo obtener los valores username y password de la variable de entorno VCAP_SERVICES: 

```
   if (process.env.VCAP_SERVICES) {
        var env = JSON.parse(process.env.VCAP_SERVICES);
        var credentials = env['pm-20'][0].credentials;
        var username = credentials.username;
        var password = credentials.password;
        var instance_id = credentials.instance_id;
    }
```
{: codeblock}
