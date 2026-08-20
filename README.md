# Predicción de Demanda de un Sistema de Bicicletas Compartidas

Modelo predictivo que estima la demanda diaria de un sistema de *bike-sharing* a partir de variables climáticas y temporales, comparando tres algoritmos supervisados y explicando qué factores influyen más en el comportamiento de los usuarios.

**Random Forest — R² = 0.87 · RMSE = 696.6 · MAE = 498.1**

![Valores reales vs predichos](Valores%20Reales%20vs%20Predichos%20-%20Random%20forest.png)
---

## Contexto del problema

Un sistema de bicicletas compartidas necesita anticipar cuántas unidades van a alquilarse cada día para:
- Planificar mantenimiento sin afectar el servicio.
- Redistribuir bicicletas hacia zonas de mayor demanda.
- Anticipar picos y valles de uso según clima y estacionalidad.

Este proyecto responde a la pregunta: 
**¿qué tan bien se puede predecir la demanda diaria usando solo datos de clima, calendario y estacionalidad?**

## Dataset

[Bike Sharing Dataset](https://archive.ics.uci.edu/dataset/275/bike+sharing+dataset) (UCI Machine Learning Repository) — registros diarios de alquiler de bicicletas en Washington D.C., 2011–2012 (731 observaciones, 16 variables: clima, temporada, día laboral/feriado, temperatura, humedad, viento, y conteo de alquileres).

> El archivo `data/day.csv` no está incluido en este repo por convención de tamaño/licencia — descargalo del enlace de arriba y colocalo en `data/`.

## Metodología

1. **Exploración y limpieza**: verificación de estructura, tipos de datos, nulos y duplicados (dataset sin valores faltantes ni duplicados).
2. **Conversión de variables categóricas** (`season`, `weathersit`, `yr`, `weekday`) a factor.
3. **Análisis de outliers** mediante boxplots en variables numéricas clave.
4. **Análisis exploratorio de patrones temporales**: demanda por mes, por tipo de día, y su relación con temperatura.
5. **Modelado**: partición 70/30 con validación cruzada de 10 folds, comparando:
   - Regresión Lineal
   - Árbol de Decisión (rpart)
   - Random Forest
6. **Evaluación** con RMSE, MAE y R² sobre el conjunto de prueba independiente.
7. **Interpretación de importancia de variables** del modelo ganador.

## Resultados

| Modelo             | RMSE    | MAE    | R²     |
|---------------------|---------|--------|--------|
| **Random Forest**   | **696.57** | **498.13** | **0.8735** |
| Regresión Lineal    | 805.29  | 593.42 | 0.8286 |
| Árbol de Decisión   | 1304.22 | 1020.66| 0.5529 |

El Random Forest explica el **87.35%** de la variabilidad en la demanda diaria, superando a los modelos más simples al capturar relaciones no lineales entre clima, estacionalidad y uso del servicio.

**Variables más influyentes**: año (tendencia de crecimiento del servicio), humedad, temperatura y mes — confirmando que el clima y la estacionalidad son determinantes clave.

## Recomendaciones de negocio

- **Gestión estacional**: aprovechar noviembre–febrero (baja demanda) para mantenimiento preventivo.
- **Clima adverso**: incentivos (descuentos, puntos de fidelidad) en días de alta humedad o lluvia.
- **Fines de semana**: redistribuir unidades hacia zonas recreativas los viernes por la noche.
- **Crecimiento**: planificar ampliación gradual de la flota dado el crecimiento interanual observado.

## Stack técnico

`R` · `tidyverse` · `caret` · `rpart` / `rpart.plot` · `randomForest` · `ggplot2` · `janitor` · `knitr` / `kableExtra`

## Estructura del repo

```
bike-sharing-demand/
├── README.md
├── analisis_demanda_bicicletas.Rmd   # Análisis completo (código + narrativa)
├── data/
│   └── day.csv                       # Dataset (descargar del UCI Repository)
└── .gitignore
```

## Cómo reproducirlo

1. Descargá `day.csv` del [UCI Bike Sharing Dataset](https://archive.ics.uci.edu/dataset/275/bike+sharing+dataset) y colocalo en `data/`.
2. Abrí `analisis_demanda_bicicletas.Rmd` en RStudio.
3. Instalá las dependencias si hace falta:
   ```r
   install.packages(c("tidyverse", "janitor", "lubridate", "skimr", "scales",
                       "knitr", "kableExtra", "caret", "rpart", "rpart.plot", "e1071", "randomForest"))
   ```
4. Click en **Knit** para generar el reporte en HTML.

---

**Autores**: Rodder Gamboa Mora, Oscar Andino Quirós, Brandon Lanzoni Rodríguez
Proyecto académico — Ingeniería en Ciencia de Datos, Universidad Fidélitas
