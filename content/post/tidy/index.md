---
output: hugodown::md_document
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Ordenar datos con el paquete Tidyverse"
subtitle: ""
summary: "Es un ejemplo de caso extraido del libro R4DS"
authors: [Franklin Santos]
tags: [RStudio, Tidyverse, R4DS]
categories: [R]
date: 2020-10-31
lastmod: 2020-10-31
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
rmd_hash: ae94283fb2f9b34d

---

Introducción
------------

La ordenación de datos es una de las tareas mas importantes despues de concluir la investigación. En las ciencias agrícolas, generalmente la investigación concluye con la evaluación de la cosecha del cultivo. Generalmente nuestros datos pueden estar organizados en un libro de campo; sin embargo, en otras áreas no es así.

En este blog replicaré un ejemplo de ordenación de datos con el paquete `Tidyverse` del libro R4DS. El dataset [`datos::oms`](https://rdrr.io/pkg/datos/man/oms.html) contiene datos de tuberculosis (TB) detallados por año, país, edad, sexo y método de diagnóstico. Los datos provienen del Informe de Tuberculosis de la Organización Mundial de la Salud 2014, disponible en <a href="http://www.who.int/tb/country/data/download/en/" class="uri">http://www.who.int/tb/country/data/download/en/</a>.

Procedimiento de ordenación
---------------------------

### Cargar el paquete `tidyverse`

El primer paso es instalar el paquete `tidyverse` del CRAN de R. Posterior a esto es cargar el paquete en nuestra consola de R.

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='kr'><a href='https://rdrr.io/r/base/library.html'>library</a></span><span class='o'>(</span><span class='nv'><a href='http://tidyverse.tidyverse.org'>tidyverse</a></span><span class='o'>)</span>

<span class='c'>#&gt; ── <span style='font-weight: bold;'>Attaching packages</span><span> ─────────────────────────────────────── tidyverse 1.3.0 ──</span></span>

<span class='c'>#&gt; <span style='color: #00BB00;'>✔</span><span> </span><span style='color: #0000BB;'>ggplot2</span><span> 3.3.2     </span><span style='color: #00BB00;'>✔</span><span> </span><span style='color: #0000BB;'>purrr  </span><span> 0.3.4</span></span>
<span class='c'>#&gt; <span style='color: #00BB00;'>✔</span><span> </span><span style='color: #0000BB;'>tibble </span><span> 3.0.4     </span><span style='color: #00BB00;'>✔</span><span> </span><span style='color: #0000BB;'>dplyr  </span><span> 1.0.2</span></span>
<span class='c'>#&gt; <span style='color: #00BB00;'>✔</span><span> </span><span style='color: #0000BB;'>tidyr  </span><span> 1.1.2     </span><span style='color: #00BB00;'>✔</span><span> </span><span style='color: #0000BB;'>stringr</span><span> 1.4.0</span></span>
<span class='c'>#&gt; <span style='color: #00BB00;'>✔</span><span> </span><span style='color: #0000BB;'>readr  </span><span> 1.3.1     </span><span style='color: #00BB00;'>✔</span><span> </span><span style='color: #0000BB;'>forcats</span><span> 0.5.0</span></span>

<span class='c'>#&gt; ── <span style='font-weight: bold;'>Conflicts</span><span> ────────────────────────────────────────── tidyverse_conflicts() ──</span></span>
<span class='c'>#&gt; <span style='color: #BB0000;'>✖</span><span> </span><span style='color: #0000BB;'>dplyr</span><span>::</span><span style='color: #00BB00;'>filter()</span><span> masks </span><span style='color: #0000BB;'>stats</span><span>::filter()</span></span>
<span class='c'>#&gt; <span style='color: #BB0000;'>✖</span><span> </span><span style='color: #0000BB;'>dplyr</span><span>::</span><span style='color: #00BB00;'>lag()</span><span>    masks </span><span style='color: #0000BB;'>stats</span><span>::lag()</span></span>

<span class='c'>#En el paquete datos se encuentra la base de datos para este ejemplo</span>
<span class='kr'><a href='https://rdrr.io/r/base/library.html'>library</a></span><span class='o'>(</span><span class='nv'><a href='https://github.com/cienciadedatos/datos'>datos</a></span><span class='o'>)</span>
</code></pre>

</div>

A continuación observación el estado de los datos de `oms`.

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nv'>oms</span>

<span class='c'>#&gt; <span style='color: #555555;'># A tibble: 7,240 x 60</span></span>
<span class='c'>#&gt;    pais  iso2  iso3   anio nuevos_fpp_h014 nuevos_fpp_h1524 nuevos_fpp_h2534</span>
<span class='c'>#&gt;    <span style='color: #555555;font-style: italic;'>&lt;chr&gt;</span><span> </span><span style='color: #555555;font-style: italic;'>&lt;chr&gt;</span><span> </span><span style='color: #555555;font-style: italic;'>&lt;chr&gt;</span><span> </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span>           </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span>            </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span>            </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 1</span><span> Afga… AF    AFG    </span><span style='text-decoration: underline;'>1</span><span>980              </span><span style='color: #BB0000;'>NA</span><span>               </span><span style='color: #BB0000;'>NA</span><span>               </span><span style='color: #BB0000;'>NA</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 2</span><span> Afga… AF    AFG    </span><span style='text-decoration: underline;'>1</span><span>981              </span><span style='color: #BB0000;'>NA</span><span>               </span><span style='color: #BB0000;'>NA</span><span>               </span><span style='color: #BB0000;'>NA</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 3</span><span> Afga… AF    AFG    </span><span style='text-decoration: underline;'>1</span><span>982              </span><span style='color: #BB0000;'>NA</span><span>               </span><span style='color: #BB0000;'>NA</span><span>               </span><span style='color: #BB0000;'>NA</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 4</span><span> Afga… AF    AFG    </span><span style='text-decoration: underline;'>1</span><span>983              </span><span style='color: #BB0000;'>NA</span><span>               </span><span style='color: #BB0000;'>NA</span><span>               </span><span style='color: #BB0000;'>NA</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 5</span><span> Afga… AF    AFG    </span><span style='text-decoration: underline;'>1</span><span>984              </span><span style='color: #BB0000;'>NA</span><span>               </span><span style='color: #BB0000;'>NA</span><span>               </span><span style='color: #BB0000;'>NA</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 6</span><span> Afga… AF    AFG    </span><span style='text-decoration: underline;'>1</span><span>985              </span><span style='color: #BB0000;'>NA</span><span>               </span><span style='color: #BB0000;'>NA</span><span>               </span><span style='color: #BB0000;'>NA</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 7</span><span> Afga… AF    AFG    </span><span style='text-decoration: underline;'>1</span><span>986              </span><span style='color: #BB0000;'>NA</span><span>               </span><span style='color: #BB0000;'>NA</span><span>               </span><span style='color: #BB0000;'>NA</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 8</span><span> Afga… AF    AFG    </span><span style='text-decoration: underline;'>1</span><span>987              </span><span style='color: #BB0000;'>NA</span><span>               </span><span style='color: #BB0000;'>NA</span><span>               </span><span style='color: #BB0000;'>NA</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 9</span><span> Afga… AF    AFG    </span><span style='text-decoration: underline;'>1</span><span>988              </span><span style='color: #BB0000;'>NA</span><span>               </span><span style='color: #BB0000;'>NA</span><span>               </span><span style='color: #BB0000;'>NA</span></span>
<span class='c'>#&gt; <span style='color: #555555;'>10</span><span> Afga… AF    AFG    </span><span style='text-decoration: underline;'>1</span><span>989              </span><span style='color: #BB0000;'>NA</span><span>               </span><span style='color: #BB0000;'>NA</span><span>               </span><span style='color: #BB0000;'>NA</span></span>
<span class='c'>#&gt; <span style='color: #555555;'># … with 7,230 more rows, and 53 more variables: nuevos_fpp_h3544 </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span style='color: #555555;'>,</span></span>
<span class='c'>#&gt; <span style='color: #555555;'>#   nuevos_fpp_h4554 </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span style='color: #555555;'>, nuevos_fpp_h5564 </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span style='color: #555555;'>, nuevos_fpp_h65 </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span style='color: #555555;'>,</span></span>
<span class='c'>#&gt; <span style='color: #555555;'>#   nuevos_fpp_m014 </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span style='color: #555555;'>, nuevos_fpp_m1524 </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span style='color: #555555;'>, nuevos_fpp_m2534 </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span style='color: #555555;'>,</span></span>
<span class='c'>#&gt; <span style='color: #555555;'>#   nuevos_fpp_m3544 </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span style='color: #555555;'>, nuevos_fpp_m4554 </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span style='color: #555555;'>, nuevos_fpp_m5564 </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span style='color: #555555;'>,</span></span>
<span class='c'>#&gt; <span style='color: #555555;'>#   nuevos_fpp_m65 </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span style='color: #555555;'>, nuevos_fpn_h014 </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span style='color: #555555;'>, nuevos_fpn_h1524 </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span style='color: #555555;'>,</span></span>
<span class='c'>#&gt; <span style='color: #555555;'>#   nuevos_fpn_h2534 </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span style='color: #555555;'>, nuevos_fpn_h3544 </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span style='color: #555555;'>, nuevos_fpn_h4554 </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span style='color: #555555;'>,</span></span>
<span class='c'>#&gt; <span style='color: #555555;'>#   nuevos_fpn_h5564 </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span style='color: #555555;'>, nuevos_fpn_h65 </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span style='color: #555555;'>, nuevos_fpn_m014 </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span style='color: #555555;'>,</span></span>
<span class='c'>#&gt; <span style='color: #555555;'>#   nuevos_fpn_m1524 </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span style='color: #555555;'>, nuevos_fpn_m2534 </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span style='color: #555555;'>, nuevos_fpn_m3544 </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span style='color: #555555;'>,</span></span>
<span class='c'>#&gt; <span style='color: #555555;'>#   nuevos_fpn_m4554 </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span style='color: #555555;'>, nuevos_fpn_m5564 </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span style='color: #555555;'>, nuevos_fpn_m65 </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span style='color: #555555;'>,</span></span>
<span class='c'>#&gt; <span style='color: #555555;'>#   nuevos_ep_h014 </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span style='color: #555555;'>, nuevos_ep_h1524 </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span style='color: #555555;'>, nuevos_ep_h2534 </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span style='color: #555555;'>,</span></span>
<span class='c'>#&gt; <span style='color: #555555;'>#   nuevos_ep_h3544 </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span style='color: #555555;'>, nuevos_ep_h4554 </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span style='color: #555555;'>, nuevos_ep_h5564 </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span style='color: #555555;'>,</span></span>
<span class='c'>#&gt; <span style='color: #555555;'>#   nuevos_ep_h65 </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span style='color: #555555;'>, nuevos_ep_m014 </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span style='color: #555555;'>, nuevos_ep_m1524 </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span style='color: #555555;'>,</span></span>
<span class='c'>#&gt; <span style='color: #555555;'>#   nuevos_ep_m2534 </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span style='color: #555555;'>, nuevos_ep_m3544 </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span style='color: #555555;'>, nuevos_ep_m4554 </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span style='color: #555555;'>,</span></span>
<span class='c'>#&gt; <span style='color: #555555;'>#   nuevos_ep_m5564 </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span style='color: #555555;'>, nuevos_ep_m65 </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span style='color: #555555;'>, nuevosrecaida_h014 </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span style='color: #555555;'>,</span></span>
<span class='c'>#&gt; <span style='color: #555555;'>#   nuevosrecaida_h1524 </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span style='color: #555555;'>, nuevosrecaida_h2534 </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span style='color: #555555;'>,</span></span>
<span class='c'>#&gt; <span style='color: #555555;'>#   nuevosrecaida_h3544 </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span style='color: #555555;'>, nuevosrecaida_h4554 </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span style='color: #555555;'>,</span></span>
<span class='c'>#&gt; <span style='color: #555555;'>#   nuevosrecaida_h5564 </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span style='color: #555555;'>, nuevosrecaida_h65 </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span style='color: #555555;'>,</span></span>
<span class='c'>#&gt; <span style='color: #555555;'>#   nuevosrecaida_m014 </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span style='color: #555555;'>, nuevosrecaida_m1524 </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span style='color: #555555;'>,</span></span>
<span class='c'>#&gt; <span style='color: #555555;'>#   nuevosrecaida_m2534 </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span style='color: #555555;'>, nuevosrecaida_m3544 </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span style='color: #555555;'>,</span></span>
<span class='c'>#&gt; <span style='color: #555555;'>#   nuevosrecaida_m4554 </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span style='color: #555555;'>, nuevosrecaida_m5564 </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span style='color: #555555;'>,</span></span>
<span class='c'>#&gt; <span style='color: #555555;'>#   nuevosrecaida_m65 </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span></span>
</code></pre>

</div>

En la salida se observa un ejemplo muy típico de una base de datos de la vida real. Contiene columnas redundantes, códigos extraños de variables y muchos valores faltantes. Practicamente, la base de datos `oms` está desordenado, por tanto, se necesita ordenarlo de manera sencilla con tidyverse.

### Pasos de ordenación

Necesitamos agrupar todas las columnas desde `nuevos_fpp_h014 hasta recaidas_m65`. No sabemos aún qué representa esto, por lo que le daremos el nombre genérico de `"clave"`. Sabemos que las celdas representan la cuenta de casos, por lo que usaremos la variable `casos`.

Existen múltiples valores faltantes en la representación actual, por lo que de momento usaremos `na.rm` para centrarnos en los valores que están presentes.

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nv'>oms1</span> <span class='o'>&lt;-</span> <span class='nv'>oms</span> <span class='o'>%&gt;%</span>
  <span class='nf'>pivot_longer</span><span class='o'>(</span>
    cols <span class='o'>=</span> <span class='nv'>nuevos_fpp_h014</span><span class='o'>:</span><span class='nv'>nuevosrecaida_m65</span>, 
    names_to <span class='o'>=</span> <span class='s'>"clave"</span>, 
    values_to <span class='o'>=</span> <span class='s'>"casos"</span>, 
    values_drop_na <span class='o'>=</span> <span class='kc'>TRUE</span>
  <span class='o'>)</span>
<span class='nv'>oms1</span>

<span class='c'>#&gt; <span style='color: #555555;'># A tibble: 76,046 x 6</span></span>
<span class='c'>#&gt;    pais       iso2  iso3   anio clave            casos</span>
<span class='c'>#&gt;    <span style='color: #555555;font-style: italic;'>&lt;chr&gt;</span><span>      </span><span style='color: #555555;font-style: italic;'>&lt;chr&gt;</span><span> </span><span style='color: #555555;font-style: italic;'>&lt;chr&gt;</span><span> </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span> </span><span style='color: #555555;font-style: italic;'>&lt;chr&gt;</span><span>            </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 1</span><span> Afganistán AF    AFG    </span><span style='text-decoration: underline;'>1</span><span>997 nuevos_fpp_h014      0</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 2</span><span> Afganistán AF    AFG    </span><span style='text-decoration: underline;'>1</span><span>997 nuevos_fpp_h1524    10</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 3</span><span> Afganistán AF    AFG    </span><span style='text-decoration: underline;'>1</span><span>997 nuevos_fpp_h2534     6</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 4</span><span> Afganistán AF    AFG    </span><span style='text-decoration: underline;'>1</span><span>997 nuevos_fpp_h3544     3</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 5</span><span> Afganistán AF    AFG    </span><span style='text-decoration: underline;'>1</span><span>997 nuevos_fpp_h4554     5</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 6</span><span> Afganistán AF    AFG    </span><span style='text-decoration: underline;'>1</span><span>997 nuevos_fpp_h5564     2</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 7</span><span> Afganistán AF    AFG    </span><span style='text-decoration: underline;'>1</span><span>997 nuevos_fpp_h65       0</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 8</span><span> Afganistán AF    AFG    </span><span style='text-decoration: underline;'>1</span><span>997 nuevos_fpp_m014      5</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 9</span><span> Afganistán AF    AFG    </span><span style='text-decoration: underline;'>1</span><span>997 nuevos_fpp_m1524    38</span></span>
<span class='c'>#&gt; <span style='color: #555555;'>10</span><span> Afganistán AF    AFG    </span><span style='text-decoration: underline;'>1</span><span>997 nuevos_fpp_m2534    36</span></span>
<span class='c'>#&gt; <span style='color: #555555;'># … with 76,036 more rows</span></span>
</code></pre>

</div>

Para visualizar el conteo de valores en la nueva columna `clave`:

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nv'>oms1</span> <span class='o'>%&gt;%</span>
  <span class='nf'>count</span><span class='o'>(</span><span class='nv'>clave</span><span class='o'>)</span>

<span class='c'>#&gt; <span style='color: #555555;'># A tibble: 56 x 2</span></span>
<span class='c'>#&gt;    clave               n</span>
<span class='c'>#&gt;    <span style='color: #555555;font-style: italic;'>&lt;chr&gt;</span><span>           </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 1</span><span> nuevos_ep_h014   </span><span style='text-decoration: underline;'>1</span><span>038</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 2</span><span> nuevos_ep_h1524  </span><span style='text-decoration: underline;'>1</span><span>026</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 3</span><span> nuevos_ep_h2534  </span><span style='text-decoration: underline;'>1</span><span>020</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 4</span><span> nuevos_ep_h3544  </span><span style='text-decoration: underline;'>1</span><span>024</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 5</span><span> nuevos_ep_h4554  </span><span style='text-decoration: underline;'>1</span><span>020</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 6</span><span> nuevos_ep_h5564  </span><span style='text-decoration: underline;'>1</span><span>015</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 7</span><span> nuevos_ep_h65    </span><span style='text-decoration: underline;'>1</span><span>018</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 8</span><span> nuevos_ep_m014   </span><span style='text-decoration: underline;'>1</span><span>032</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 9</span><span> nuevos_ep_m1524  </span><span style='text-decoration: underline;'>1</span><span>021</span></span>
<span class='c'>#&gt; <span style='color: #555555;'>10</span><span> nuevos_ep_m2534  </span><span style='text-decoration: underline;'>1</span><span>021</span></span>
<span class='c'>#&gt; <span style='color: #555555;'># … with 46 more rows</span></span>
</code></pre>

</div>

Para entender el significado de cada variable, se dispone de un diccionario de datos a mano. Este dice lo siguiente:

1.  Lo que aparece antes del primer `_` en las columnas indica si la columna contiene casos nuevos o antiguos de tuberculosis. En este dataset, cada columna contiene nuevos casos.

2.  Lo que aparece luego de indicar si se refiere casos nuevos o antiguos es el tipo de tuberculosis:

-   `recaida` se refiere a casos reincidentes
-   `ep` se refiere a tuberculosis extra pulmonar
-   `fpn` se refiere a casos de tuberculosis pulmonar que no se pueden detectar mediante examen de frotis pulmonar (frotis pulmonar negativo)
-   `fpp` se refiere a casos de tuberculosis pulmonar que se pueden detectar mediante examen de frotis pulmonar (frotis pulmonar positivo)

1.  La letra que aparece después del último `_` se refiere al sexo de los pacientes. El conjunto de datos agrupa en hombres (`h`) y mujeres (`m`).

2.  Los números finales se refieren al grupo etareo que se ha organizado en siete categorías:

-   `014` = `0 - 14` años de edad
-   `1524` = `15 – 24` años de edad
-   `2534` = `25 – 34` años de edad
-   `3544` = `35 – 44` años de edad
-   `4554` = `45 – 54` años de edad
-   `5564` = `55 – 64` años de edad
-   `65` = `65` o más años de edad

Necesitamos hacer un pequeño cambio al formato de los nombres de las columnas: desafortunadamente lo nombres de las columnas son ligeramente inconsistentes debido a que en lugar de `nuevos_recaida` tenemos `nuevosrecaida` (es difícil darse cuenta de esto en esta parte, pero si no lo arreglas habrá errores en los pasos siguientes). Para esto, la idea básica es bastante simple: reemplazar los caracteres `“nuevosrecaida”` por `“nuevos_recaida”`. Esto genera nombres de variables consistentes.

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nv'>oms2</span> <span class='o'>&lt;-</span> <span class='nv'>oms1</span> <span class='o'>%&gt;%</span>
  <span class='nf'>mutate</span><span class='o'>(</span>clave <span class='o'>=</span> <span class='nf'>stringr</span><span class='nf'>::</span><span class='nf'><a href='https://stringr.tidyverse.org/reference/str_replace.html'>str_replace</a></span><span class='o'>(</span><span class='nv'>clave</span>, <span class='s'>"nuevosrecaida"</span>, <span class='s'>"nuevos_recaida"</span><span class='o'>)</span><span class='o'>)</span>
<span class='nv'>oms2</span>

<span class='c'>#&gt; <span style='color: #555555;'># A tibble: 76,046 x 6</span></span>
<span class='c'>#&gt;    pais       iso2  iso3   anio clave            casos</span>
<span class='c'>#&gt;    <span style='color: #555555;font-style: italic;'>&lt;chr&gt;</span><span>      </span><span style='color: #555555;font-style: italic;'>&lt;chr&gt;</span><span> </span><span style='color: #555555;font-style: italic;'>&lt;chr&gt;</span><span> </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span> </span><span style='color: #555555;font-style: italic;'>&lt;chr&gt;</span><span>            </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 1</span><span> Afganistán AF    AFG    </span><span style='text-decoration: underline;'>1</span><span>997 nuevos_fpp_h014      0</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 2</span><span> Afganistán AF    AFG    </span><span style='text-decoration: underline;'>1</span><span>997 nuevos_fpp_h1524    10</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 3</span><span> Afganistán AF    AFG    </span><span style='text-decoration: underline;'>1</span><span>997 nuevos_fpp_h2534     6</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 4</span><span> Afganistán AF    AFG    </span><span style='text-decoration: underline;'>1</span><span>997 nuevos_fpp_h3544     3</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 5</span><span> Afganistán AF    AFG    </span><span style='text-decoration: underline;'>1</span><span>997 nuevos_fpp_h4554     5</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 6</span><span> Afganistán AF    AFG    </span><span style='text-decoration: underline;'>1</span><span>997 nuevos_fpp_h5564     2</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 7</span><span> Afganistán AF    AFG    </span><span style='text-decoration: underline;'>1</span><span>997 nuevos_fpp_h65       0</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 8</span><span> Afganistán AF    AFG    </span><span style='text-decoration: underline;'>1</span><span>997 nuevos_fpp_m014      5</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 9</span><span> Afganistán AF    AFG    </span><span style='text-decoration: underline;'>1</span><span>997 nuevos_fpp_m1524    38</span></span>
<span class='c'>#&gt; <span style='color: #555555;'>10</span><span> Afganistán AF    AFG    </span><span style='text-decoration: underline;'>1</span><span>997 nuevos_fpp_m2534    36</span></span>
<span class='c'>#&gt; <span style='color: #555555;'># … with 76,036 more rows</span></span>
</code></pre>

</div>

Una vez reemplazado, nos facilita separar los valores en cada código aplicando `separate()` dos veces. La primera aplicación dividirá los códigos en cada `_`.

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nv'>oms3</span> <span class='o'>&lt;-</span> <span class='nv'>oms2</span> <span class='o'>%&gt;%</span>
  <span class='nf'>separate</span><span class='o'>(</span><span class='nv'>clave</span>, <span class='nf'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='o'>(</span><span class='s'>"nuevos"</span>, <span class='s'>"tipo"</span>, <span class='s'>"sexo_edad"</span><span class='o'>)</span>, sep <span class='o'>=</span> <span class='s'>"_"</span><span class='o'>)</span>
<span class='nv'>oms3</span>

<span class='c'>#&gt; <span style='color: #555555;'># A tibble: 76,046 x 8</span></span>
<span class='c'>#&gt;    pais       iso2  iso3   anio nuevos tipo  sexo_edad casos</span>
<span class='c'>#&gt;    <span style='color: #555555;font-style: italic;'>&lt;chr&gt;</span><span>      </span><span style='color: #555555;font-style: italic;'>&lt;chr&gt;</span><span> </span><span style='color: #555555;font-style: italic;'>&lt;chr&gt;</span><span> </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span> </span><span style='color: #555555;font-style: italic;'>&lt;chr&gt;</span><span>  </span><span style='color: #555555;font-style: italic;'>&lt;chr&gt;</span><span> </span><span style='color: #555555;font-style: italic;'>&lt;chr&gt;</span><span>     </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 1</span><span> Afganistán AF    AFG    </span><span style='text-decoration: underline;'>1</span><span>997 nuevos fpp   h014          0</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 2</span><span> Afganistán AF    AFG    </span><span style='text-decoration: underline;'>1</span><span>997 nuevos fpp   h1524        10</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 3</span><span> Afganistán AF    AFG    </span><span style='text-decoration: underline;'>1</span><span>997 nuevos fpp   h2534         6</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 4</span><span> Afganistán AF    AFG    </span><span style='text-decoration: underline;'>1</span><span>997 nuevos fpp   h3544         3</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 5</span><span> Afganistán AF    AFG    </span><span style='text-decoration: underline;'>1</span><span>997 nuevos fpp   h4554         5</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 6</span><span> Afganistán AF    AFG    </span><span style='text-decoration: underline;'>1</span><span>997 nuevos fpp   h5564         2</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 7</span><span> Afganistán AF    AFG    </span><span style='text-decoration: underline;'>1</span><span>997 nuevos fpp   h65           0</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 8</span><span> Afganistán AF    AFG    </span><span style='text-decoration: underline;'>1</span><span>997 nuevos fpp   m014          5</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 9</span><span> Afganistán AF    AFG    </span><span style='text-decoration: underline;'>1</span><span>997 nuevos fpp   m1524        38</span></span>
<span class='c'>#&gt; <span style='color: #555555;'>10</span><span> Afganistán AF    AFG    </span><span style='text-decoration: underline;'>1</span><span>997 nuevos fpp   m2534        36</span></span>
<span class='c'>#&gt; <span style='color: #555555;'># … with 76,036 more rows</span></span>
</code></pre>

</div>

A continuación podemos eliminar la columna `nuevos`, ya que es constante en este dataset. Además eliminaremos `iso2` e `iso3` ya que son redundantes.

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nv'>oms3</span> <span class='o'>%&gt;%</span>
  <span class='nf'>count</span><span class='o'>(</span><span class='nv'>nuevos</span><span class='o'>)</span>

<span class='c'>#&gt; <span style='color: #555555;'># A tibble: 1 x 2</span></span>
<span class='c'>#&gt;   nuevos     n</span>
<span class='c'>#&gt;   <span style='color: #555555;font-style: italic;'>&lt;chr&gt;</span><span>  </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span></span>
<span class='c'>#&gt; <span style='color: #555555;'>1</span><span> nuevos </span><span style='text-decoration: underline;'>76</span><span>046</span></span>


<span class='nv'>oms4</span> <span class='o'>&lt;-</span> <span class='nv'>oms3</span> <span class='o'>%&gt;%</span>
  <span class='nf'>select</span><span class='o'>(</span><span class='o'>-</span><span class='nv'>nuevos</span>, <span class='o'>-</span><span class='nv'>iso2</span>, <span class='o'>-</span><span class='nv'>iso3</span><span class='o'>)</span>
</code></pre>

</div>

Luego separamos `sexo_edad` en `sexo` y `edad` dividiendo luego del primer carácter:

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nv'>oms5</span> <span class='o'>&lt;-</span> <span class='nv'>oms4</span> <span class='o'>%&gt;%</span>
  <span class='nf'>separate</span><span class='o'>(</span><span class='nv'>sexo_edad</span>, <span class='nf'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='o'>(</span><span class='s'>"sexo"</span>, <span class='s'>"edad"</span><span class='o'>)</span>, sep <span class='o'>=</span> <span class='m'>1</span><span class='o'>)</span>
<span class='nv'>oms5</span>

<span class='c'>#&gt; <span style='color: #555555;'># A tibble: 76,046 x 6</span></span>
<span class='c'>#&gt;    pais        anio tipo  sexo  edad  casos</span>
<span class='c'>#&gt;    <span style='color: #555555;font-style: italic;'>&lt;chr&gt;</span><span>      </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span> </span><span style='color: #555555;font-style: italic;'>&lt;chr&gt;</span><span> </span><span style='color: #555555;font-style: italic;'>&lt;chr&gt;</span><span> </span><span style='color: #555555;font-style: italic;'>&lt;chr&gt;</span><span> </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 1</span><span> Afganistán  </span><span style='text-decoration: underline;'>1</span><span>997 fpp   h     014       0</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 2</span><span> Afganistán  </span><span style='text-decoration: underline;'>1</span><span>997 fpp   h     1524     10</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 3</span><span> Afganistán  </span><span style='text-decoration: underline;'>1</span><span>997 fpp   h     2534      6</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 4</span><span> Afganistán  </span><span style='text-decoration: underline;'>1</span><span>997 fpp   h     3544      3</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 5</span><span> Afganistán  </span><span style='text-decoration: underline;'>1</span><span>997 fpp   h     4554      5</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 6</span><span> Afganistán  </span><span style='text-decoration: underline;'>1</span><span>997 fpp   h     5564      2</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 7</span><span> Afganistán  </span><span style='text-decoration: underline;'>1</span><span>997 fpp   h     65        0</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 8</span><span> Afganistán  </span><span style='text-decoration: underline;'>1</span><span>997 fpp   m     014       5</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 9</span><span> Afganistán  </span><span style='text-decoration: underline;'>1</span><span>997 fpp   m     1524     38</span></span>
<span class='c'>#&gt; <span style='color: #555555;'>10</span><span> Afganistán  </span><span style='text-decoration: underline;'>1</span><span>997 fpp   m     2534     36</span></span>
<span class='c'>#&gt; <span style='color: #555555;'># … with 76,036 more rows</span></span>
</code></pre>

</div>

¡Ahora la base de datos `oms` está ordenado!

Resumen
-------

En la anterior sección se hizo el procedimiento de ordenación paso a paso, asignando los resultados intermedios a nuevas variables. Esta no es la forma típica de trabajo. En realidad, los códigos debería ser de manera incremental usando `pipes ("%>%)`:

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nv'>oms</span> <span class='o'>%&gt;%</span>
  <span class='nf'>pivot_longer</span><span class='o'>(</span>
    cols <span class='o'>=</span> <span class='nv'>nuevos_fpp_h014</span><span class='o'>:</span><span class='nv'>nuevosrecaida_m65</span>,
    names_to <span class='o'>=</span> <span class='s'>"clave"</span>, 
    values_to <span class='o'>=</span> <span class='s'>"valor"</span>, 
    values_drop_na <span class='o'>=</span> <span class='kc'>TRUE</span><span class='o'>)</span> <span class='o'>%&gt;%</span>
  <span class='nf'>mutate</span><span class='o'>(</span>clave <span class='o'>=</span> <span class='nf'>stringr</span><span class='nf'>::</span><span class='nf'><a href='https://stringr.tidyverse.org/reference/str_replace.html'>str_replace</a></span><span class='o'>(</span><span class='nv'>clave</span>, <span class='s'>"nuevosrecaida"</span>, <span class='s'>"nuevos_recaida"</span><span class='o'>)</span><span class='o'>)</span> <span class='o'>%&gt;%</span>
  <span class='nf'>separate</span><span class='o'>(</span><span class='nv'>clave</span>, <span class='nf'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='o'>(</span><span class='s'>"nuevos"</span>, <span class='s'>"tipo"</span>, <span class='s'>"sexo_edad"</span><span class='o'>)</span><span class='o'>)</span> <span class='o'>%&gt;%</span>
  <span class='nf'>select</span><span class='o'>(</span><span class='o'>-</span><span class='nv'>nuevos</span>, <span class='o'>-</span><span class='nv'>iso2</span>, <span class='o'>-</span><span class='nv'>iso3</span><span class='o'>)</span> <span class='o'>%&gt;%</span>
  <span class='nf'>separate</span><span class='o'>(</span><span class='nv'>sexo_edad</span>, <span class='nf'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='o'>(</span><span class='s'>"sexo"</span>, <span class='s'>"edad"</span><span class='o'>)</span>, sep <span class='o'>=</span> <span class='m'>1</span><span class='o'>)</span>

<span class='c'>#&gt; <span style='color: #555555;'># A tibble: 76,046 x 6</span></span>
<span class='c'>#&gt;    pais        anio tipo  sexo  edad  valor</span>
<span class='c'>#&gt;    <span style='color: #555555;font-style: italic;'>&lt;chr&gt;</span><span>      </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span> </span><span style='color: #555555;font-style: italic;'>&lt;chr&gt;</span><span> </span><span style='color: #555555;font-style: italic;'>&lt;chr&gt;</span><span> </span><span style='color: #555555;font-style: italic;'>&lt;chr&gt;</span><span> </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 1</span><span> Afganistán  </span><span style='text-decoration: underline;'>1</span><span>997 fpp   h     014       0</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 2</span><span> Afganistán  </span><span style='text-decoration: underline;'>1</span><span>997 fpp   h     1524     10</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 3</span><span> Afganistán  </span><span style='text-decoration: underline;'>1</span><span>997 fpp   h     2534      6</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 4</span><span> Afganistán  </span><span style='text-decoration: underline;'>1</span><span>997 fpp   h     3544      3</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 5</span><span> Afganistán  </span><span style='text-decoration: underline;'>1</span><span>997 fpp   h     4554      5</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 6</span><span> Afganistán  </span><span style='text-decoration: underline;'>1</span><span>997 fpp   h     5564      2</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 7</span><span> Afganistán  </span><span style='text-decoration: underline;'>1</span><span>997 fpp   h     65        0</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 8</span><span> Afganistán  </span><span style='text-decoration: underline;'>1</span><span>997 fpp   m     014       5</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 9</span><span> Afganistán  </span><span style='text-decoration: underline;'>1</span><span>997 fpp   m     1524     38</span></span>
<span class='c'>#&gt; <span style='color: #555555;'>10</span><span> Afganistán  </span><span style='text-decoration: underline;'>1</span><span>997 fpp   m     2534     36</span></span>
<span class='c'>#&gt; <span style='color: #555555;'># … with 76,036 more rows</span></span>
</code></pre>

</div>

Conclusión
----------

Es un ejemplo muy bueno para practicar y usar las diferentes funciones de `tidyverse` en la ordenación de datos.

