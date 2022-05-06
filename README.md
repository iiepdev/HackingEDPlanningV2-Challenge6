# 🎨 HackingEDPlanningV2-Challenge6

<a href="http://www.youtube.com/watch?feature=player_embedded&v=z7BVog3RnuQ" target="_blank"><img src="https://img.evbuc.com/https%3A%2F%2Fcdn.evbuc.com%2Fimages%2F237802049%2F336870561013%2F1%2Foriginal.20220228-102209?w=800&auto=format%2Ccompress&q=75&sharp=10&rect=0%2C54%2C1200%2C600&s=92cc71cae0ff03ed75357a1f0aef9819" 
alt="Introductory video" width="720" height="360" border="10" /></a>

🇫🇷 [Version française](#version-française)

🇪🇸 [Versión en Español](#versi%C3%B3n-en-espa%C3%B1ol)

Note: By accessing this repository and the corresponding files, you agree to a non-disclosure agreement. To get a copy, please write to development@iiep.unesco.org

## Improving the visualisation of education indicators to make them more accessible and user-friendly

Through its web platform SITEAL, IIEP provides a set of educational indicators from Latin American countries disaggregated by socio-economic variables. This data is **highly relevant for officers of the Ministries of Education of the region, as well as for researchers and developers** interested in statistical trends. However, the output tables of the current interface are not visually attractive, which makes them hard to interpret and leads to under-utilization. 
This challenge proposes to create a minimum working example or a mock-up of proposed improvements for the data visualization, with the aim of fostering a greater and better use of the data. This entails making the output tables more visually appealing and user friendly and offering graphs that allow the visualization of long-term trends and the comparison between indicators. 


# Data

The data can be found in the `data` folder. There is one CSV file for each table.

- `datos.csv` is the main table that contains the indicator values for each cut-off variable;
- each `labels_*.csv` file contains the categories of each cut-off variable. Although the cut-off variables are the same, the categories differ by country;
- `pais_ano.csv` is a table indicating which years and "chapter" of data are available for each country;
- `fuentes.csv` lists the source of the data for each country.

You can find the data model with relations between each table below:
![SITEAL data model](data_model_siteal.png "SITEAL Data Model")

\
The table `datos` has the following fields:
- **id_datos**: ID of the record;
- **pais, region, ano, area, sexo, grupo_edad, clima_educativo,nivel_ingresos**: To which cutoffs the record corresponds. Except for `ano`, the values of the columns are IDs bound to each `labels_*` tables. Each record is the value of an indicator for a subset of the population corresponding to the intersection of all these cutoffs. Note that each indicator, depending on the chapter to which it corresponds, uses different cut-off variables. In other words, in some records some cutoff variables will be 0, which means that this indicator does not use it.
- **indicador**: The ID of the indicator;
- **valor_indicador**: The value of the indicator for that subset of the population corresponding to the intersection of all the cutoffs.
- **factor**: A weighting factor relative to the percentage of the country's population that corresponds to the subset of the population identified by this record.
- **muestra**: The sample size of the subset of the population that was used to construct the indicator value. If in the results table a cell corresponds to a very small sample (summing the samples of all the records that were used to generate that cell) a warning is displayed that the value may not be representative.


# Compute the indicators

To calculate the indicators, there is a little bit of work.

For each row of the table, all the records corresponding to that crossing are summed.

For example: if I select the filters `name_indicador="Tasa de asistencia escolar por edad"`, `ano=2000` and `pays=Argentina`, 
I will sum the remaining records regardless of region, area, educational level, income, etc..

Given all the records I will calculate :

![Compute indicators](https://latex.codecogs.com/svg.latex?%5CLarge%5Cfrac%7B%5Csum_%7Bi%7D%28valor%5C_indicador_i%20%5Ctimes%20factor_i%29%7D%20%7B%5Csum_%7Bi%7D%20factor_i%7D)

\
Here is an example of code with the above filters and grouping by the `region` and `sexo` written in Python and the Pandas library:
```py
#data df holds the name of each cut-off variable along with the indicators values
(data_df[
    (data_df.id_indicador == 1) 
    & (data_df.name_pais == "Argentina") 
    & (data_df.ano == 2000)
]
.groupby(["name_region","name_sexo"])
.apply(
    lambda x: (x.valor_indicador * x.factor).sum() / x.factor.sum()
))
```
It will output:
```
name_region  name_sexo    value
Cuyo         F            92.772666
             M            92.596198
GBA          F            95.550191
             M            93.866597
NEA          F            90.699658
             M            90.797952
NOA          F            92.361585
             M            92.078883
Pampeana     F            93.687275
             M            93.538756
Patagonia    F            94.748224
             M            94.416389
```

# Resources

To get you started quickly and help you with this challenge, we have prepared a non-exhaustive list of inspiring data visualizations and tools on this Notion page : **[Inspiring visualizations and Toolbox](https://fabiencazals.notion.site/Inspiring-visualizations-and-Toolbox-3797702146d443078ac6413b33203c00)**.

However, feel free to use the tools you prefer to succeed in this challenge!


# How to use this GitHub repository ? 

If you have never used github repository you can download the content of this repository by clicking on the button **Code** and then **download zip**. If you want you can start to use github by forking this project as a base for your project and share your work on Github. 

![image](https://user-images.githubusercontent.com/20289907/165938434-c12486a7-b9ae-43e8-81f2-0e15e279bfd3.png)

# Version française

# Versión en Español

Nota: Al acceder a este repositorio y a los archivos correspondientes, usted acepta un acuerdo de no divulgación. Para obtener una copia, escriba a development@iiep.unesco.org

🧐 A través de su plataforma web SITEAL, el IIPE ofrece un conjunto de indicadores educativos de los países latinoamericanos desagregados por variables socioeconómicas. Estos datos son muy relevantes para los funcionarios de los ministerios de educación de la región, así como para los investigadores y desarrolladores interesados en tendencias estadísticas. Sin embargo, visualizaciones que actualmente produce la interfaz no son atractivas, lo que dificulta su interpretación y conduce a una subutilización de la información.

🎯 Este reto propone crear un prototipo o una maqueta para mostrar cómo la visualización de datos podría ser más dinámica y atractiva, con el objetivo de fomentar un mayor uso de la información. Esto implica mejorar la usabilidad de la plataforma y ofrecer gráficos que permitan la visualización de tendencias a largo plazo y la comparación entre indicadores.

⛑ Se fomenta la participación de diseñadores web, analistas de datos y especialistas en visualización de datos.

# Datos
Los datos se encuentran en la carpeta `data`. Hay un archivo CSV para cada tabla.

- `datos.csv` es la tabla principal que contiene los valores de los indicadores para cada variable de corte;
- cada archivo `labels_*.csv` contiene las categorías de cada variable de corte. Aunque las variables de corte son las mismas, las categorías difieren según el país;
- `pais_ano.csv` es una tabla que indica qué años y "capítulo" de datos están disponibles para cada país;
- `fuentes.csv` enumera la fuente de los datos de cada país.

A continuación, puede encontrar el modelo de datos con las relaciones entre cada tabla: 

![SITEAL data model](data_model_siteal.png "SITEAL Data Model")

La tabla datos tiene los siguientes campos:
- **id_datos**: ID del registro;
- **país, región, año, zona, sexo, grupo_edad, clima_educativo,nivel_ingresos**: A qué cortes corresponde el registro. Excepto en el caso de ano, los valores de las columnas son identificadores ligados a cada una de las tablas labels_*. Cada registro es el valor de un indicador para un subconjunto de la población correspondiente a la intersección de todos estos cortes. Note que cada indicador, según el capítulo al que corresponde, utiliza diferentes variables de corte. Es decir, en algunos registros algunas variables de corte serán 0, lo que significa que ese indicador no lo utiliza.
- **indicador**: El ID del indicador;
- **valor_indicador**: El valor del indicador para aquel subconjunto de la población correspondiente a la intersección de todos los cortes.
- **factor**: Un factor de ponderación relativo al porcentaje de la población del país que corresponde al subconjunto de la población identificado por este registro.
- **muestra**: El tamaño de la muestra del subconjunto de la población que se utilizó para construir el valor del indicador. Si en la tabla de resultados una celda corresponde a una muestra muy pequeña (sumando las muestras de todos los registros que se utilizaron para generar esa celda) se muestra una advertencia de que el valor puede no ser representativo.

# Calcular los indicadores

Para calcular los indicadores, hay un poco de trabajo.
Para cada fila de la tabla, se suman todos los registros correspondientes a ese cruce.
Por ejemplo: si selecciono los filtros `name_indicador="Tasa de asistencia escolar por edad"`, `ano=2000` y `pays=Argentina`, sumaré el resto de los registros independientemente de la región, zona, nivel educativo, ingresos, etc.

Teniendo en cuenta todos los registros voy a calcular:

A continuación se muestra un ejemplo de código con los filtros anteriores y la agrupación por la region y sexo escrito en Python y la biblioteca Pandas:

```
#data df contiene el nombre de cada variable de corte junto con los valores de los indicadores
(data_df[
    (data_df. id_indicador == 1) 
    & (data_df. name_pais == "Argentina") 
    & (data_df. ano == 2000)
]
. groupby(["nombre_región", "nombre_sexo"])
. aplicar(
    lambda x: (x. valor_indicador * x. factor). sum() / x. factor. sum()
))
```

Esto dará como resultado:

```
name_region  name_sexo    value
Cuyo         F            92.772666
             M            92.596198
GBA          F            95.550191
             M            93.866597
NEA          F            90.699658
             M            90.797952
NOA          F            92.361585
             M            92.078883
Pampeana     F            93.687275
             M            93.538756
Patagonia    F            94.748224
             M            94.416389
```

# Recursos
Para ayudarle a empezar con este reto, hemos preparado una lista no exhaustiva de visualizaciones de datos y herramientas inspiradoras en esta página de Notion : [Visualizaciones inspiradoras y Caja de herramientas](https://fabiencazals.notion.site/Inspiring-visualizations-and-Toolbox-3797702146d443078ac6413b33203c00).

Sin embargo, ¡siéntase libre de utilizar las herramientas que prefiera para tener éxito en este desafío!

# ¿Cómo utilizar este repositorio de GitHub?

Si nunca ha utilizado el repositorio de GitHub, puede descargar el contenido de este repositorio haciendo clic en el botón Código y luego descargar el zip. Si así lo desea, puede empezar a usar GitHub bifurcando este proyecto como base para su proyecto y compartir su trabajo en GitHub.

![image](https://user-images.githubusercontent.com/20289907/165938434-c12486a7-b9ae-43e8-81f2-0e15e279bfd3.png)
