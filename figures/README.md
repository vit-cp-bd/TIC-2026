# Figures

Este directorio contiene las figuras generadas para el manuscrito **"Vision Transformer-Based Postharvest Grading of Chaucha and Chola Potato Varieties"**. Las imágenes documentan el comportamiento del modelo ViT-Base/16 durante la evaluación de tubérculos de papa en dos clases: **buen estado** y **defectuoso**.

Las figuras se presentan en formato PNG y corresponden a las dos estrategias de ajuste aplicadas al modelo:

- **TL (Transfer Learning):** reentrenamiento de la cabeza de clasificación manteniendo congeladas las capas extractoras de características.
- **FTP (Fine-Tuning parcial):** reentrenamiento parcial del encoder y de la cabeza de clasificación, manteniendo congeladas las primeras seis capas del modelo.

## Estructura del directorio

```text
figures/
├── comparative_test_tl_ftp.png
├── confusion_matrices_tl_ftp.png
├── test_metrics_chart_tl_ftp.png
├── results_ftp/
│   ├── accuracy_curves_ftp.png
│   ├── f1_curves_ftp.png
│   ├── loss_curves_ftp.png
│   ├── mcc_curves_ftp.png
│   ├── precision_curves_ftp.png
│   ├── recall_curves_ftp.png
│   ├── test_ftp.png
│   ├── test_metrics_chart_ftp.png
│   └── eigen-*-ftp.png
└── results_tl/
    ├── accuracy_curves_tl.png
    ├── f1_curves_tl.png
    ├── loss_curves_tl.png
    ├── mcc_curves_tl.png
    ├── precision_curves_tl.png
    ├── recall_curves_tl.png
    ├── test_tl.png
    ├── test_metrics_chart_tl.png
    └── eigen-*-tl.png
```

## Figuras comparativas

Los archivos ubicados directamente en `figures/` resumen la comparación entre las dos estrategias de ajuste:

| Archivo | Descripción |
| --- | --- |
| `comparative_test_tl_ftp.png` | comparación general de TL y FTP en el conjunto de prueba. Resume el desempeño de ambas técnicas. |
| `confusion_matrices_tl_ftp.png` | Matrices de confusión comparativas para TL y FTP sobre el subconjunto de prueba. Permite revisar aciertos y errores por clase. |
| `test_metrics_chart_tl_ftp.png` | Gráfico comparativo de métricas de prueba, como accuracy, precision, recall, F1-score y MCC. |

Estas figuras son útiles para la discusión principal del manuscrito, porque permiten contrastar de forma directa el rendimiento del modelo con transferencia de aprendizaje y con ajuste fino parcial.

## Resultados por técnica

### `results_tl/`

Contiene las figuras asociadas a la fase de **Transfer Learning**. En esta etapa se evalua el desempeño del modelo cuando se ajusta principalmente la cabeza de clasificación.

Archivos incluidos:

| Archivo | Contenido |
| --- | --- |
| `accuracy_curves_tl.png` | Evolución de la exactitud durante entrenamiento, validación y prueba. |
| `precision_curves_tl.png` | Evolución de la precisión del modelo por época. |
| `recall_curves_tl.png` | Evolución de la sensibilidad o recall por época. |
| `f1_curves_tl.png` | Evolución del F1-score, que integra precision y recall. |
| `mcc_curves_tl.png` | Evolución del coeficiente de correlación de Matthews, útil en clasificación binaria. |
| `loss_curves_tl.png` | Evolución de la función de pérdida durante el entrenamiento y la evaluación. |
| `test_tl.png` | Resumen visual de la evaluación final de TL en el conjunto de prueba. |
| `test_metrics_chart_tl.png` | Gráfico de métricas finales de prueba para TL. |
| `eigen-*-tl.png` | Mapas de activación Eigen-CAM obtenidos con el modelo TL. |

### `results_ftp/`

Contiene las figuras asociadas a la fase de **Fine-Tuning parcial**. En esta etapa se descongela las seis últimas capas del encoder del ViT-Base/16 para ajustar representaciones más específicas al dominio de tubérculos de papa.

Archivos incluidos:

