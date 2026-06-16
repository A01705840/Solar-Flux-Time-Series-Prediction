# Solar Flux Forecasting usando LSTM Networks

---

# Descripción del Proyecto

Este proyecto se enfoca en el análisis y preprocesamiento de datos del **10.7cm Solar Flux (F10.7)**, una medición de la actividad solar ampliamente utilizada en estudios de clima espacial, física solar y fenómenos atmosféricos terrestres.

El objetivo principal es preparar el dataset para tareas de **forecasting de series temporales**, utilizando técnicas de análisis temporal, extracción y detección de periodicidades solares.


‌
---

# Preparación de Datos

Para entender los patrones dentro de los datos se implementaron los Lags:
## Lag Features
Características temporales generadas a partir de versiones desplazadas de la señal original. Estas permiten que el modelo aprenda dependencias temporales utilizando valores pasados del flujo solar.

Ejemplos implementados:
- Lag diario
- Lag semanal
- Lag mensual
- Lag anual
- Lag asociado a la rotación solar (~27 días)

Con la implementación de estos lags, permitió que se visualizara los datos más parecidos a sus datos pasados. En este caso, los lags diarios y lags de rotación solar, fueron más similares. Esto nos permite tomar una decisión en base al historial que recibirá el modelo para predecir el valor a futuro.

---

## Los Ciclos de Rotacion Solar
Periodicidad solar aproximada de 27 días causada por la rotación del Sol. Este patrón aparece frecuentemente en las mediciones del flujo solar debido a la reaparición de regiones activas solares.

---
# Modeling
# Implementación de Modelos

## Modelo 1: [Predicting the Daily 10.7-cm Solar Radio Flux Using the Long Short-Term Memory Method 2022]


### Descripción

El modelo utilizado fue una red neuronal Long Short-Term Memory (LSTM), diseñada para capturar dependencias temporales en secuencias de datos. Se utilizaron 50 neuronas y un learning rate de 0.001. En los resultados del modelo, se declaró que el forecast con mayor precisión fue de 1 dia. Al implementar este modelo, se implementó con la recomendación de predicción diaria. Al iniciar, se implementaron 100 epochs, resultando en un underfitting. Por lo que se incrementó a 300 epochs, razonando que como diariamente se hacian 3 registros del dato y para este modelo se utilizaba la media diaria, la cantidad de datos a procesar incrementó por data preparation y tiempo de realización del modelo.

---

### Arquitectura Implementada

| Parámetro | Valor |
|-----------|---------|
| Capas  | 1|
| Neuronas | 50 |
| Función de activación | relu |
| Optimizador | adam |
| Learning Rate |0.001 |
| Epochs | 300|
| Batch Size |32 |


### Ventajas 

- El modelo original toma en cuenta el ciclo
- Aprende patrones temporales complejos.
- Puede modelar ciclos solares de diferentes escalas.

---

## Modelo 2: [Deep Learning LSTM-based approaches for 10.7 cm solar radio flux forecasting up to 45-days]

### Descripción

El siguiente modelo implementa 3 capas de 24 neuronas, el cual se compone de un input layer, un hidden layer y un output layer. Estas capas permiten al modelo detectar patrones entre los datos dependiendo el forecast, esto es debido a la hidden layer o forget layer, que solo detecta los patrones más relevantes para la siguiente predicción.
En este modelo se escogío debido a su implementación de forecasting hasta 45 dias, incluyendo 27 dias (basandose en el ciclo solar). Durante su implementación, se experimentó con 1, 3 y 27 días y al igual que las conclusiones del artículo, las predicciones con historial diario resultaron más acertadas.

---

### Configuración Utilizada

| Parámetro | Valor |
|-----------|---------|
| Capas  | 3|
| Neuronas | 24 |
| Función de activación | tanh |
| Optimizador | adam |
| Learning Rate |0.001 |
| Epochs | 225|
| Batch Size |48 |


---

### Variables de Entrada

- La variable utilizada en ambos modelos es el fujo solar ajustado 'fluxadjflux', debido a que se utiliza como el valor estandarizado en el área. Además, a diferencia de ambos artículos científicos, se evaluó en base al flujo solar por hora y no con la media del día completo.

# Evaluación
Al iniciar la fase de evaluación, se implementó el uso de R² y función de loss. Estas métricas fueron implementadas, en base a un ejercicio de práctica con modelos de series de tiempo, pero con modelos CNN y RNN. Pero en base a la lectura de los artículos cientificos y su justificación de métricas, se añadieron métricas adicionales para evaluar el desempeño entre ambos modelos.

