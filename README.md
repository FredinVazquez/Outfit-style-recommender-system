<div align='center'>
  <h1>Fashion style Recommender system</h1>
</div>
<h4> Recommendation system based on clustering, feature extraction and textual similarity of Nike fashion products.</h4>

---

<div align='justify'>
El sistema se encuentra basado en la extracción de características de los productos dados por el dataset de Nike, como se pudo observar del proceso EDA, los productos son las prendas vendidas por Nike. Se estará teniendo que la extracción de características será sobre las imagenes de las prendas según la distribución de sus pixeles.
  <br></br>
Junto con la extracción de características de cada imagen será posible usar esta como parte de nuestra métrica para la comparación entre características de pares de imágenes, con esta métrica se estará creando los clusters de las imágenes. La cantidad de clusters estará basado en algunas de las ténicas para la elección de k más óptimizada. Entre los algpritmos de la creación de clusters se creará una validación cruzada de los elementos para ver qué algoritmo nos está generando un mejor score para la creación de clusters que se encuentren más separados entre ellos y así poder tener clusters que se encuentren bien diferenciados. En este punto se tiene la creación de clusters basado en la extracción de características para las imágenes, por ende se interpreta que los clusters de imágenes creados estará basado en la similitud de imágenes entre sí y no directaemente una categorización de éstilos de las prendas.

<h3>Identificación de cada cluster</h3>
  El siguiente problema a retomar es cómo se podrá identificar cada cluster, la idea detrás de esto es que la búsqueda de prendas se realicé sobre un solo cluster para hacer una búsqueda más refinada. Para esto se asignará a cada cluster una respectiva lista de palabras claves que funcionará como categoría. De manera que al momento que el ususario esté realizando sus elecciones, pueda seleccionar por medio de un dropbox las categorías de interés, lo cual hará que se seleccione un cluster en específico para después hacer la búsquedas de las prendas que pertenecen a esta categoría y así obtener un conjunto más acorde a lo solcitiuado por el usuario.

La idea de realizar esto será con base a las categorías definidas por cada prenda o la extracción de palabras claves de la descripciones de los productos, sea cual sea el caso, se estará realizando un mapreduce para obtener estas palabras que más se repite, después de un tratamiento de eliminación de stopwords, y así categorízar cada cluster.

      +--------------+ 
      |   MapReduce  | 
      |              |
      |--------------|
      |  Texto/      |
      |  Descripción |
      +--------------+
      
        |          |
        |          | Obtención de palabras que más se repiten
        v          v
        Categorizando 
        el cluster 2
        +-------------+   +-------------+   +-------------+
        |   cluster2  |   |   cluster3  |   |   cluster4  |    --- EL MISMO PROCESO SE APLICA PARA EL RESTO DE CLUSTERS.
        |             |   |             |   |             |
        +-------------+   +-------------+   +-------------+

<h3>Recomendación de prendas en el i-ésimo Cluster categórico</h3>
Se recoge la entrada del usuario para después vectorizarla a manera de tokens, llevando un preprocesamiento para la eliminación de stopwords. Una vez tokenizada la entrada del usuario se puede hacer una comparación de esta consulta con todos las descripciones de los productos, la estrategía a usar estará basado en búsqueda de vencinos más cercanos por LSH usando la implementación de pyspark. Por medio de la distancia jaccard, angular, o alguna otra, se podrá obtener la distancia jaccard de nuestra consulta con cada registro obtenido, podemos agilizar el proceso usando pyspark aunque esto posibleemente quede en discusión ya que no estaremos pasando un dataframe de consultas ya que solo tenemos una sola consulta, por ende podemos hacer la creación de buckets con una aproximación por join y posteriomente hacer una aproximación de los k vecinos más cercanos por medio de la entrada tokenizada del usuario. De manera que se obtendrá k vecinos de acuerdo a la descripción del usuario para después mostrarlo como resultado. 
  <br></br>
Así mismo, la idea será solicitar una descripción para cada prenda:
 <ul>
   <li>Playera</li>
   <li>Pantalon</li>
   <li>Zapatos</li>
   <li>Sudadera (opcional)</li>
 </ul>
    
          +-------------------------+
          |          Entrada        |
          |                         |
          +------------+------------+
                       |
                       v
          +-------------------------+
          |        Tokenizar        |
          |                         |
          +------------+------------+
                       | -------------------------------------------------------------------------------------
                       v                                            v                                        v
    +-------------------+-------------------+  +--------------------------------------+  +-------------------+------------------+
    | Medida de similitud para registro 1   |  | Medida de similitud para registro 2  |  | Medida de similitud para registro 3  |  |  ...  |
    |                                       |  |                                      |  |                                      |  |       |
    +-------------------+-------------------+  +--------------------------------------+  +-------------------+-------------------

Posteriormente se muestran los k vecinos asociados a esa prenda. De igual manera se puede experimentar tratar de hacer el mismo procedimiento pero con una sola entrada del usuario donde coloque la descripción del tipo de prendas a usar para con esta descripción se enceuntren todas las prendas que constituye un oufit.
  <br></br>
En caso contrario, tomar en cuenta que dentro de los vecinos más cercanos se espera obtener un filtrado de resultados para solo mostrar resultados de un tipo de prenda.
<h3>Búsqueda de prendas similares dada una iamgen, de una prenda, como entrada</h3>
Esta sección es similiar a la primera parte del proceso, debido aq ue solo se trata extraer las características de la imagen de entrada, después clasificar la imagen en alguno de los clusters encontrados anteriormente según al métrica de similitud a usar.

</div>