| Archivo | Contenido |
| --- | --- |
| `accuracy_curves_ftp.png` | Evolución de la exactitud durante entrenamiento, validación y prueba. |
| `precision_curves_ftp.png` | Evolución de la precisión del modelo por época. |
| `recall_curves_ftp.png` | Evolución de la sensibilidad o recall por época. |
| `f1_curves_ftp.png` | Evolución del F1-score para el modelo con ajuste fino parcial. |
| `mcc_curves_ftp.png` | Evolución del coeficiente de correlación de Matthews. |
| `loss_curves_ftp.png` | Evolución de la pérdida durante entrenamiento, validación y prueba. |
| `test_ftp.png` | Resumen visual de la evaluación final de FTP en el conjunto de prueba. |
| `test_metrics_chart_ftp.png` | Gráfico de métricas finales de prueba para FTP. |
| `eigen-*-ftp.png` | Mapas de activación Eigen-CAM obtenidos con el modelo FTP. |

## Convención de nombres

Los nombres de archivo siguen una estructura descriptiva:

- `*_tl.png`: resultado generado con la estrategia **Transfer Learning**.
- `*_ftp.png`: resultado generado con la estrategia **Fine-Tuning parcial**.
- `*_tl_ftp.png`: figura comparativa que integra resultados de ambas estrategias.
- `accuracy_curves_*`: curvas de exactitud por época.
- `precision_curves_*`: curvas de precisión por época.
- `recall_curves_*`: curvas de sensibilidad por época.
- `f1_curves_*`: curvas de F1-score por época.
- `mcc_curves_*`: curvas del coeficiente MCC por época.
- `loss_curves_*`: curvas de pérdida por época.
- `test_*`: resumen de resultados finales en el conjunto de prueba.
- `test_metrics_chart_*`: visualización de métricas finales de prueba.
- `eigen-*`: visualizaciones de interpretabilidad generadas con Eigen-CAM.

## Convención de archivos Eigen-CAM

Los archivos `eigen-*` representan mapas de calor superpuestos sobre imágenes de prueba. Estos mapas muestran las regiones de la imagen que más influyeron en la decisión del modelo.

La nomenclatura usada es:

```text
eigen-{variedad}-{clase}-{técnica}.png
```

Donde:

| Componente | Valores | Significado |
| --- | --- | --- |
| `{variedad}` | `cha`, `cho` | Variedad de papa: chaucha o chola. |
| `{clase}` | `be`, `bro`, `cor`, `pod` | Categoría o tipo visual analizado. |
| `{técnica}` | `tl`, `ftp` | Estrategia de ajuste del modelo. |

Interpretación de abreviaturas:

- `cha`: papa chaucha.
- `cho`: papa chola.
- `be`: buen estado.
- `bro`: brotes.
- `cor`: cortes.
- `pod`: pudrición.

Por ejemplo, `eigen-cha-pod-ftp.png` corresponde a una visualización Eigen-CAM para papa chaucha con signos de pudrición, generada con el modelo ajustado mediante Fine-Tuning parcial.

## Relación con el manuscrito

Estas figuras respaldan principalmente:

- La comparación de rendimiento entre TL y FTP.
- La evaluación cuantitativa del modelo en el conjunto de prueba.
- La interpretación visual de las predicciones mediante Eigen-CAM.
- La discusión sobre la capacidad del modelo para enfocar regiones relevantes como cortes, brotes y zonas de pudrición.

Las figuras comparativas de la raíz del directorio son las mas adecuadas para presentar resultados globales. Las figuras dentro de `results_tl/` y `results_ftp/` sirven como evidencia detallada por técnica.

## Reproducibilidad

Las figuras fueron generadas a partir de los notebooks de entrenamiento y evaluación disponibles en `code/`. En particular, los notebooks `ViT_calidad_papas.ipynb` contienen las procedimientos de entrenamiento, cálculo de métricas, generación de curvas, matrices de confusión y visualizaciones Eigen-CAM.

Para reproducir o actualizar las figuras:

1. Ejecutar el preprocesamiento del dataset desde `code/prepro/preprocesamiento.ipynb`.
2. Ejecutar el notebook de entrenamiento/evaluación correspondiente en `code/exp-*/ViT_calidad_papas.ipynb`.
3. Exportar las visualizaciones en formato PNG.
4. Guardar los resultados en `results_tl/`, `results_ftp/` o en la raíz de `figures/` según corresponda.

## Recursos relacionados

- Dataset del proyecto en Zenodo: [Hybrid Potato Tuber Dataset](https://doi.org/10.5281/zenodo.20616991)
- Datasets versionados en Hugging Face: [Potato Tuber Datasets](https://huggingface.co/datasets/Carlos012/dataset_papas)
- Modelos y métricas en Hugging Face: [Fine‑Tuned Models](https://huggingface.co/Carlos012/vit_papas)