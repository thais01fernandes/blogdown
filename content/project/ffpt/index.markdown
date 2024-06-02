---
title: Cidades com Tarifa Zero no Brasil e os Movimentos Sociais de 2013
author: Thais F. Pereira
date: "2023-02-18"
draft: false
excerpt: "Esse relatório contém análises desenvolvidas no escopo do meu trabalho de mestrado no departamento de Ciência Política da USP"
layout: single
links:
- icon: door-open
  icon_pack: fas
  name: website
  url: https://thais01fernandes.github.io/mestrado_usp/analysis.html
- icon: github
  icon_pack: fab
  name: code
  url: https://github.com/thais01fernandes/mestrado_usp
output: html_document

---





``` r
library("tidyverse")
```

```
## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
## ✔ dplyr     1.1.4     ✔ readr     2.1.5
## ✔ forcats   1.0.0     ✔ stringr   1.5.1
## ✔ ggplot2   3.5.1     ✔ tibble    3.2.1
## ✔ lubridate 1.9.3     ✔ tidyr     1.3.1
## ✔ purrr     1.0.2     
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors
```

``` r
library("kableExtra")
```

```
## 
## Anexando pacote: 'kableExtra'
## 
## O seguinte objeto é mascarado por 'package:dplyr':
## 
##     group_rows
```

``` r
library("knitr")
library("tidylog")
```

```
## 
## Anexando pacote: 'tidylog'
## 
## Os seguintes objetos são mascarados por 'package:dplyr':
## 
##     add_count, add_tally, anti_join, count, distinct, distinct_all,
##     distinct_at, distinct_if, filter, filter_all, filter_at, filter_if,
##     full_join, group_by, group_by_all, group_by_at, group_by_if,
##     inner_join, left_join, mutate, mutate_all, mutate_at, mutate_if,
##     relocate, rename, rename_all, rename_at, rename_if, rename_with,
##     right_join, sample_frac, sample_n, select, select_all, select_at,
##     select_if, semi_join, slice, slice_head, slice_max, slice_min,
##     slice_sample, slice_tail, summarise, summarise_all, summarise_at,
##     summarise_if, summarize, summarize_all, summarize_at, summarize_if,
##     tally, top_frac, top_n, transmute, transmute_all, transmute_at,
##     transmute_if, ungroup
## 
## Os seguintes objetos são mascarados por 'package:tidyr':
## 
##     drop_na, fill, gather, pivot_longer, pivot_wider, replace_na,
##     separate_wider_delim, separate_wider_position,
##     separate_wider_regex, spread, uncount
## 
## O seguinte objeto é mascarado por 'package:stats':
## 
##     filter
```

``` r
library("DT")
library("kableExtra")
library("tidylog")
library("readxl")
library("geobr")
```

```
## Carregando namespace exigido: sf
```

``` r
library("sf")
```

```
## Linking to GEOS 3.12.1, GDAL 3.8.4, PROJ 9.3.1; sf_use_s2() is TRUE
```

``` r
library("wesanderson")
library("ggplot2")
library("stringr")
library("stargazer")
```

```
## 
## Please cite as: 
## 
##  Hlavac, Marek (2022). stargazer: Well-Formatted Regression and Summary Statistics Tables.
##  R package version 5.2.3. https://CRAN.R-project.org/package=stargazer
```

``` r
library ("abjData")
library("extrafont")
```

```
## Registering fonts with R
```

``` r
library("gganimate")
library("gifski")
library("av")
library("lubridate")
library("transformr")
```

```
## 
## Anexando pacote: 'transformr'
## 
## O seguinte objeto é mascarado por 'package:sf':
## 
##     st_normalize
```

``` r
library("reactable")
library("rnaturalearth")
library("plotly")
```

```
## 
## Anexando pacote: 'plotly'
## 
## Os seguintes objetos são mascarados por 'package:tidylog':
## 
##     distinct, filter, group_by, mutate, rename, select, slice,
##     summarise, transmute, ungroup
## 
## O seguinte objeto é mascarado por 'package:ggplot2':
## 
##     last_plot
## 
## O seguinte objeto é mascarado por 'package:stats':
## 
##     filter
## 
## O seguinte objeto é mascarado por 'package:graphics':
## 
##     layout
```

``` r
library("leaflet")
```


## 17. Social Movements and Tarifa Zero













The map below contains the location of social movements that act or acted in Brazil for Tarifa Zero in public transport. The data were collected from the search for pages on facebook. 50 pages of social movements were found, among them, 12 are entitled "Movimento Tarifa Zero", and 38 of them, "Movimento Passe Livre", also bearing the name of the city of origin of the movement. 





```
## left_join: added 8 columns (nome, latitude, longitude, capital, codigo_uf, …)
##            > rows only in x                   0
##            > rows only in cities_lat_lng (    5)
##            > matched rows                 5,565
##            >                             =======
##            > rows total                   5,565
## left_join: added 8 columns (nome, latitude, longitude, capital, codigo_uf, …)
##            > rows only in x                   0
##            > rows only in cities_lat_lng (    5)
##            > matched rows                 5,565
##            >                             =======
##            > rows total                   5,565
## Assuming "longitude" and "latitude" are longitude and latitude, respectively
## 
## Assuming "longitude" and "latitude" are longitude and latitude, respectively
## 
## Assuming "longitude" and "latitude" are longitude and latitude, respectively
```



The table below shows us some information about the social movement pages found on facebook: 




---



