---
title: "Análise do PIB do Estado de São Paulo"
author: "Thais Pereira"
date: "2024-06-13"
excerpt: "Esse é um mapa que nos mostra a contribuição de cada uma das áreas cidades e das áreas econômicas para o PIB"
output: html_document
featured: true
layout: "single"
links:
- icon: github
  icon_pack: fab
  name: code
  url: https://github.com/thais01fernandes/maps/blob/main/infografrico.Rmd
draft: false
---


{{< figure src="mapa_final.png" alt="Traditional right sidebar layout" caption="" >}}

**Código para fazer esse mapa:**


``` r
library(ggplot2)
library(dplyr)
library(MetBrewer)
library(showtext)
library(readxl)
library(geobr)
library(tidyverse)
library(here)
library(sf)
library(patchwork)
library(MoMAColors)

#devtools::install_github("BlakeRMills/MoMAColors")

file_1  <- "https://raw.githubusercontent.com/thais01fernandes/maps/main/dados_pib_2021.csv"
  panel  <- read_delim(file_1, delim = ",", 
                             locale = locale(encoding='UTF-8')) |>  
    select(-1)
```






``` r
# baixando dados geográficos:

munis <- read_municipality(code_muni= 35, year=2020, showProgress = F) |>   as.tibble()

geostate <- read_state(code_state = 35, year = 2020, showProgress = F) |>  as.tibble()

# organizando os dados de PIB:


pib_cols <- c("pib_agriculture", "pib_industrial", "pib_services",
             "pib_govmt_services")

pib_share <- panel |>  
  filter(`Sigla da Unidade da Federação` == "SP") |>  
  select(-2:-5, -9:-32, -37, -38, -40) |>  janitor::clean_names() |> 
  rename("code_muni" = "codigo_do_municipio", 
         "pib_agriculture" = "valor_adicionado_bruto_da_agropecuaria_a_precos_correntes_r_1_000",
         "pib_industrial" = "valor_adicionado_bruto_da_industria_a_precos_correntes_r_1_000",
         "pib_services" =  "valor_adicionado_bruto_dos_servicos_a_precos_correntes_exceto_administracao_defesa_educacao_e_saude_publicas_e_seguridade_social_r_1_000",
         "pib_govmt_services" =  "valor_adicionado_bruto_da_administracao_defesa_educacao_e_saude_publicas_e_seguridade_social_a_precos_correntes_r_1_000", 
         "pib" = "produto_interno_bruto_a_precos_correntes_r_1_000") |>  
  select(all_of(c("code_muni", "pib", pib_cols))) |> 
  pivot_longer(cols = 3:last_col(), names_to = "sector", values_to = "total") |> 
  mutate(
    share = total / pib * 100,
    sector_label = str_remove(sector, "pib_"),
    sector_label = str_to_title(sector_label),
    sector_label = str_replace(sector_label, "Govmt_services", "Government"),
    sector_label = str_replace(sector_label, "Industrial", "Industry")
    )


# Convertendo para wider
tab_pib_share <- pib_share |> 
  pivot_wider(
    id_cols = "code_muni",
    names_from = "sector",
    values_from = "share"
    )         

        
# encontrando o setor economico mais relevante por cidade 

top_pib_share <- pib_share |> 
  filter(share == max(share), .by = "code_muni") |> 
  select(code_muni, top_sector = sector_label)
         
# Juntando os dados do pib com os dados geográficos

map_share <- munis |> 
  left_join(tab_pib_share, by = "code_muni") |> 
  left_join(top_pib_share, by = "code_muni")  


  # Paleta de cores para usar no mapa

cores = met.brewer("Kandinsky", 20)[c(1, 8, 15, 20)]

# layout do mapa:

theme_2 <- function() {
  theme_minimal() %+replace%
    theme(panel.grid = element_blank(),
          plot.background = element_rect(fill = "#ffffff", color = NA),
          panel.background = element_rect(fill = "#ffffff", color = NA),
          text = element_text(colour = "black", family = "Segoe UI"),
          plot.title = element_text(size = 22,  hjust = -0.8, family = "Segoe UI", color = "#50595c"),
          axis.text = element_blank(),
          plot.subtitle = element_text(size = 11, hjust = -0.98, vjust = -1, color = "#50595c"),
          axis.title = element_blank(),
          axis.ticks = element_blank(),
          legend.text =  element_text(size = 10,  family = "Segoe UI"),
          legend.position = "none",
          legend.justification = 0.5,
          legend.title = element_blank())}
```




``` r
geostate <- st_as_sf(geostate)
map_share <- st_as_sf(map_share)

# Mapa

mapa_1 <- ggplot() +
  # State borders
  geom_sf(data = geostate, lwd = 0.3, color = "gray30", fill = "white") +
  # City colors
  geom_sf(
    data = na.omit(map_share),
    aes(fill = top_sector),
    lwd = 0.01,
    color = "gray"
    ) +
  scale_fill_manual(name = "", values = cores) +
  labs(
    title = "Contribuição do PIB por Cidade",
    subtitle = "As cores representam a àrea econômica mais relevante em cada cidade",
    caption = "Source: National Accounts, IBGE (2021)") +
 theme_2()


# Gráfico de barras

barrinhas <- top_pib_share %>% 
  group_by(top_sector) %>% 
  tally() %>% 
  mutate(pct = n/sum(n)) %>% 
  ggplot() +
  geom_col(aes(x = reorder(top_sector, pct), y =  pct, fill = top_sector)) + 
  theme_void()+
  coord_flip()+
  theme( axis.text.y=element_text(size = 8, family = "Rubik"),
        legend.position = "none")+
  scale_fill_manual(name = "", values = cores) +
  geom_text(
    aes(x = top_sector, y =  pct, label = scales::percent(pct, accuracy = 1)),
    size = 2.5,
    hjust = 1.2,
    vjust = 0.5,
    family = "Segoe UI",
    color = "white", 
    fontface = "bold")
```




``` r
# juntando o mapa com o gráfico de barras

mapa_final <- mapa_1 + inset_element(barrinhas, left = -0.15, bottom = -0.05, right = 0.36, top =  0.3)

# salvando o mapa em formato png

ggsave("mapa_final.png", plot = mapa_final, width = 4, height = 3, units = "in", dpi = 400)
```





