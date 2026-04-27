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
