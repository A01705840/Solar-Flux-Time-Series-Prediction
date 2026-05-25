# TC3002B
## Avance 1: Generación o selección del set de datos y preprocesado

---

# Descripción del Proyecto

Este proyecto se enfoca en el análisis y preprocesamiento de datos del **10.7cm Solar Flux (F10.7)**, una medición de la actividad solar ampliamente utilizada en estudios de clima espacial, física solar y fenómenos atmosféricos terrestres.

El objetivo principal es preparar el dataset para tareas de **forecasting de series temporales**, utilizando técnicas de análisis temporal, extracción y detección de periodicidades solares.

Canada,. (2019). About the solar flux data. Spaceweather.Gc.Ca. https://www.spaceweather.gc.ca/forecast-prevision/solar-solaire/solarflux/sx-3-en.php
‌
---

# Key Concepts Implemented

## Lag Features
Características temporales generadas a partir de versiones desplazadas de la señal original. Estas permiten que el modelo aprenda dependencias temporales utilizando valores pasados del flujo solar.

Ejemplos implementados:
- Lag diario
- Lag semanal
- Lag mensual
- Lag anual
- Lag asociado a la rotación solar (~27 días)

---

## Solar Rotation Cycle
Periodicidad solar aproximada de 27 días causada por la rotación del Sol. Este patrón aparece frecuentemente en las mediciones del flujo solar debido a la reaparición de regiones activas solares.

---

## Time Series Forecasting
Técnica utilizada para predecir valores futuros del flujo solar a partir de observaciones históricas y patrones temporales.



# Prerequisites

Para ejecutar este proyecto se requieren las siguientes librerías de Python:

* `numpy`
* `pandas`
* `matplotlib`
* `seaborn`
* `sklearn.preprocessing`


Instalación rápida:

```bash
pip install numpy pandas matplotlib 
