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

# Utilización del servicio Machine Learning con modelos IBM
SPSS Modeler

Los métodos de modelado disponibles en la paleta de modelado de SPSS Modeler le permiten obtener nueva información a partir de sus datos y desarrollar modelos de predicción. Cada método tiene determinados puntos fuertes y resulta más adecuado para ciertos tipos de problemas. 
{: .shortdesc}

Para obtener detalles sobre SPSS Modeler y los algoritmos de modelado que proporciona, consulte [IBM
Knowledge Center](https://www.ibm.com/support/knowledgecenter/SS3RA7).

Después de implementar los requisitos de entrada y salida de la aplicación Bluemix y el diseño del sistema de puntuación de SPSS Modeler, el analista de datos puede modificar cualquier aspecto interno del sistema de puntuación. El analista de datos puede incluso cambiar los algoritmos del modelo utilizados en una operación de renovación, lo que garantiza la capacidad de ajustar mejor el análisis predictivo sin tener que volver a escribir las aplicaciones.

Recuerde la información importante que se proporciona a continuación sobre el servicio Machine Learning cuando se utiliza con modelos creados en SPSS Modeler:

*  Cuando se prepara una rama de puntuación para su uso en puntuaciones en tiempo real, los datos de entrada procedentes de la solicitud de puntuación deben sustituir al nodo de origen diseñado en la rama de puntuación y el resultado del análisis predictivo debe volver atrás al flujo de respuesta (reemplazando de forma efectiva el nodo terminal en el diseño de rama de puntuación).

*  Como la rama de puntuación está preparada para la ejecución en tiempo real en Bluemix, no necesita una conexión a un servicio externo. Por ejemplo, un diseño de rama de puntuación de IBM Analytical Decision Management no puede incluir referencias a reglas o modelos almacenados en un repositorio de IBM SPSS Collaboration and
Deployment Services.

*  La ejecución de una rama de puntuación para la puntuación en tiempo real en Bluemix no necesita un servicio externo. Por ejemplo, no puede desplegar y puntuar en algoritmos de modelo que requieran un almacén de datos de IBM SPSS
Analytic Server y Apache Hadoop en tiempo real.

*  Machine Learning da soporte a los scripts Python incluidos en Modeler. Existen unas pocas restricciones debido al método usado para procesar secuencias antes de ejecutarlas en Machine Learning. Normalmente, si un usuario decide controlar la ejecución de la secuencia, hará referencia al nodo terminal de la rama.
   Para Machine Learning, al procesar la secuencia, identificamos los nodos de JSON que se alterarán temporalmente y, a continuación, realizaremos la sustitución antes de que se ejecute la secuencia. Ello provoca que la secuencia falle en el script porque los nodos de exportación y la entrada a la que se hace referencia ya no existen. La solución pasa por utilizar el ID de otro nodo para identificar la rama de forma exclusiva durante la ejecución. Así se garantiza que la secuencia se ejecuta tal como se define en el script Python incluido.

Para obtener más detalles sobre el soporte actual para los modelos predictivos formados de IBM SPSS Analytic Server, consulte la sección Analytic Server de IBM Knowledge Center.

1. Puede descargar el código de ejemplo Node.js para probar el servicio Machine Learning. Siga los pasos siguientes para crear una aplicación Bluemix y enlazar el servicio Machine Learning. En estos ejemplos se utiliza Node.js porque es un tiempo de ejecución muy usado.
   Se pueden utilizar otros, como por ejemplo iOS, Ruby, Perl o
        Java.

2. Utilice el mandato cf create-service para crear una instancia del servicio:

   ```
   cf create-service pm-20 Free {local naming}
   ```

   Por ejemplo:

   ```
   cf create-service pm-20 Free my_pm_free
   ```

   Este mandato crea una instancia del servicio Machine Learning con el plan Free llamado my_pm_free en el espacio de Bluemix.

3. Utilice el mandato `cf create-service-key` para crear credenciales de servicio:

   ```cf create-service-key "{service instance name}" {vcap key name}```

   Por ejemplo:

   ```cf create-service-key "IBM Watson Machine Learning - my instance" Credentials-1```

   Este mandato crea credenciales de servicio Machine Learning.
4. Utilice el mandato cf bind-service para enlazar la instancia de servicio my_pm_free a la aplicación.

   ```cf bind-service AppName my_pm_service```

   Por ejemplo:

   ```cf bind-service my_app1 my_pm_free```

   Este mandato enlaza la instancia de servicio
   `my_pm_free` de Machine Learning con la aplicación my_app1 de Bluemix.

5. Credenciales de Machine Learning: 

   Después de enlazar la instancia del servicio Machine Learning a la aplicación Bluemix, las credenciales de Machine Learning se añaden a la variable de entorno `VCAP_SERVICES`: 

```
    {   
        "pm-20": {      
            "name": "pm20-1",
            "label": "pm-20",
            "plan": "Free",
            "credentials": {
                "url": "https://ibm-watson-ml.mybluemix.net",
                "access_key": "XXXXXXXXXXXXX"
            }
        }       
    } 
```

   La variable de entorno `VCAP_SERVICES` incluye la siguiente información:

   <dl>
   
   <dt>plan</dt>
   <dd>El plan de Machine Learning que se utiliza en el suministro del servicio.</dd>

   <dt>url</dt>
   <dd>La dirección de la instancia del servicio Machine Learning.</dd>

   <dt>access_key</dt>
   <dd>El parámetro de la consulta accessKey que se va a pasar en todas las solicitudes a esta instancia de servicio.</dd>

   </dl>
            
Por ejemplo:             

```
Get https://ibm-watson-ml.mybluemix.net/pm/v1/model/sales_model2?accesskey=XXXXXXXXXXXXX 
```

   Código Node.js de ejemplo que muestra cómo obtener el valor de accessKey a partir de la variable de entorno `VCAP_SERVICES`: 

```
   if (process.env.VCAP_SERVICES) {
        var env = JSON.parse(process.env.VCAP_SERVICES);
        var credentials = env['pm-20'][0].credentials;
        var accessKey = credentials.access_key;
    }
```
