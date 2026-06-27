# Data

Este directorio documenta los conjuntos de datos utilizados en el proyecto **"Vision Transformer-Based Postharvest Grading of Chaucha and Chola Potato Varieties"**. El objetivo del conjunto de datos es apoyar la clasificación automática de tubérculos de papa en etapa de poscosecha, diferenciando entre las categorías **buen estado** y **defectuoso**.

Los datos no se almacenan directamente en este repositorio debido a su tamaño. En su lugar, se proporcionan enlaces a los repositorios externos donde se encuentran las versiones completas y reutilizables.

## Conjunto de datos principal

El conjunto de datos híbrido está disponible en Zenodo:

- **Zenodo:** [Hybrid Potato Tuber Dataset](https://doi.org/10.5281/zenodo.20616991)
- **DOI:** `10.5281/zenodo.20616991`

Este conjunto integra imágenes de tubérculos de papa obtenidas desde dos tipos de fuentes:

- **Fotografías locales:** imágenes capturadas en un entorno controlado para representar condiciones reales de poscosecha.
- **Repositorios públicos:** imágenes provenientes de conjuntos de datos abiertos relacionados con enfermedades, defectos y estado visual de la papa.

Cada imagen cuenta con una etiqueta asociada a su categoría, lo que permite utilizar el conjunto de datos en tareas supervisadas de clasificación binaria.

## Clases de clasificación

Las imágenes se organizan en dos clases principales:

| Clase | Descripción |
| --- | --- |
| `buen_estado` | Tubérculos sin defectos visibles relevantes para la clasificación poscosecha. |
| `defectuoso` | Tubérculos con cortes, brotes o signos de pudrición visibles. |

## Variedades consideradas

El estudio se centra en dos variedades de papa de importancia agrícola:

- **Chaucha** (_Solanum phureja_).
- **Chola** (_Solanum tuberosum_).

Estas variedades fueron consideradas para evaluar la capacidad del modelo de generalizar entre diferencias visuales propias del tubérculo, como forma, textura, coloración y presencia de defectos.

## Composición del conjunto de datos

La versión utilizada en el proyecto contiene:

| Característica | Valor |
| --- | --- |
| Tamaño total | 36 000 imágenes |
| Imágenes en buen estado | 18 000 |
| Imágenes defectuosas | 18 000 |
| Imágenes locales | 32 292 |
| Imágenes de repositorios públicos | 3 708 |
| Formato de imagen | Archivos de imagen procesados para clasificación |
| Tamaño usado por el modelo | 224 × 224 píxeles |
| Tipo de tarea | Clasificación binaria supervisada |

El balance entre clases permite entrenar y evaluar el modelo sin favorecer a una categoría sobre la otra.

## Particiones utilizadas

Para el entrenamiento y la evaluación del modelo se empleó la siguiente partición:

| Subconjunto | Porcentaje | Número de imágenes |
| --- | ---: | ---: |
| Entrenamiento | 64 % | 23 040 |
| Validación | 16 % | 5 760 |
| Prueba | 20 % | 7 200 |

El subconjunto de prueba se reservó para la evaluación final del modelo y no se utilizó durante el ajuste de hiperparámetros.

## Fuentes públicas integradas

El conjunto de datos híbrido incorpora imágenes provenientes de los siguientes recursos públicos:

- Mafi, M. M. H. M. et al. _Potato Disease Recognition Dataset_. Mendeley Data, 2023. DOI: `10.17632/pmbc875pr7.1`.
- Mridha, M. H.; Mridha, N. S. _Healthy Potato Image Dataset_. Mendeley Data, 2023. DOI: `10.17632/5m38z6jthb.1`.
- Islam, S.; Afrin, T. _PotatoCare: Deep learning based potato disease dataset_. Mendeley Data, 2025. DOI: `10.17632/7vm7xskfg4.2`.

Estas fuentes fueron integradas con fotografías locales para aumentar la diversidad visual del conjunto de datos.

## Versiones en Hugging Face

Los conjuntos de datos completos y versionados también están disponibles en Hugging Face:

- **Datasets del proyecto:** [Datasets de tubérculos de pap](https://huggingface.co/datasets/Carlos012/dataset_papas)

Se recomienda usar las versiones publicadas en Hugging Face cuando se necesite integrar el conjunto de datos directamente en flujos de trabajo con `datasets`, PyTorch o notebooks de Google Colab.

## Preprocesamiento aplicado

Antes del entrenamiento, las imágenes fueron curadas y preprocesadas para mantener consistencia visual y técnica. El flujo general incluye:

1. Revisión y descarte de imágenes duplicadas, desenfocadas o con iluminación extrema.
2. Organización en directorios por clase.
3. Estandarización de nombres de archivo.
4. Redimensionamiento a `224 × 224` píxeles.
5. Normalización con la media y desviación estándar de ImageNet.
6. Aplicación de técnicas de aumento de datos durante el entrenamiento.

El notebook asociado al preprocesamiento se encuentra en:

```text
code/prepro/preprocesamiento.ipynb
```

## Uso recomendado

Este conjunto de datos puede utilizarse para:

- Entrenar modelos de clasificación binaria de imágenes.
- Comparar técnicas de transferencia de aprendizaje y ajuste fino.
- Evaluar arquitecturas basadas en Vision Transformers o redes neuronales convolucionales.
- Generar métricas de desempeño como accuracy, precision, recall, F1-score y MCC.
- Analizar interpretabilidad visual mediante técnicas como Eigen-CAM.

Para reproducir los experimentos del proyecto, se recomienda utilizar los notebooks disponibles en `code/exp-*/ViT_calidad_papas.ipynb`.

## Modelos entrenados relacionados

Los datasets utilizados para entrenar, validar y evaluar los modelos ajustados durante las fases de **Transfer Learning (TL)** y **Fine-Tuning parcial (FTP)** están disponibles en:

- **Zenodo:** [Hybrid Potato Tuber Dataset](https://doi.org/10.5281/zenodo.20616991).
- **Zenodo:** [TL and PFT Models](https://doi.org/10.5281/zenodo.20649945).
- **Hugging Face:** [Fine‑Tuned Models](https://huggingface.co/Carlos012/vit_papas).
- **Hugging Faces:** [Potato Tuber Datasets](https://huggingface.co/datasets/Carlos012/dataset_papas).

Estos modelos fueron entrenados con los conjuntos de datos documentado en este directorio (Zenodo y Hugging Face).

## Citación sugerida

Si utiliza este conjunto de datos o los modelos asociados, cite el recurso de Zenodo y el manuscrito del proyecto:

```bibtex
@dataset{dataset_potatoes_2026,
  title     = {Hybrid Potato Tuber Dataset (Solanum tuberosum, Solanum phureja)},
  author    = {Armijos-Sarango, Cristian},
  year      = 2026,
  month     = jun,
  publisher = {Zenodo},
  version   = 1,
  doi       = {10.5281/zenodo.20616991},
  url       = {https://doi.org/10.5281/zenodo.20616991}
}
```

```bibtex
@misc{pytorch_model_2026,
  title        = {PyTorch Model Files (.pt): Transfer Learning and
                   Partial Fine-Tuning for Potato Tuber
                   Classification
                  },
  author       = {Armijos-Sarango, Cristian},
  month        = jun,
  year         = 2026,
  publisher    = {Zenodo},
  version      = 1,
  doi          = {10.5281/zenodo.20649945},
  url          = {https://doi.org/10.5281/zenodo.20649945},
}
```
