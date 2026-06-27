# TIC-2026

**Vision Transformer-Based Postharvest Grading of Chaucha and Chola Potato Varieties**

_Universidad Nacional de Loja (UNL), Facultad de la Energía, las Industrias y los Recursos Naturales No Renovables, Carrera de Computación._

## Descripción

Este repositorio contiene el manuscrito, los notebooks, las figuras y la documentación asociada a la investigación experimental sobre clasificación automática de tubérculos de papa en etapa de poscosecha.

El estudio se centra en las variedades **chaucha** (_Solanum phureja_) y **chola** (_Solanum tuberosum_), clasificadas en dos categorías:

- **Buen estado:** tubérculos sin defectos visibles relevantes.
- **Defectuoso:** tubérculos con cortes, brotes y signos de pudrición.

El modelo propuesto se basa en **Vision Transformer (ViT-Base/16)**, preentrenado en ImageNet-21k y ajustado sobre ImageNet-1K. La adaptación al dominio de tubérculos de papa se realizó mediante dos estrategias:

1. **Transfer Learning (TL):** congelamiento de las capas extractoras de características y reentrenamiento de la cabeza de clasificación.
2. **Fine-Tuning parcial (FTP):** congelamiento de las seis primeras capas del encoder y reentrenamiento de las últimas seis capas restantes junto con la cabeza de clasificación.

Además, se emplearon técnicas de aumento de datos, normalización basada en ImageNet y análisis de interpretabilidad mediante **Eigen-CAM** con `pytorch-grad-cam`.

## Objetivo

El objetivo del proyecto es reducir la subjetividad y el error humano en la clasificación poscosecha de variedades andinas de papa, mediante un modelo de aprendizaje profundo basado en los Transformadores de Visión (ViT), capaz de identificar tubérculos en buen estado y tubérculos defectuosos.

## Resultados principales

La evaluación final se realizó sobre un subconjunto de prueba de **7 200 imágenes**, balanceado en dos clases.

| Técnica                   | Accuracy |    MCC |   Loss |
| ------------------------- | -------: | -----: | -----: |
| Transfer Learning (TL)    |   0.9600 | 0.9202 | 0.1353 |
| Fine-Tuning parcial (FTP) |   0.9999 | 0.9997 | 0.0015 |

**Resumen de matrices de confusión:**

- **Transfer Learning:** 3 420 de 3 600 imágenes de la clase buen estado fueron clasificadas correctamente; 3 492 de 3 600 imágenes defectuosas fueron clasificadas correctamente.
- **Fine-Tuning parcial:** el modelo clasificó correctamente casi todas las muestras del conjunto de prueba, con un único falso negativo.

El análisis con Eigen-CAM mostró que las regiones de mayor activación del modelo coinciden con zonas visualmente relevantes, como cortes, brotes y áreas de pudrición, en lugar de enfocarse principalmente en el fondo de la imagen.

## Estructura del repositorio

```text
TIC-2026/
├── README.md
├── manuscript/
│   └── manuscript.pdf
├── code/
│   ├── prepro/
│   │   └── preprocesamiento.ipynb
│   └── exp-*/
│       └── ViT_calidad_papas.ipynb
├── figures/
│   ├── README.md
│   ├── comparative_test_tl_ftp.png
│   ├── confusion_matrices_tl_ftp.png
│   ├── test_metrics_chart_tl_ftp.png
│   ├── results_tl/
│   └── results_ftp/
└── data/
    └── README.md
```

## Contenido del repositorio

### `manuscript/`

Contiene el manuscrito en formato PDF. El documento describe la motivación, metodología, experimentos, resultados y conclusiones del estudio.

### `code/`

Contiene los notebooks utilizados para el preprocesamiento, entrenamiento, evaluación e interpretabilidad del modelo.

| Ruta                                 | Descripción                                                                          |
| ------------------------------------ | ------------------------------------------------------------------------------------ |
| `code/prepro/preprocesamiento.ipynb` | Curación, organización y preprocesamiento del conjunto de datos.                     |
| `code/exp-*/ViT_calidad_papas.ipynb` | Notebooks de entrenamiento y evaluación para distintos experimentos con ViT-Base/16. |

Los notebooks incluyen el cálculo de métricas como accuracy, precision, recall, F1-score, MCC, pérdida y matrices de confusión. También generan visualizaciones de interpretabilidad mediante Eigen-CAM.

### `figures/`

Contiene las figuras del manuscrito y los resultados visuales de los experimentos. Incluye:

- Figuras comparativas entre TL y FTP.
- Curvas de accuracy, precision, recall, F1-score, MCC y loss.
- Resultados finales sobre el subconjunto de prueba.
- Mapas de calor Eigen-CAM por variedad, clase y técnica de ajuste.

La descripción detallada de cada archivo se encuentra en [figures/README.md](figures/README.md).

### `data/`

Contiene la documentación del conjunto de datos utilizado en el proyecto. Los datos no se almacenan directamente en este repositorio; están disponibles en Zenodo y Hugging Face.

La descripción completa del conjunto de datos, sus particiones, fuentes públicas y enlaces de descarga se encuentra en [data/README.md](data/README.md).

## Conjunto de datos

El conjunto de datos híbrido contiene **36 000 imágenes** balanceadas:

| Clase       | Número de imágenes |
| ----------- | -----------------: |
| Buen estado |             18 000 |
| Defectuoso  |             18 000 |

Las imágenes provienen de fotografías locales y repositorios públicos. La partición utilizada fue:

| Subconjunto   | Porcentaje | Número de imágenes |
| ------------- | ---------: | -----------------: |
| Entrenamiento |       64 % |             23 040 |
| Validación    |       16 % |              5 760 |
| Prueba        |       20 % |              7 200 |

