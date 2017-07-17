---

copyright:
  years: 2016, 2017
lastupdated: "2017-06-21"

---
<!-- Copyright info and last updated date at top of file: REQUIRED
    The copyright and lastupdated info is YAML content that must occur at the top of the MD file, before attributes are listed.
    It must be --- surrounded by 3 dashes ---
    The value "years" can contain just one year or a two years separated by a comma. (years: 2014, 2016)
    The value "lastupdated" must be followed by a machine date in quotes in the following format: "YYYY-MM-DD"
    The value for "years" must be indented 2 spaces under "copyright", followed by "lastupdated" which should start on its own non-indented line.

-->

<!-- Common attributes used in the template are defined as follows: -->
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Desarrollo y permanencia del modelo personalizado

## Cómo trabajar con modelos personalizados

### Nombre del caso de ejemplo: Predicción de línea de productos.

### Descripción del caso de ejemplo: 

Nuestro cliente es el propietario de una de las cadenas de grandes almacenes más famosas de Europa. Quieren que estudiemos los intereses de sus clientes en relación con su línea de productos, como Accesorios personales, Equipamiento de camping y Protección aire libre.
Un científico de los datos desarrolla un modelo de predicción y lo comparte con el cliente (el desarrollador). La tarea del cliente consiste en desplegar el modelo y generar análisis predictivos mediante la realización de solicitudes de puntuaciones sobre el modelo desplegado.

### Desarrollo y permanencia del modelo personalizado

1. Utilice Data Science Experience para crear modelos personalizados. Después de registrarse, entre y siga los pasos siguientes.

2. La primera vez que se registre, se le solicitará que cree una organización y un espacio. Pulse **Continuar** y acepte los valores predeterminados.

3. Una vez creada la organización, vaya a **Mis proyectos** y pulse **crear proyecto**.

4. Especifique un nombre y una descripción para el proyecto y pulse **Crear**. El nombre del proyecto que especifique se utilizará también como nombre del contenedor de destino.

5. Una vez creado el proyecto, puede añadir cuadernos y empezar a desarrollar sus propios modelos, o bien puede cargar uno de los siguientes cuadernos de ejemplo:

   *  [Desarrollo de modelos Spark con Python](https://apsportal.ibm.com/analytics/notebooks/89492fd6-a641-4819-9176-3d9381561df9/view?access_token=d80bef1a172d1d83d3721b101886337158457281774186f181a2e6a5b57f5ec7)

   *  [Desarrollo de modelos Spark con Scala](https://apsportal.ibm.com/analytics/notebooks/c8652d2c-bfc9-4354-8168-f1c9f7f8dfc2/view?access_token=02a83fea8450a452c8de76af98dae078459d0f56810ddef4f4c62d5bc4fc72cf)

   *  [Desarrollo de modelos scikit-learn con Python](https://apsportal.ibm.com/analytics/notebooks/5215a61a-16d7-4fa2-b060-e3e243ceebe3/view?access_token=70f48c95c5571a614ce97484d3f168b1d9b6aeebce015187d3d77ce6038f025e)



### Despliegue y puntuación del modelo personalizado

Consulte las secciones siguientes para ver instrucciones sobre cómo desplegar y puntuar modelos o bien consulte la sección sobre puntuación de los cuadernos enlazados anteriormente.

*  [Despliegue de modelos en línea](pm_service_api_spark_online.html)

*  [Despliegue de modelos por lotes](pm_service_api_spark_batch.html)

*  [Despliegue de modelos en modalidad continua](pm_service_api_spark_streaming.html)