Para evaluar el desempeño de los modelos desarrollados se utilizaron tres métricas ampliamente empleadas en problemas de forecasting de series temporales: Mean Absolute Error (MAE), Root Mean Squared Error (RMSE) y el coeficiente de correlación (R) o coeficiente de determinación (R²).

---
# Métricas de Evaluación

## Mean Absolute Error (MAE)

### Fórmula

$$
MAE = \frac{1}{n}\sum_{i=1}^{n}|y_i-\hat{y}_i|
$$

### Justificación

El MAE mide la magnitud promedio del error absoluto entre los valores observados y los valores predichos por el modelo. Esta métrica permite interpretar directamente el error en las mismas unidades del flujo solar F10.7.

De acuerdo con Zhang, W (2022), el MAE es utilizado para cuantificar la desviación entre los valores observados y predichos, proporcionando una medida clara del desempeño predictivo del modelo. Un valor menor de MAE indica una mayor precisión en las predicciones.

Además, al no elevar los errores al cuadrado, el MAE es menos sensible a valores atípicos, lo que permite evaluar el comportamiento general del modelo sobre toda la serie temporal.

---

## Root Mean Squared Error (RMSE)

### Fórmula

$$
RMSE = \sqrt{\frac{1}{n}\sum_{i=1}^{n}(y_i-\hat{y}_i)^2}
$$

### Justificación

El RMSE mide la desviación promedio entre los valores observados y predichos, penalizando con mayor intensidad los errores grandes debido al término cuadrático.

Zhang, W (2022) identifica al RMSE como una de las métricas principales para evaluar la diferencia entre valores observados y estimados. Asimismo, Jerse & Marcucci (2024) emplea RMSE para comparar distintos modelos de predicción del flujo solar y analizar la estabilidad de las predicciones a diferentes horizontes temporales.

Esta métrica resulta especialmente relevante para el presente proyecto debido a que errores grandes en la predicción de actividad solar pueden tener un impacto significativo en aplicaciones relacionadas con clima espacial y sistemas de comunicación satelital. Un valor menor de RMSE indica un mejor desempeño del modelo.

---

## Coeficiente de Correlación (R) / Coeficiente de Determinación (R²)

### Fórmula (R²)

$$
R^2 = 1 - \frac{SS_{res}}{SS_{tot}}
$$

### Justificación

Zhang, W (2022) utiliza el coeficiente de correlación de Pearson (R) para medir el grado de relación lineal entre los valores observados y los valores predichos.

De manera similar, en este proyecto se utiliza el coeficiente de determinación (R²), una métrica derivada de la correlación que permite cuantificar qué proporción de la variabilidad de los datos observados es explicada por el modelo.

Mientras que MAE y RMSE evalúan el tamaño del error cometido, R² permite analizar qué tan bien el modelo reproduce el comportamiento general de la serie temporal. Valores cercanos a 1 indican una alta capacidad explicativa y una fuerte concordancia entre los valores reales y los predichos.

---

## Interpretación General

Siguiendo la metodología presentada en los artículos consultados, un modelo será considerado superior cuando:

- Presente valores menores de MAE.
- Presente valores menores de RMSE.
- Obtenga valores mayores de R o R².

Estas métricas permiten evaluar simultáneamente la magnitud de los errores y la capacidad del modelo para reproducir la dinámica temporal del flujo solar F10.7.

---

# Resultados Experimentales

## Desempeño del Modelo 1

| Métrica | Resultado Modelo 1| Resultado Modelo 2 |
|----------|------------|----------|
| MAE | 9.93| 12.86 |
| RMSE | 17.39| 21.79|
| R² | 0.8278 | 0.73 |

### Gráfica de Predicciones Modelo 1