Recursos principales:

- **Dataset en Zenodo:** [Hybrid Potato Tuber Dataset](https://doi.org/10.5281/zenodo.20616991)
- **Datasets en Hugging Face:** [Potato Tuber Datasets](https://huggingface.co/datasets/Carlos012/dataset_papas)

## Modelos entrenados

Los modelos resultantes de las estrategias **Transfer Learning (TL)** y **Fine-Tuning parcial (FTP)** están disponibles en:

- **Zenodo:** [TL and PFT Models](https://doi.org/10.5281/zenodo.20649945).
- **Hugging Face:** [Fine‑Tuned Models](https://huggingface.co/Carlos012/vit_papas).

## Requisitos

El entrenamiento original se realizó en **Google Colab**, con **PyTorch 2.10** y una GPU **NVIDIA Tesla T4**.

Dependencias principales:

```text
torch
torchvision
timm
scikit-learn
pandas
numpy
matplotlib
pytorch-grad-cam
huggingface_hub
datasets
```

Para ejecutar los notebooks en un entorno local:

```bash
python -m venv venv
source venv/bin/activate
pip install torch torchvision timm scikit-learn pandas numpy matplotlib pytorch-grad-cam huggingface_hub datasets
jupyter notebook
```

En Windows, la activación del entorno virtual puede realizarse con:

```bash
venv\Scripts\activate
```

## Reproducción de los experimentos

Flujo recomendado:

1. Descargar el conjunto de datos desde Zenodo o Hugging Face.
2. Ejecutar `code/prepro/preprocesamiento.ipynb` para revisar y preparar las imágenes.
3. Ejecutar el notebook experimental correspondiente en `code/exp-*/ViT_calidad_papas.ipynb`.
4. Evaluar el modelo sobre los subconjuntos de validación y prueba.
5. Generar las métricas, matrices de confusión, curvas de entrenamiento y mapas Eigen-CAM.
6. Guardar las figuras resultantes en `figures/results_tl/`, `figures/results_ftp/` o en la raíz de `figures/`, según corresponda.

En Google Colab se recomienda configurar `NUM_WORKERS = 0` para evitar problemas de carga paralela de datos.

## Figuras y resultados visuales

Las figuras principales disponibles en este repositorio incluyen:

| Archivo o carpeta                       | Descripción                                          |
| --------------------------------------- | ---------------------------------------------------- |
| `figures/comparative_test_tl_ftp.png`   | Comparación global entre TL y FTP.                   |
| `figures/confusion_matrices_tl_ftp.png` | Matrices de confusión comparativas.                  |
| `figures/test_metrics_chart_tl_ftp.png` | Métricas finales de prueba para ambas técnicas.      |
| `figures/results_tl/`                   | Curvas, métricas y Eigen-CAM de Transfer Learning.   |
| `figures/results_ftp/`                  | Curvas, métricas y Eigen-CAM de Fine-Tuning parcial. |

## Fuentes públicas integradas

El conjunto de datos híbrido incorpora imágenes de los siguientes recursos:

- Mafi, M. M. H. M. et al. _Potato Disease Recognition Dataset_. Mendeley Data, 2023. DOI: `10.17632/pmbc875pr7.1`.
- Mridha, M. H.; Mridha, N. S. _Healthy Potato Image Dataset_. Mendeley Data, 2023. DOI: `10.17632/5m38z6jthb.1`.
- Islam, S.; Afrin, T. _PotatoCare: Deep learning based potato disease dataset_. Mendeley Data, 2025. DOI: `10.17632/7vm7xskfg4.2`.

## Citación

Si utiliza este repositorio, el conjunto de datos o los modelos entrenados, cite el manuscrito del proyecto y el recurso correspondiente en Zenodo.

```bibtex
@article{ticpapas2026,
  title   = {Vision Transformer-Based Postharvest Grading of Chaucha and Chola Potato Varieties},
  author  = {Armijos-Sarango, Cristian and Chamba-Eras, Luis},
  year    = 2026,
  month   = jun,
  doi     = {},
  note    = {Version June 11, 2026 submitted to Journal Not Specified}
}
```

```bibtex
@dataset{armijos_sarango_2026_20616991,
  author       = {Armijos-Sarango, Cristian},
  title        = {Hybrid Potato Tuber Dataset (Solanum tuberosum,
                   Solanum phureja)
                  },
  month        = jun,
  year         = 2026,
  publisher    = {Zenodo},
  version      = 1,
  doi          = {10.5281/zenodo.20616991},
  url          = {https://doi.org/10.5281/zenodo.20616991},
}
```

## Licencia
© 2026 Cristian Armijos
El contenido de este repositorio se distribuye bajo la licencia [Creative Commons Attribution 4.0 International License](https://creativecommons.org/licenses/by/4.0/).

Se permite el uso, distribución y reproducción del material en cualquier medio, siempre que se cite adecuadamente el trabajo original y se respeten las licencias de las fuentes públicas integradas.

## Declaración sobre uso de IA generativa

Los autores declaran haber utilizado herramientas de IA generativa como apoyo en la redacción, organización, formateo, extracción de información y validación bibliográfica. La interpretación de resultados, la metodología experimental y las conclusiones son responsabilidad exclusiva de los autores.

## Contacto

**Autor:** Armijos-Sarango, Cristian. Universidad Nacional de Loja (UNL), Facultad de la Energía, las Industrias y los Recursos Naturales No Renovables, Carrera de Computación. `cristian.e.armijos@unl.edu.ec`

**Coautor:** Chamba-Eras, Luis. Universidad Nacional de Loja (UNL), Facultad de la Energía, las Industrias y los Recursos Naturales No Renovables, Carrera de Computación. `lachamba@unl.edu.ec`
