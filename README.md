![DANELOGO](Figuras/dane_logo.PNG)
 
 # Percepción de Confianza [Mintic]

La medición de la percepción de la confianza en el instituto nacional de estadísticas se enmarca en el propósito de la  (OCDE, 2017) de fortalecer las estadísticas oficiales para la medición del capital social, como parte de un sistema robusto para la comprensión y medición del bienestar. Surgió como respuesta a la Iniciativa de Paris 21 promueve la medición de la confianza de los ciudadanos en los institutos nacionales de estadísticas, los sistemas estadísticos nacionales y las estadísticas oficiales. La confianza en las estadísticas, según (PARIS 21, 2019) se entiende como la credibilidad en los productos estadísticos producidos en el Sistema Estadístico Nacional, el cual actúa como un proveedor de formación estadística creíble, precisa y oportuna libre de interferencia política inapropiada. 
En este sentido, el objetivo del proyecto es medir la percepción y confianza que tienen los usuarios de Twitter frente al DANE y a los productos estadísticos, a través del uso de fuentes alternativas para la captura de datos y el diseño e implementación de técnicas y metodologías de procesamiento de lenguaje natural y análisis de sentimientos.
La información estadística derivada de este proyecto contribuye a conocer la percepción de los diferentes usuarios en las estadísticas producidas por el DANE, desagregada a nivel geográfico, así como conocer quiénes son los usuarios más activos en Twitter, los temas más recurrentes y cuál es la polaridad de sus opiniones, y aspectos como la relación de esta percepción con las diferentes publicaciones del DANE, lo cual puede contribuir a la gestión de la difusión y la cultura estadística en el DANE. 
Este repositorio contiene las bases de datos de los Tweets publicados por DANE_Colombia, desde octubre de 2020 hasta enero de 2021, descargados mediante técnicas de web scraping mediante el [API de Twitter](https://developer.twitter.com/en/docs/twitter-api), así como los diccionarios de datos de estas bases. Este ejercicio exploratorio se desarrolla como uno de los proyectos de la línea de aprovechamiento de fuentes de información alternativas (Twitter) de la Coordinación de Prospectiva y Analítica de Datos de la Dirección de Regulación, Planificación, Estandarización y Normalización (DIRPEN) del DANE.

## Contenido

1. [Tecnologías usadas](#tecnologías)
2. [Piloto DataSandbox](#piloto)
3. [Datasets](#datasets)
4. [Diccionarios](#diccionarios)
5. [Dashboard](#dashboard)

## Tecnologías usadas

Este ejercicio fue desarrollado utilizando el servicio de computación en la nube creado por Microsoft, [Microsoft Azure](https://azure.microsoft.com/es-es/). En adición, fue utlizada las plataformas [Databricks](https://databricks.com/) y [Google Drive](https://www.google.com/intl/es_co/drive/) así como los lenguajes de programación [Python](https://www.python.org/) y [RStudio](https://rstudio.com/).

De igual manera se usaron las siguientes librerías:

Python
- Pandas
- Plotly

RStudio
- jsonlite

## Piloto DataSandbox

Este piloto consiste en un ejercicio experimental para medición de percepción de confianza de los usuarios desde fuentes no tradicionales de información (Twitter en este caso). Esta sección presenta un proyecto piloto en el cual se desarrolló al final un Dashboard para visualización de diferentes estadísticos relacionados a los sentimientos de los usurios sobre el Departamento Administrativo Nacional de Estadística (DANE).

Este ejercicio consta de cinco etapas principales (como se muestra en la figura a continuación) las cuales son: ingesta de datos, modelado, almacenamiento, procesamiento y visualización.

<img src="/Figuras/architecture_diag.PNG" width="706" height="195">

* **Ingesta de datos**
 En esta estapa se realizó la adquisición de los datos de Twitter haciendo uso de su API sobre el servicio de Logic APS de Microsoft Azure.

* **Modelado**
 Haciendo uso del servicio Text Analytics de Microsoft Azure fueron asignados puntajes de percepción en una escala 0 a 1, siendo: positivo (>0.5), neutral (0.5) y negativo (< 0.5). En adición, una vez procesados los datos son generados los estadísticos correspondientes para la etapa de visualización.
 
* **Almacenamiento**
 Los datos obtenidos son almacenados dentro de Google Drive en un formato .xlsx contando con 25 características (ver diccionario ``Twitter Data Dictionary_Senti_Tweet_all_variables.xlsx``).

* **Procesamiento**
 El conjunto de datos es cargado a la plataforma de Databricks en la cual es garantizado el tipo de dato adecuado para cada variable tanto para la generación de estadísticos, como para la visualización de la información.

* **Visualización**
 La información es visualizada mediante gráficas interactivas de Plotly generadas en la plataforma de Databricks bajo lenguaje Python y organizadas en estructura de Databricks Dashboard para exportación.

## Datasets

Se encuentran dos bases de datos principales (ubicadas en [`Datos-Ejemplo/Para-Modelo`](Datos-Ejemplo/Para-Modelo)):

* ``dscrga_json.csv`` <br>
Esta base cuenta con 5968 registros de tweets en los periodos 2018-II, 2019 y 2020-I, sobre las que se establecieron tres criterios de búsqueda en los objetos tweets. Estos criterios se basan en las palabras “DANE, empleo o desempleo”. 
Los datos fueron descargados mediante el paquete de [jsonlite](https://cran.r-project.org/web/packages/jsonlite/jsonlite.pdf) de RStudio. Este se ejecuta mediante un código que simula una búsqueda de términos en la página de Twitter con los criterios establecidos.

* ``Senti_Tweet_all_variables.xlsx`` <br>
Esta base consta de 23975 registros entre las fechas 01 de octubre de 2020 y 17 de diciembre de 2020 utilizando la etiqueta de búsqueda “DANE_Colombia”.
Los objetos de esta base fueron descargados mediante el servicio de [Logic Apps](https://azure.microsoft.com/es-es/services/logic-apps/) de Microsoft Azure mediante la conexión a la API de twitter y marcados en tiempo real mediante el servicio de [Text Analytics](https://azure.microsoft.com/es-es/services/cognitive-services/text-analytics/) el cual es un componente de detección del sentimiento que marca la polaridad (positivo, negativo, neutro) de los textos (tweets).

A continuación se muestra los componentes desde Logic Apps para adquirir los atributos del twitter y su clasificación: 

<img src="/Figuras/logic_apps_diagram.PNG" width="450" height="450">

## Diccionarios

De igual forma, contiene dos diccionarios de datos (ubicados en [`Documentacion/DiccionarioDatos`](Documentacion/DiccionarioDatos)):

* ``Twitter Data Dictionary_Full.xlsx`` <br>
Este diccionario contiene todas las variables entregadas por el API de Twitter.

* ``Twitter Data Dictionary_Senti_Tweet_all_variables.xlsx`` <br>
Este diccionario contiene únicamente las variables presentes en la base de datos ``Senti_Tweet_all_variables.xlsx``

## Dashboard

En la ubicación [`App/Despligue`](App/Despligue) se encuentra tanto el Notebook ``p_to_dasboard_twitter.ipynb`` como el modelo de dashboard ``p_to_dasboard_twitter.html`` desarrollados para este ejercicio exploratorio con información referente al conjunto de datos ``Senti_Tweet_all_variables.xlsx``.
