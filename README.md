---
title: "Análise dos Gols Esperados (xG) - Campeonato Brasileiro 2024"
author: "ISRAEL MARTINEZ RODRIGUES"
date: "`r Sys.Date()`"
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```


# Instalar pacotes necessários

```
if (!requireNamespace("worldfootballR", quietly = TRUE)) {
  install.packages("worldfootballR")
}
if (!requireNamespace("tidyverse", quietly = TRUE)) {
  install.packages("tidyverse")
}
if (!requireNamespace("showtext", quietly = TRUE)) {
  install.packages("showtext")
}
```

# Carregar pacotes

```
library(worldfootballR)
library(dplyr)
library(ggplot2)
library(tidyverse)
library(showtext)
```

# Adicionar fonte

```
showtext_auto()
font_add("Arial", regular = "Arial.ttf")
```


# Buscar os dados de estatísticas de chutes dos jogadores da primeira divisão brasileira de 2024

```
prem_2024_player_shooting <- fb_league_stats(
  country = "BRA",
  gender = "M",
  season_end_year = 2024,
  tier = "1st",
  non_dom_league_url = NA,
  stat_type = "shooting",
  team_or_player = "player"
)
```

# Filtrando os 10 jogadores com o maior valor de xG

```
top_10_xg_players <- prem_2024_player_shooting %>%
  arrange(desc(xG_Expected)) %>%
  head(10)
```

# Criar uma nova coluna para combinar o nome do jogador e o time

```
top_10_xg_players <- top_10_xg_players %>%
  mutate(Player_Squad = paste(Player, Squad, sep = "\n"))
```


# Exibir os dados dos 10 jogadores
```
print(top_10_xg_players)
```

# Plotando o gráfico

```
ggplot(top_10_xg_players, aes(x = reorder(Player_Squad, -xG_Expected), y = xG_Expected)) +
  geom_bar(stat = "identity", fill = "#01204E") +
  geom_text(aes(label = round(xG_Expected, 2)), vjust = -0.5, family = "Arial") +
  labs(
    title = "Campeonato Brasileiro de Futebol de 2024 - Série A \nGols Esperados (xG) p/ jogo",
    x = "Jogador e Time",
    y = "xG_Expected (Gols esperados /90min)"
  ) +
  theme_minimal(base_family = "Arial") +
  theme(
    plot.background = element_rect(fill = "#DFD0B8", color = NA),
    panel.background = element_rect(fill = "#DFD0B8", color = NA),
    axis.text.x = element_text(angle = 45, hjust = 1)
  )
```

# Imprimir o gráfico

```
print(plot)
```

