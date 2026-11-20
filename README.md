# karting-telemetry-ml
Proyecto académico de Machine Learning aplicado a telemetría real de karting. Ingeniería de características por curva y evaluación de modelos con separación por tandas.
# Machine Learning aplicado a telemetría de karting

Proyecto final integrador de la materia **Aprendizaje de Máquina** de Ingeniería en Inteligencia Artificial, Universidad de Palermo.

El trabajo explora si es posible identificar pasadas por curva con mayor pérdida de tiempo a partir de datos reales de telemetría, y analizar qué variables se asocian con esas diferencias.

## Contexto

Estoy desarrollando **MetriKart**, una plataforma de análisis de telemetría para karting de competición.

Este proyecto académico fue una oportunidad para explorar un enfoque de Machine Learning y estudiar qué información podría aportar al análisis basado en reglas que vengo desarrollando.

## Datos

- 21 tandas registradas con data loggers AiM MyChron y exportadas desde Race Studio 3.
- Tres pilotos anonimizados.
- Circuito de Villaguay, Entre Ríos.
- Telemetría a 20 Hz.
- 13 tramos de análisis asociados a las curvas del circuito.

Después del filtrado quedaron **17 tandas, 73 vueltas y 949 pasadas por curva**.

No se contaba con sensores de freno ni acelerador. La frenada y la reaceleración se estimaron a partir de la aceleración longitudinal.

## Preparación y variables

El trabajo incluye:

- Exploración de aceleración lateral y tiempos de vuelta para identificar tandas compatibles con pista mojada.
- Filtrado de vueltas lentas o no representativas.
- Construcción de referencias por piloto y curva a partir de sus tres mejores pasadas.
- Cálculo de diferencias respecto de esas referencias.

Las variables incluyen velocidad de entrada, aceleración lateral, punto y duración de frenada, posición de velocidad mínima y punto de reaceleración.

La etiqueta es relativa: identifica pasadas cuya pérdida respecto de la referencia supera la mediana de su curva. No representa un diagnóstico absoluto de manejo.

## Modelos y evaluación

Se compararon:

- Árbol de decisión.
- Árbol de decisión con poda.
- Random Forest.
- Gradient Boosting.

Se reservaron cuatro tandas completas para prueba. El ajuste del Random Forest se realizó con `GridSearchCV` y validación cruzada mediante `GroupKFold`, agrupando por tanda.

## Resultados exploratorios

El Random Forest ajustado obtuvo:

| Métrica | Resultado aproximado |
| --- | --- |
| Accuracy en prueba | 72 % |
| Baseline de clase mayoritaria en prueba | 59 % |
| Recall de la clase con mayor pérdida | 68 % |
| Precision de la clase con mayor pérdida | 65 % |
| Accuracy en entrenamiento | 80,5 % |

La velocidad de entrada y la aceleración lateral fueron las variables con mayor importancia dentro del modelo.

Estas importancias describen cómo el modelo utiliza las variables: no demuestran causas de pérdida de tiempo ni porcentajes de tiempo atribuibles a cada factor.

## Limitaciones y mejoras pendientes

- Dataset pequeño, de un solo circuito y tres pilotos.
- Las tandas de prueba corresponden a pilotos también presentes en entrenamiento.
- Sin medición directa de freno y acelerador.
- Precisión GPS insuficiente para estudiar algunas diferencias de trayectoria.
- Referencias y umbrales calculados antes de la separación de los datos.
- Uso del conjunto de prueba para comparar modelos.

Por estos motivos, los resultados deben interpretarse como exploratorios.

Una próxima versión deberá calcular las referencias y umbrales únicamente con los datos de entrenamiento, repetir ese procedimiento dentro de cada fold y reservar un conjunto final independiente de las decisiones de modelado.

También se propone incorporar más sesiones, otros circuitos y mejores mediciones.

## Herramientas

Python, Jupyter Notebook, Pandas, NumPy, Matplotlib y Scikit-learn.

## Disponibilidad de los datos

Los datos originales de los pilotos no se publican por privacidad.

El notebook se incorporará después de revisar su contenido y sus salidas. Sin el dataset, será posible consultar el procedimiento y los resultados guardados, pero no reproducir la ejecución completa.
