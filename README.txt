Se trata de un dataset que contiene cursos de formación, con lo que la mayoría de las columnas contienen strings. EL dataset que he empleado se trata de un fragmento del original, mucho más grande (300.000 filas).

EL objetivo es, por un lado, realizar cluster a partir de las descripciones de los cursos (almacenados en la columna D_CONTENIDOS). 

Con esta doble finalidad iniciaré un proceso que constará de cuatro fases:

1. Exploración y limpiza de datos (importación del documento, eliminación de filas duplicadas, eliminación de filas que tienen menos de diez palabras en el campo D_CONTENIDOS, eliminación de columnas, eliminación de filas en inglés).

2. Data Mining (eliminación de caracteres no alfabéticos, tokenización y stem -que se realiza con SnowballStemmer que cuenta con diccionario en español. No se realiza lematización ya que la galería de python no resulta efectiva, falta probar SpaCy o es-lemmatize. Eliminación de stopwords en español. Limitación del máximo de palabras a analizar -100 palabras más relevantes-. ).

3. Clusterización (para vectorizar y poder transformar strings a números se usa TfidfVectorizer y se genera un nuevo df. Optimización del número de clusters mediante silhouette_score y elbow curve de la galería KMeans, que arrojan una cifra de 47. Se realizan los clusters por medio de KMeans, aunque se prueban también DBscan y Agglomerative con peores resultados -falta LDA-). LOs clusters realizados no son plenamente satisfactorios. Necesitan más precisión. En este sentido, la lematización y la exclusión de ciertas palabras que parecen ensuciar el análisis pueden ayudar.

4. Predicción (Para poder elaborar el modelo predictivo se realiza un PCA sobre el nuevo df -excluyendo la columna clusters- de tal forma que se pueda reducir dimensionalidad. Una vez realizado esto se aplican distindos modelos -LR, SVM, GB- pero el que mejores resultados tiene es Random Forest. RF, a su vez, es optimizado mediante GridSearch- obteniendo un score en la predicción de un máximo del 76% -probablemente suba con un mejor tratado de datos  previo).