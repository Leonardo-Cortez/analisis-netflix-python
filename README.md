# analisis-netflix-python

## Spanish version

# Poyecto : Análisis y Modelado predictivo del contenido de Netflix.

### Descripción

Este proyecto integra el análisis exploratorio de datos (EDA) y aprendizaje automático para desentrañar la estrategia de contenido global de Netflix. El objetivo principal fue identificar patrones de crecimiento histórico (como el auge post-2016) y geográfico, además de construir modelos de clasificación (KNN, árbol de decisión y regresión logística) para predecir si el contenido es Serie (TV Show) o película (Movie).

### Herramientas utilizadas

* Python
  * Procesamiento: Pandas, Numpy.
  * Visualización: Matplotlib y Seaborn.
  * Machine Learning: Scikit-Learn (para el pipeline).

### Desafíos enfrentados

* **Manejo de valores nulos**: Una de las variables más retadoras fue "director", ya que contenía poco más del 30% de valores nulos del conjunto de datos original. Si se eliminaban dichos valores, se perdería bastante información y muy posiblemente se crearían sesgos al momento de entrenar y evaluar los modelos; por lo que la solución fue "imputar" dicha variable con una etiqueta "Unknown".
<br> De manera similar, para los campos "country", "cast", se "imputaron" mediante etiquetas "Unknown" y "Not Available" respectivamente; ya que juntos representaban un 15% de datos nulos.
<br> Para el campo de "rating" se utilizó la moda para los 7 registros encontrados y finalmente en el campo "date_added", se eliminaron 98 registros para evitar trabajar con fechas "falsas".

* **Ingeniería de características**: Se crearon nuevas variables a partir de algunos campos:
  * Campo date_added: Con el fin de facilitar el análisis temporal del contenido agregado a la plataforma, se generaron nuevas variables numéricas ("year_added" y "month_added").
  * Campo duration: Para facilitar el análisis comparativo entre películas y series en las fases posteriores, se dosificó en dos nuevas variables ("duration_num" y "duration_unit"), para almacenar la parte numérica de la duración y la unidad de medida ("min", "Season", "Seasons") respectivamente.
  * Campos multivalor como director, cast, country y listed_in: Se generó director_count como tipo numérico para poder contar el número de directores que participaron en cada obra.
<br> De manera similar se transformaron el campo cast_count y country_count, ambos de tipo numérico, para saber cuántos actores formaron parte del elenco y cuántos países tuvieron relación con la producción.
<br> Además, se creó un campo adicional con el nombre main_genre, donde sólo se toma el primer género encontrado en la lista en caso de haber más de uno.
  * Campos adicionales: content_age permite calcular "la edad de la película" realizando un operación aritmética, el año actual menos el año de lanzamiento de la película/serie.
<br> Finalmente se generaron campos con valor de tipo booleano, is_multicountry mostrará un valor de 1 si el registro contiene más de un país, 0 si es lo contrario. is_long_content mostrará 1 en caso de que la duración de las películas sea mayor a 120 minutos, 0 en caso contrario.

### Métricas de evaluación

Se evaluaron los modelos de K-Nearest Neighbors, Regresión Logística y Árbol de decisión mediante accuracy, precision, recall, F1-Score y la Matriz de confusión:

| Modelo | Accuracy| Precision | Recall | F1-Score |
|---------|-----------|---------|-----------|---------|
| KNN | 0.985696 | 0.976190 | 0.976190 | 0.976190 |
| Regresión Logística | 0.996749| 0.991398 | 0.997835 | 0.994606 |
| Árbol de decisión | 0.997399 | 0.995671 | 0.995671 | 0.995671 |

La matriz de confusión de cada modelo puede ser consultada en "netflix_project/assets". A continuación se presenta la interpretación de los resultados, donde:
* TN = Verdaderos negativos
* FP = Falsos positivos
* FN = Falsos negativos
* TP = Verdaderos positivos

<table>
  <tr>
    <th colspan="4">Tabla 1. Resultados matriz de confusión para KNN </th>
  </tr>
  <tr>
    <td>TN: 1065</td>
    <td>FP: 11</td>
    <td>FN: 11</td>
    <td>TP: 451</td>
  </tr>
</table>

La Tabla 1 indica que el modelo KNN es relativamente el menos preciso en cuanto a desempeño, ya que cometió 22 errores totales. Porque clasificó 11 series de televisión como películas y 11 películas como series de televisión.

<table>
  <tr>
    <th colspan="4">Tabla 2. Resultados matriz de confusión para Árbol de decisión </th>
  </tr>
  <tr>
    <td>TN: 1074</td>
    <td>FP: 2</td>
    <td>FN: 2</td>
    <td>TP: 460</td>
  </tr>
</table>

La Tabla 2 indica que el Árbol de decisión tiene mucho mejor desempeño que KNN, ya que se sólo cometió 4 errores. Porque clasificó 2 series de televisión como películas y 2 películas como series de televisión. Por lo que es el modelo más equilibrado y confiable, ideal para entender qué reglas de negocio definen cada tipo de contenido.

<table>
  <tr>
    <th colspan="4">Tabla 3. Resultados matriz de confusión para Regresión Logística </th>
  </tr>
  <tr>
    <td>TN: 1072</td>
    <td>FP: 4</td>
    <td>FN: 1</td>
    <td>TP: 461</td>
  </tr>
</table>

Y finalmente la Tabla 3, indica que la Regresión Logística tiene alta precisión en en clasificar series, ya que clasificó 1 serie como película y 4 películas como series. Si se compara este modelo con los 2 anteriores, estaría en un punto intermedio al tener 5 errores totales. Al tener un falso negativo se puede decir que todas las series de televisión que había en los datos, el modelo no pudo acertar en una.
