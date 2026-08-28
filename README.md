# Demand Forecasting with Machine Learning

### Supply Chain Analytics | Python · Machine Learning · XGBoost

Proyecto de predicción de demanda desarrollado sobre datos históricos de ventas de Corporación Favorita, utilizando técnicas de análisis de datos, forecasting y Machine Learning.

El objetivo es predecir las ventas diarias por **tienda y familia de productos** para un horizonte futuro de **16 días**, simulando un problema real de planificación de demanda dentro de Supply Chain.

---

## Objetivo

Desarrollar un modelo capaz de anticipar la demanda utilizando información histórica de ventas, promociones, calendario, eventos, características de las tiendas y variables externas.

Desde una perspectiva de Supply Chain, un forecast más preciso puede apoyar decisiones relacionadas con:

- planificación de inventarios;
- reposición;
- abastecimiento;
- disponibilidad de productos;
- reducción del riesgo de sobrestock y quiebres de stock.

---

## Dataset

El proyecto utiliza el dataset **Store Sales - Time Series Forecasting** de Kaggle.

El conjunto de entrenamiento contiene más de **3 millones de observaciones**, correspondientes a:

- 54 tiendas;
- 33 familias de productos;
- período histórico entre 2013 y agosto de 2017;
- información diaria de ventas y promociones.

También se integraron datos de:

- características de las tiendas;
- feriados y eventos;
- precio del petróleo;
- transacciones por tienda.

Los archivos originales no se almacenan en este repositorio.

---

## Análisis exploratorio

El análisis permitió identificar varios patrones relevantes:

- aproximadamente 31% de las observaciones presentan ventas iguales a cero;
- existe una marcada estacionalidad semanal;
- las ventas promedio son mayores durante los fines de semana;
- las promociones presentan una fuerte asociación con mayores niveles de venta;
- existe una alta heterogeneidad entre familias de productos;
- algunas categorías presentan demanda altamente intermitente.

---

## Feature Engineering

Se construyeron variables orientadas a capturar comportamiento temporal y comercial:

- variables de calendario;
- día de la semana;
- fin de semana;
- eventos nacionales, regionales y locales;
- promociones;
- precio del petróleo;
- características de tienda;
- lags de ventas de 16, 21 y 28 días;
- media móvil de 28 días desplazada 16 días.

Los lags fueron construidos utilizando **días calendario**, evitando asumir que una fila equivale necesariamente a un día.

---

## Prevención de Data Leakage

Una parte central del proyecto fue garantizar que las variables utilizadas estuvieran disponibles al momento de generar una predicción real.

Por este motivo:

- se utilizó validación cronológica;
- no se utilizaron transacciones del mismo día como predictor;
- se evitaron lags inferiores al horizonte cuando podían requerir ventas futuras desconocidas;
- las medias móviles fueron desplazadas 16 días;
- los encoders fueron ajustados únicamente con los datos de entrenamiento correspondientes.

---

## Modelos

Se compararon tres enfoques:

| Modelo | RMSLE validación | Mejora vs. baseline |
|---|---:|---:|
| Seasonal Naive (lag 21) | 0.6330 | — |
| Random Forest | 0.4329 | 31.61% |
| **XGBoost** | **0.4110** | **35.07%** |

XGBoost obtuvo el mejor desempeño y fue seleccionado como modelo final.

---

## Backtesting

Para comprobar la estabilidad temporal del modelo se utilizaron cuatro ventanas consecutivas de 16 días.

Resultados promedio:

| Modelo | RMSLE promedio |
|---|---:|
| Seasonal Naive | 0.5595 |
| **XGBoost** | **0.4004** |

XGBoost superó al baseline en las cuatro ventanas evaluadas, alcanzando una mejora promedio aproximada del **28.13%**.

La desviación estándar del RMSLE de XGBoost entre ventanas fue aproximadamente **0.0092**, mostrando un desempeño relativamente estable.

---

## Análisis de errores

El desempeño fue analizado también a nivel de familia de productos.

XGBoost superó al baseline en **31 de las 33 familias**.

Los resultados muestran que el error no es homogéneo entre categorías, especialmente en familias con patrones de demanda más intermitentes.

---

## Evaluación externa

El modelo final fue utilizado para generar **28,512 predicciones** correspondientes al horizonte futuro de 16 días.

La predicción fue evaluada en Kaggle:

**Public Score: 0.50663 RMSLE**

La diferencia respecto del backtesting muestra la importancia de evaluar los modelos en períodos completamente futuros y de no interpretar las métricas históricas como garantía de desempeño futuro.

---

## Aplicación en Supply Chain

Un sistema de forecasting de este tipo puede utilizarse como input para procesos de:

- Demand Planning;
- planificación de inventarios;
- reposición;
- abastecimiento;
- planificación de compras;
- análisis de demanda por categoría y establecimiento.

El forecast representa una estimación de demanda y no reemplaza por sí solo un sistema de planificación de inventarios, que debería incorporar además variables como lead time, stock disponible, nivel de servicio y restricciones logísticas.

---

## Tecnologías

- Python
- Pandas
- NumPy
- Matplotlib
- Scikit-learn
- XGBoost
- Google Colab
- Git
- GitHub
- Kaggle

---

## Estructura del repositorio

```text
demand-forecasting-machine-learning/
│
├── data/
│   ├── raw/
│   └── processed/
│
├── notebooks/
│   └── 01_demand_forecasting.ipynb
│
├── models/
├── outputs/
│   ├── figures/
│   └── predictions/
│
├── src/
├── README.md
├── requirements.txt
└── .gitignore