(<img width="1238" height="547" alt="image" src="https://github.com/user-attachments/assets/f025909f-22f8-4369-b357-6593f5ecf8af" />


### Gráfica de Predicciones Modelo 2

(<img width="1238" height="547" alt="image" src="https://github.com/user-attachments/assets/d77a4c95-c5a7-4754-9d87-c75e49ae64ba" />
)



## Análisis Comparativo

### Precisión

El Modelo 1 obtuvo mejores resultados en todas las métricas de error evaluadas. Alcanzó un MAE de 9.93 y un RMSE de 17.39, mientras que el Modelo 2 presentó un MAE de 12.86 y un RMSE de 21.80. Esto indica que las predicciones generadas por el Modelo 1 fueron, en promedio, más cercanas a los valores reales del flujo solar F10.7.

Asimismo, el Modelo 1 obtuvo un MAPE de 5.13%, inferior al 6.33% registrado por el Modelo 2, lo que demuestra una menor desviación porcentual respecto a los valores observados.

### Capacidad para Capturar Patrones Temporales

El Modelo 1 alcanzó un coeficiente de determinación (R²) de 0.8278, superior al 0.7301 obtenido por el Modelo 2. Este resultado indica que el Modelo 1 logró explicar aproximadamente el 82.8% de la variabilidad presente en la serie temporal, capturando de forma más efectiva los patrones asociados a la actividad solar.

La diferencia observada en R² sugiere que el Modelo 1 fue más capaz de representar la dinámica temporal del flujo solar, incluyendo tendencias y comportamientos periódicos presentes en los datos históricos.

### Costo Computacional

El Modelo 1 requirió un mayor tiempo de entrenamiento debido a la complejidad de su arquitectura y al procesamiento de secuencias temporales. Sin embargo, este costo computacional adicional se vio reflejado en una mejora consistente en todas las métricas de evaluación.

Por otro lado, el Modelo 2 presentó un entrenamiento más rápido y una menor complejidad computacional, aunque con una reducción en la precisión de las predicciones.

### Robustez

Ambos modelos mostraron capacidad para generalizar sobre el conjunto de prueba. Sin embargo, el Modelo 1 presentó simultáneamente menores errores y una mayor capacidad explicativa, lo que indica una mejor adaptación a las características temporales de la serie analizada.

Los resultados sugieren que el Modelo 1 mantiene un equilibrio adecuado entre precisión y capacidad de representación de los datos, mientras que el Modelo 2 presenta un desempeño más limitado para capturar la variabilidad completa de la actividad solar.

---

# Selección del Modelo

## Modelo Seleccionado

**Modelo 1**

### Justificación

Después de comparar ambos modelos mediante las métricas MAE, RMSE, MAPE y R², se seleccionó el Modelo 1 como modelo final para este proyecto.

El Modelo 1 obtuvo el mejor desempeño en todas las métricas evaluadas. Presentó el menor error absoluto promedio (MAE = 9.93), el menor error cuadrático medio (RMSE = 17.39) y la menor desviación porcentual (MAPE = 5.13%). Además, alcanzó un coeficiente de determinación de 0.8278, superando al Modelo 2 y demostrando una mayor capacidad para explicar la variabilidad de la serie temporal.

Estos resultados indican que el Modelo 1 no solo genera predicciones más precisas, sino que también representa de forma más efectiva los patrones temporales presentes en el flujo solar F10.7.

Por lo tanto, considerando tanto la precisión predictiva como la capacidad para modelar el comportamiento de la actividad solar, el Modelo 1 fue seleccionado como la alternativa más adecuada para las siguientes etapas del proyecto.

<img width="855" height="547" alt="image" src="https://github.com/user-attachments/assets/dffb7277-1cb2-41ed-aaeb-b2e39f4f4e56" />

---

# Mejora a Modelo Final

Obteniendo una arquitectura para el modelo, se hicieron varios intentos de mejora. 

## Intento 1
Al visualizar la curva de aprendizaje del modelo 1 con un desempeño de R²≈ 0.85, se intentó implementar la función de activación de tanh, esta decisión se hizo en base a la justificación de uso del modelo 2, ya que tanh es una función común en modelos LSTM. 

## Resultados de Intento 1
Al entrenar el modelo, las métricas empeoraron a  R²≈ 0.80. En su mejor entrenamiento, se logró R²≈ 0.85, lo cual no incrementó su precisión. 
<img width="1238" height="547" alt="image" src="https://github.com/user-attachments/assets/65b150d4-0c7e-47a5-8040-6ff420b85f40" />

## Intento 2
Después se intentó la implementación de más epochs hast 600 epochs, lo cual tampoco resultó a cambios positivos y solo creo overfitting en el modelo

## Resultados de Intento 2
Al entrenar el modelo, las métricas empeoraron a  R²≈ 0.80. En su mejor entrenamiento, se logró R²≈ 0.85, lo cual no incrementó su precisión
<img width="1238" height="547" alt="image" src="https://github.com/user-attachments/assets/4cf49f44-7cf4-454c-a91f-c4e16a75af00" />



## Intento 3
Por último se implementó early stopping con 100 epochs. 
## Resultados de Intento 3
Este modelo, fue el más preciso y se entrenó entre las 120 y 130 epochs. 
<img width="1238" height="547" alt="image" src="https://github.com/user-attachments/assets/522bea30-4fa4-4082-a719-5596c642eff9" />

| Métrica | Resultado Modelo 1| Resultado Modelo 2 | Resultado Modelo FINAL |
|----------|------------|----------| ----------|
| MAE | 9.93| 12.86 | 6.09 |
| RMSE | 17.39| 21.79| 11.30 |
| R² | 0.83 | 0.73 | 0.93 |

# Predicción Final del Modelo
Timestamp: 17:00:00
  Actual Fluxadjflux: 135.4000
  Predicted Fluxadjflux: 139.7660
  Absolute Error: 4.3660
  Squared Error: 19.0618

Timestamp: 20:00:00
  Actual Fluxadjflux: 135.6000
  Predicted Fluxadjflux: 135.4461
  Absolute Error: 0.1539
  Squared Error: 0.0237

Timestamp: 23:00:00
  Actual Fluxadjflux: 136.5000
  Predicted Fluxadjflux: 136.0847
  Absolute Error: 0.4153
  Squared Error: 0.1725
<img width="1490" height="590" alt="image" src="https://github.com/user-attachments/assets/b7b1be00-f69b-48d0-8def-45dcd91d08c5" />

# Conclusiones Generales
El proyecto logró desarrollar un modelo capaz de predecir el flujo solar F10.7 con un coeficiente de determinación de R² = 0.92, obteniendo un desempeño competitivo en comparación con los trabajos analizados del estado del arte. Aunque no se logró superar la precisión reportada en los artículos científicos utilizados como referencia, es importante considerar que dichos estudios realizan sus predicciones utilizando la media diaria del flujo solar, mientras que el modelo desarrollado en este proyecto trabaja con las tres mediciones diarias disponibles, lo que representa un problema de predicción más detallado y con mayor variabilidad en los datos.

Asimismo, la combinación de un adecuado proceso de preparación de datos, la incorporación de características temporales relevantes y la optimización del entrenamiento mediante técnicas como Early Stopping permitió obtener un modelo robusto y con una alta capacidad de generalización. Los resultados obtenidos demuestran el potencial de las redes LSTM para modelar la actividad solar y respaldar aplicaciones relacionadas con el monitoreo del clima espacial, las comunicaciones satelitales y otros sistemas sensibles a las variaciones de la actividad solar.

# A mejorar...
Como trabajo futuro, una posible mejora del modelo consiste en incorporar la información de los horarios exactos en los que se realizan las mediciones del flujo solar. Durante el análisis se observó que, dependiendo de la época del año, las tres mediciones diarias no siempre se realizan a las mismas horas, lo que introduce variaciones temporales que actualmente no son consideradas de forma explícita por el modelo.

La inclusión de esta información como variable de entrada podría permitir una representación más precisa de las condiciones bajo las cuales se registran los datos, mejorando la capacidad predictiva del modelo. Asimismo, contar con acceso anticipado a los horarios programados de medición permitiría generar predicciones más precisas para el mismo día en que se realizan las observaciones, incrementando el valor práctico del sistema para aplicaciones de monitoreo de actividad solar y clima espacial.
# Referencias

## Modelo 1

[Zhang, W., Zhao, X., Feng, X., Liu, C., Xiang, N., Li, Z., & Lu, W. (2022). Predicting the Daily 10.7-cm Solar Radio Flux Using the Long Short-Term Memory Method. Universe, 8(1), 30. https://doi.org/10.3390/universe8010030]

---

## Modelo 2

[G. Jerse, & Marcucci, A. (2024). Deep Learning LSTM-based approaches for 10.7 cm solar radio flux forecasting up to 45-days. Astronomy and Computing, 100786–100786. https://doi.org/10.1016/j.ascom.2024.100786]

---
## Dataset
Canada,. (2019). About the solar flux data. Spaceweather.Gc.Ca. https://www.spaceweather.gc.ca/forecast-prevision/solar-solaire/solarflux/sx-3-en.php

## Google Colab Final: 
https://drive.google.com/file/d/1qXCicjbVbAcF3Aq8u8VcoLCbhjs1FCfJ/view?usp=sharing (Compartido en caso de que los widgets del jupyter notebook no permitan ver el código en github)
