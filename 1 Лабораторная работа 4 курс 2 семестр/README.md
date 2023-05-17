# Задание 1: Надите утечку данных из Вашей сети

Один из хостов в нашей сети используется для пересылки этой
информации – он пересылает гораздо больше информации на внешние ресурсы
в Интернете, чем остальные компьютеры нашей сети. Определите его
IP-адрес.

```r
library(stringr)
library(tidymodels)
library(arrow)
library(dplyr)
library(ggplot2)
```

## Импорт датасета и присвоение имён признакам

```r
df_data <- arrow::read_csv_arrow("traffic_security.csv")
```

```r
colnames(df_data) <- c('timestamp','src','dst','port','bytes')
head(df_data,3)
```

## Очистка датасет, с оставлением в src нашего ip-адрес

```r
knitr::opts_chunk$set(
  df_data <- df_data[df_data$src > 11 & df_data$src < 15 & df_data$dst < 11 | df_data$dst > 15, ]
)
```

## Найдём ip-адрес и максимальное число передаваемых байтов

```r
knitr::opts_chunk$set(
 found_ip1 <- df_data %>%
            group_by(src) %>%
            summarise(bytes = mean(bytes)),
  found_ip1 <- found_ip1[which.max(found_ip1$bytes),],
  print(found_ip1)
)
```

Ответ: 13.37.84.125
