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

# Creación de aplicaciones de análisis predictivo


En esta sección se describe el proceso para desplegar y utilizar una aplicación de ejemplo.

**Nombre del caso de ejemplo**: Predicción de línea de productos. 

**Descripción del caso de ejemplo**: Nuestro cliente es el propietario de una de las cadenas de grandes almacenes más famosas de Europa. Quieren que estudiemos los intereses de sus clientes en relación con su línea de productos, como Accesorios personales, Equipamiento de camping y Protección aire libre.
Un científico de los datos desarrolla un modelo de predicción y lo comparte con el cliente (el desarrollador). La tarea del cliente consiste en preparar y desplegar la aplicación Node.js que recomienda líneas de productos deportivos mediante la realización de solicitudes de puntuaciones sobre el modelo desplegado.

1. Inicie la sesión en Bluemix y cree una instancia de Machine Learning.

2. Inicie el panel de control de Machine Learning.

3. Vaya al separador Ejemplos.

4. En la sección Modelos de ejemplo, busque el mosaico Predicción de línea de productos y pulse el botón Añadir modelo (+). Ahora verá el modelo de predicción de línea de productos de ejemplo en la lista de modelos disponibles en el separador Modelos.

   **Nota**: Si desea utilizar su propio proyecto y modelos de Data Science Experience en lugar de los ejemplos, debe guardar de forma permanente un modelo personalizado en el repositorio de Machine Learning. Para ello puede utilizar la API REST o bibliotecas de cliente. Para obtener información más detallada, consulte Desarrollo y permanencia del modelo personalizado. 

5. En el menú Acción, seleccione Crear despliegue para desplegar el modelo de predicción de línea de productos como despliegue en línea.

6. Especifique el nombre del despliegue (por ejemplo, Predicción de línea de productos) y pulse Guardar. Ahora debería ver el despliegue Predicción de línea de productos en la lista Despliegues.

7. En el menú Acción, seleccione Ver detalles para su despliegue. El Punto final de puntuación que se muestra en el panel de detalles se utilizará para ejecutar solicitudes de puntuación.

8. Para ejecutar solicitudes de puntuación mediante la API REST, se necesita un punto final de puntuación y una señal de autorización. Para obtener más información, consulte Despliegue de modelos en línea.

9. Ahora puede experimentar con la aplicación de ejemplo Node.js que está disponible en 
   https://github.com/pmservice/product-line-prediction.

   Para desplegar de forma automática el código de aplicación de ejemplo en el espacio de Bluemix, vaya al separador Ejemplos y, en la sección Aplicaciones de ejemplo, seleccione la aplicación Predicción de línea de productos y despliéguela pulsando el botón (+).
Autentíquese en el formulario de DeployToBluemix si se le solicita.

   Ahora debería ver la aplicación de ejemplo en el Panel de control de Bluemix en la sección Todas las apps.

10. Pulse en la aplicación para ver sus detalles. Aquí puede modificar los detalles de la aplicación, como el número de instancias, la memoria, etc.

11. Ahora puede enlazar la aplicación con la instancia de Machine Learning. En el separador Conexiones, pulse Conectar existente. Después de volverse a transferir, la aplicación se conecta a la instancia de servicio.

12. Ejecute la aplicación utilizando la dirección de ruta.

13. En la aplicación, seleccione su despliegue de Predicción de línea de productos. Ahora está preparado para trabajar con los registros de ejemplo y los resultados de la predicción.
