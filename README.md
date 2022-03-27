---
title: "Matriz de correlación en R con ggplot2"
output: github_document
---

# Matriz de correlación

Una matriz de correlación es una tabla de coeficientes de correlación para un conjunto de variables que se utiliza para determinar si existe una relación entre las variables. El coeficiente indica tanto la fuerza de la relación como la dirección (correlaciones positivas frente a negativas).

## Librerias

Cargamos las librerías que usaremos en este trabajo: 

```{r warning=FALSE, message=FALSE}
library(ggplot2)
library(tibble)
library(psych)
library(reshape)
library(ggthemes)
```

## Base de datos

Utilizaremos una base de datos que consta de cinco variables cuantitativas edáficas de un suelo cultivado con banano, del cual se analiza la densidad aparente (da), la densidad real (dr), el porcentaje de porosidad total (ppt), el porcentaje de humedad del suelo (phs) y la resistencia a la compactación del suelo (rps). 

```{r  warning=FALSE, message=FALSE}

dirSuelo <- "https://raw.githubusercontent.com/pVillaGuerrero/-Correlation-Matrix/main/suelo.csv"
suelo <-read.csv(dirSuelo, header = T, sep = ",")
suelo <- na.omit(suelo)
```

## Cosnstrucción de la matriz de correlación

Mediante la función `cor` calculamos el coeficiente de correlación de Pearson, Kendall o Spearman para las variables cuantitativas.

```{r warning=FALSE, message=FALSE}
matSuelo = cor(suelo, method = "pearson")
matSuelo
```

## Matriz en formato largo

Usamos la función `melt` para pivotar la tabla y arreglar los datos y disponerlos en formato largo.

```{r warning=FALSE, message=FALSE}
corSuelo = melt(matSuelo)
corSuelo
```

## Formateo

### Nombres de las variables

Cambiaremos los nombres de las columnas de la tabla, para representar mejor el gráfico.

```{r warning=FALSE, message=FALSE}
names(corSuelo)=c("X1","X2","Correlación")
```

### Escala de correlación

Estableceremos nuestra escala de correlación, con los respectivos intervalos.

```{r warning=FALSE, message=FALSE}
escala = seq(-1,1,0.2)
escala
```

## Visualización de la matriz de correlación en ggplot2

En R, usaremos el paquete `ggplot2` para generar la matriz de correlación usando el objeto de formato largo llamado `corSuelo`. 

```{r warning=FALSE, message=FALSE}
ggplot(corSuelo, aes(x = X1, y = X2, fill = Correlación)) +
  geom_tile(aes(fill=Correlación))+
  scale_fill_gradient2(low = "#008837", mid = "#F7F7F7", high = "#7B3294", breaks=escala)+ 
  labs(x="", y="")+
  geom_text(aes(label = format(corSuelo$Correlación, digits=2)), color = "black", size = 4)+
  geom_text(aes(label = format(corSuelo$Correlación, digits=2)), color = "black", size = 4)+
  theme_few()+
  theme(axis.text.x = element_text(size = 10, angle = 90, hjust = 1, vjust = 0.5),
        axis.text.y = element_text(size = 10),
        legend.text = element_text(size = 8),
        legend.title = element_text(size = 10))
```
![Image text](https://github.com/pVillaGuerrero/-Correlation-Matrix/blob/main/unnamed-chunk-8-1.png?raw=true)
