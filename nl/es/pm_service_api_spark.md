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

# API de servicio Machine Learning para modelos Spark y Python


El servicio Machine Learning es un conjunto de [API REST](https://watson-ml-api.mybluemix.net/) que se invocan desde cualquier lenguaje de programación y que permiten integrar análisis desarrollados por Data Science Experience en sus aplicaciones.
Enlace sus aplicaciones Bluemix a la instancia del servicio Machine Learning y genere el análisis predictivo que necesitan las aplicaciones para mejorar el servicio a los usuarios. Gestione sus modelos en el panel de control de administración y utilice el panel de control para crear en línea, ejecutar por lotes o ejecutar en modalidad continua despliegues integrados con sus aplicaciones.

Gestione los archivos de Data Science Experience (modelos Spark y Python) que están disponibles en una instancia del servicio Machine Learning mediante el panel de control de Watson Machine Learning: 

*  Importe modelos de ejemplo 

*  Visualice detalles del modelo

*  Despliegue un modelo como:

   *  Despliegue en línea (puntuación)

   *  Despliegue por lotes (con soporte de IBM Object Storage y DashDB)

   *  Despliegue en  modalidad continua (con soporte de IBM MessageHub)

*  Obtenga detalles del despliegue

*  Suprima un despliegue


Desarrolle aplicaciones sobre los archivos de Data Science Experience (modelos Spark y Python) desplegados en una instancia de servicio mediante las potentes [REST API](https://watson-ml-api.mybluemix.net/): 

*  Recupere los metadatos correspondientes a un determinado modelo de predicción

*  Despliegue modelos y gestione modelos desplegados

*  Recupere los metadatos correspondientes a un determinado despliegue

*  Genere análisis predictivos realizando solicitudes de puntuación a los modelos desplegados

Puede explorar fácilmente las funciones del servicio Machine Learning mediante la [Representación Swagger de la API REST disponible](https://watson-ml-api.mybluemix.net/).


Consulte las secciones siguientes para ver ejemplos de API REST de:

*  [Despliegue en línea y puntuación](pm_service_api_spark_online.html)

*  [Despliegue por lotes con Object Storage](pm_service_api_spark_batch.html)

*  [Despliegue en modalidad continua con MessageHub](pm_service_api_spark_streaming.html)
