---
output: hugodown::md_document
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Extraer y ordenar datos FAOSTAT con Tidyverse"
subtitle: "FAOSTAT data Scraping"
summary: "Un post replicable para extraer y ordenar datos FAOSTAT con el paquete Tidyverse. Se presenta un análisis comparativo de paises latinoamericanos en función al rendimiento de los cultivos mas consumidas por la humanidad"
authors: [Franklin Santos]
tags: [R, Crops]
categories: [Web Scraping]
date: 2020-11-18
lastmod: 2020-11-18
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: "featured.png"
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
rmd_hash: a795991dca5a7d26

---

**Introducción**
----------------

`Extraer`, `ordenar` y `graficar` datos es una tarea que demanda tiempo y cierta habilidad informática. Generalmente, muchos investigadores buscamos datos históricos para observar `tendencias` de *rendimiento*, *producción* y *superficie cultivada*, esto con el propósito de soportar diferentes `investigaciones` del área. Para realizar estas tareas existen diferentes bases de datos de libre acceso, entre ellos está [FAOSTAT](http://www.fao.org/faostat/es/#home). Esta organización proporciona *acceso libre* a datos sobre **alimentación y agricultura** de más de 245 países y 35 regiones, desde 1961 hasta el año más reciente disponible.

Los datos de FAOSTAT habitualmente son descargados en planillas de `Microsoft Excel` y en ésta se va limpiando de forma manual hasta que el resumen de datos sea presentable. Esta actividad requiere tiempo; sin embargo, existen herramientas de programación que pueden `automatizar` este proceso y reducir tiempo en ordenar datos. Una de ellas es [**R**](https://www.r-project.org), la cual es un *lenguaje de programación* y un ambiente de *software libre* para la **ciencia de datos**. Por tanto, nuestro objetivo es `extraer` y `ordenar` datos de la página web de `FAOSTAT` con el uso del paquete `Tidyverse` en el ambiente `R`.

**Extracción**
--------------

Para acceder a FAOSTAT, diríjase al sitio web oficial a traves del enlace <a href="http://www.fao.org/faostat/en/#home" class="uri">http://www.fao.org/faostat/en/#home</a>. En la página principal tiene la opción de cambiar el idioma del ingles al español y haga clic en `Acceder a los datos`. Una vez que acceda a esta ventana, haga clic en `cultivos` en la sección de `Producción`, en la cual observará la vista de selección y `descargar datos`. En esta ventana seleccionamos la base de datos a descargar. Para esto siga los siguientes pasos:

![](fao.png)

-   Seleccione **países** o **regiones** de interes, en nuestro caso hacemos clic en `regiones` y elegimos los países de `América del Sur`.
-   En la subventana de `elementos` puede seleccionar `Área cosechada` (superficie cultivada), `Rendimiento` y `Producción`. Para nuestro ejemplo elegimos *rendimiento*.
-   El siguiente paso es seleccionar los cultivos o `productos` de interes. En nuestro caso elegimos cuatro cultivos (arroz, maíz, papa y trigo).
-   En la última subventana seleccione los `Años` de interes. Para nuestro ejemplo elegimos todos los años, esto con el propósito de hacer comparación de la tendencia de rendimiento en los países de latinoamérica.
-   El último paso es `descargar` los datos seleccionados. Para esto, la salida de datos debe estar seleccionado en formato `Tabla`, tipo de archivo `CSV` y hacer clic en `Descargar Datos` para guardar en un archivo de proyecto.

**Ordenación**
--------------

A partir de esta etapa se usará la consola de **R** a traves del entorno [RStudio](https://rstudio.com). Para ordenar los datos es recomendable usar el paquete [tidyverse](https://www.tidyverse.org), la cual es una colección obstinada de paquetes R diseñados para la ciencia de datos. Para instalar use este código [`install.packages("tidyverse")`](https://rdrr.io/r/utils/install.packages.html).

Para proceder con la ordenación de datos, analizar y/o generar otro tipo de actividades con R, recomiendo crear un proyecto a traves de `RStudio`. Esto facilita el flujo de trabajo dentro de R. Posterior a ello, llame al paquete `tidyverse`:

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='kr'><a href='https://rdrr.io/r/base/library.html'>library</a></span><span class='o'>(</span><span class='nv'><a href='http://tidyverse.tidyverse.org'>tidyverse</a></span><span class='o'>)</span>
</code></pre>

</div>

Para iniciar con el trabajo es necesario que los datos descargados de FAOSTAT se encuentre dentro de los archivos del proyecto.

Para llamar los datos al entorno de R use la siguiente función:

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nv'>key_crops</span> <span class='o'>&lt;-</span> <span class='nf'>read_csv</span><span class='o'>(</span><span class='s'>"FAOSTAT_data_11-18-2020.csv"</span><span class='o'>)</span>
</code></pre>

</div>

Para verificar el marco de datos en el ambiente R, ejecute la variable `key_crops`.

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nv'>key_crops</span>

<span class='c'>#&gt; <span style='color: #555555;'># A tibble: 2,657 x 14</span></span>
<span class='c'>#&gt;    `Domain Code` Domain `Area Code` Area  `Element Code` Element `Item Code`</span>
<span class='c'>#&gt;    <span style='color: #555555;font-style: italic;'>&lt;chr&gt;</span><span>         </span><span style='color: #555555;font-style: italic;'>&lt;chr&gt;</span><span>        </span><span style='color: #555555;font-style: italic;'>&lt;dbl&gt;</span><span> </span><span style='color: #555555;font-style: italic;'>&lt;chr&gt;</span><span>          </span><span style='color: #555555;font-style: italic;'>&lt;dbl&gt;</span><span> </span><span style='color: #555555;font-style: italic;'>&lt;chr&gt;</span><span>         </span><span style='color: #555555;font-style: italic;'>&lt;dbl&gt;</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 1</span><span> QC            Crops            9 Arge…           </span><span style='text-decoration: underline;'>5</span><span>419 Yield            56</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 2</span><span> QC            Crops            9 Arge…           </span><span style='text-decoration: underline;'>5</span><span>419 Yield            56</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 3</span><span> QC            Crops            9 Arge…           </span><span style='text-decoration: underline;'>5</span><span>419 Yield            56</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 4</span><span> QC            Crops            9 Arge…           </span><span style='text-decoration: underline;'>5</span><span>419 Yield            56</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 5</span><span> QC            Crops            9 Arge…           </span><span style='text-decoration: underline;'>5</span><span>419 Yield            56</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 6</span><span> QC            Crops            9 Arge…           </span><span style='text-decoration: underline;'>5</span><span>419 Yield            56</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 7</span><span> QC            Crops            9 Arge…           </span><span style='text-decoration: underline;'>5</span><span>419 Yield            56</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 8</span><span> QC            Crops            9 Arge…           </span><span style='text-decoration: underline;'>5</span><span>419 Yield            56</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 9</span><span> QC            Crops            9 Arge…           </span><span style='text-decoration: underline;'>5</span><span>419 Yield            56</span></span>
<span class='c'>#&gt; <span style='color: #555555;'>10</span><span> QC            Crops            9 Arge…           </span><span style='text-decoration: underline;'>5</span><span>419 Yield            56</span></span>
<span class='c'>#&gt; <span style='color: #555555;'># … with 2,647 more rows, and 7 more variables: Item </span><span style='color: #555555;font-style: italic;'>&lt;chr&gt;</span><span style='color: #555555;'>, `Year Code` </span><span style='color: #555555;font-style: italic;'>&lt;dbl&gt;</span><span style='color: #555555;'>,</span></span>
<span class='c'>#&gt; <span style='color: #555555;'>#   Year </span><span style='color: #555555;font-style: italic;'>&lt;dbl&gt;</span><span style='color: #555555;'>, Unit </span><span style='color: #555555;font-style: italic;'>&lt;chr&gt;</span><span style='color: #555555;'>, Value </span><span style='color: #555555;font-style: italic;'>&lt;dbl&gt;</span><span style='color: #555555;'>, Flag </span><span style='color: #555555;font-style: italic;'>&lt;chr&gt;</span><span style='color: #555555;'>, `Flag Description` </span><span style='color: #555555;font-style: italic;'>&lt;chr&gt;</span></span>
</code></pre>

</div>

En esta salida puede observar `2657` filas u observaciones y `14` columnas o variables. Asimismo, puede verificar los `formatos` de cada variable. Esta base de datos no facilita el uso apropiado para realizar un resumen descriptivo o generar gráficas para observar tendencias.

### Ordenando base de datos

Para este proceso se identificó que la columna de `Area` contiene países de América del Sur; sin embargo, en estas obsevarciones Bolivia tiene texto adicional "`Bolivia (Plurinational State of)`". Para eliminar el texto adicional se usa la función `separate(Area, c("country"), sep = " ")`, el mismo proceso se aplica para `Rice, paddy` en la columa de cultivos `Item`. A partir de la columna `Value` se creó otra columna `mutate(yield = Value / 10000)`, en la cual se hizo la conversión de hectogramos (hg ha<sup>-1</sup>) a toneladas (t ha<sup>-1</sup>). El siguiente paso fue seleccionar cuatro variables de la base de datos `select(country, crop, Year, yield)` y al finalizar este proceso se filtró los países más próximos a Bolivia.

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nv'>long_crops</span> <span class='o'>&lt;-</span> <span class='nv'>key_crops</span> <span class='o'>%&gt;%</span> 
  <span class='nf'>separate</span><span class='o'>(</span><span class='nv'>Area</span>, <span class='nf'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='o'>(</span><span class='s'>"country"</span><span class='o'>)</span>, sep <span class='o'>=</span> <span class='s'>" "</span><span class='o'>)</span> <span class='o'>%&gt;%</span>
  <span class='nf'>separate</span><span class='o'>(</span><span class='nv'>Item</span>, <span class='nf'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='o'>(</span><span class='s'>"crop"</span><span class='o'>)</span>, sep <span class='o'>=</span> <span class='s'>","</span><span class='o'>)</span> <span class='o'>%&gt;%</span>
  <span class='nf'>mutate</span><span class='o'>(</span>yield <span class='o'>=</span> <span class='nv'>Value</span> <span class='o'>/</span> <span class='m'>10000</span><span class='o'>)</span> <span class='o'>%&gt;%</span>
  <span class='nf'>select</span><span class='o'>(</span><span class='nv'>country</span>, <span class='nv'>crop</span>, <span class='nv'>Year</span>, <span class='nv'>yield</span><span class='o'>)</span> <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://rdrr.io/r/stats/filter.html'>filter</a></span><span class='o'>(</span><span class='nv'>country</span> <span class='o'>%in%</span> <span class='nf'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='o'>(</span><span class='s'>"Argentina"</span>, 
                        <span class='s'>"Bolivia"</span>, 
                        <span class='s'>"Brazil"</span>,
                        <span class='s'>"Chile"</span>,
                        <span class='s'>"Paraguay"</span>,
                        <span class='s'>"Peru"</span>,
                        <span class='s'>"Uruguay"</span><span class='o'>)</span>
  <span class='o'>)</span>
</code></pre>

</div>

Podemos observar los datos ordenados ejecutando `long_crops`

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nv'>long_crops</span>

<span class='c'>#&gt; <span style='color: #555555;'># A tibble: 1,624 x 4</span></span>
<span class='c'>#&gt;    country   crop   Year yield</span>
<span class='c'>#&gt;    <span style='color: #555555;font-style: italic;'>&lt;chr&gt;</span><span>     </span><span style='color: #555555;font-style: italic;'>&lt;chr&gt;</span><span> </span><span style='color: #555555;font-style: italic;'>&lt;dbl&gt;</span><span> </span><span style='color: #555555;font-style: italic;'>&lt;dbl&gt;</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 1</span><span> Argentina Maize  </span><span style='text-decoration: underline;'>1</span><span>961  1.77</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 2</span><span> Argentina Maize  </span><span style='text-decoration: underline;'>1</span><span>962  1.89</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 3</span><span> Argentina Maize  </span><span style='text-decoration: underline;'>1</span><span>963  1.65</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 4</span><span> Argentina Maize  </span><span style='text-decoration: underline;'>1</span><span>964  1.80</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 5</span><span> Argentina Maize  </span><span style='text-decoration: underline;'>1</span><span>965  1.68</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 6</span><span> Argentina Maize  </span><span style='text-decoration: underline;'>1</span><span>966  2.15</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 7</span><span> Argentina Maize  </span><span style='text-decoration: underline;'>1</span><span>967  2.47</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 8</span><span> Argentina Maize  </span><span style='text-decoration: underline;'>1</span><span>968  1.94</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 9</span><span> Argentina Maize  </span><span style='text-decoration: underline;'>1</span><span>969  1.93</span></span>
<span class='c'>#&gt; <span style='color: #555555;'>10</span><span> Argentina Maize  </span><span style='text-decoration: underline;'>1</span><span>970  2.33</span></span>
<span class='c'>#&gt; <span style='color: #555555;'># … with 1,614 more rows</span></span>
</code></pre>

</div>

Estos datos tienen 1624 filas y 4 columnas, las cuales estan ordenadas y listas para generar gráficas interactivas o estáticas.

**Graficando los datos**
------------------------

Generar gráficas es muy importante, ya que es más cómodo interpretar, analizar tendencias o identificar asociaciones. Debido a ello, se realizó una figura multipanel.

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nv'>long_crops</span> <span class='o'>%&gt;%</span>
  <span class='nf'>ggplot</span><span class='o'>(</span><span class='nf'>aes</span><span class='o'>(</span><span class='nv'>Year</span>, <span class='nv'>yield</span>, color <span class='o'>=</span> <span class='nv'>country</span><span class='o'>)</span><span class='o'>)</span> <span class='o'>+</span>
  <span class='nf'>geom_line</span><span class='o'>(</span><span class='o'>)</span> <span class='o'>+</span>
  <span class='nf'>facet_wrap</span><span class='o'>(</span><span class='o'>~</span><span class='nv'>crop</span>, ncol <span class='o'>=</span> <span class='m'>2</span><span class='o'>)</span> <span class='o'>+</span>
  <span class='nf'>labs</span><span class='o'>(</span>x <span class='o'>=</span> <span class='s'>"Year"</span>, y <span class='o'>=</span> <span class='s'>"Yield (t ha)"</span><span class='o'>)</span>

</code></pre>
<img src="figs/unnamed-chunk-6-1.png" width="700px" style="display: block; margin: auto;" />

</div>

En la figura, se observa tendencias de rendimiento a traves del tiempo que corresponden para los cuatro cultivos. Por ejemplo, el rendimiento de papa en Argentina tiende a incrmentar 10 t ha<sup>-1</sup> cada 20 años; sin embargo, en Bolivia las tendencias de rendimiento son constantes o sea no hay un incremento pronunciado a compararación de los países vecinos.

También se puede realizar una gráfica para el último año. Para ello, se filtró el año 2018 de la base de datos `long_crops` con la función [`filter(Year == 2018)`](https://rdrr.io/r/stats/filter.html). Con estos datos, se generó una gráfica de barras.

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nv'>yearfs</span> <span class='o'>&lt;-</span> <span class='nv'>long_crops</span> <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://rdrr.io/r/stats/filter.html'>filter</a></span><span class='o'>(</span><span class='nv'>Year</span> <span class='o'>==</span> <span class='m'>2018</span><span class='o'>)</span>

<span class='c'># seleccion de tema para la gráfica</span>
<span class='nf'>theme_set</span><span class='o'>(</span>
  <span class='nf'>theme_classic</span><span class='o'>(</span><span class='o'>)</span> <span class='o'>+</span>
    <span class='nf'>theme</span><span class='o'>(</span>legend.position <span class='o'>=</span> <span class='s'>"top"</span><span class='o'>)</span>
<span class='o'>)</span>

<span class='c'>#Generando gráfica de barras</span>
<span class='nf'>ggplot</span><span class='o'>(</span><span class='nv'>yearfs</span>, <span class='nf'>aes</span><span class='o'>(</span>x<span class='o'>=</span><span class='nv'>country</span>, y<span class='o'>=</span><span class='nv'>yield</span>, fill<span class='o'>=</span><span class='nv'>crop</span><span class='o'>)</span><span class='o'>)</span> <span class='o'>+</span>
  <span class='nf'>geom_bar</span><span class='o'>(</span>stat<span class='o'>=</span><span class='s'>"identity"</span>, position <span class='o'>=</span> <span class='nf'>position_dodge</span><span class='o'>(</span><span class='o'>)</span><span class='o'>)</span> <span class='o'>+</span>
  <span class='nf'>labs</span><span class='o'>(</span>x <span class='o'>=</span> <span class='s'>"Países"</span>, y <span class='o'>=</span> <span class='s'>"Yield (t ha)"</span><span class='o'>)</span>

</code></pre>
<img src="figs/unnamed-chunk-7-1.png" width="700px" style="display: block; margin: auto;" />

</div>

En esta figura se puede observar rendimientos de cuatro cultivos para el año 2018. En la cual, Argentina, Chile y Brasil tuvieron mayores rendimientos en el cultivo de papa; sin embargo, Bolivia se ubica en el último puesto en cuanto a rendimientos en los cuatro cultivos.

**Conclusión**
--------------

Los usuarios de R pueden generar scripts reproducibles a base de este post. Extraer, ordenar y graficar datos con R, facilita obtener datos limpios y es más eficiente con los tiempos de trabajo.

