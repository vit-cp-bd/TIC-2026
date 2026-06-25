# TIC-2026

**Vision Transformer-Based Postharvest Grading of Chaucha and Chola Potato Varieties**

_Universidad Nacional de Loja (UNL) — Facultad de la Energía, las Industrias y los Recursos Naturales No Renovables, Carrera de Computación._

## Descripción

Esta investigación experimental desarrolla un modelo de clasificación automática de tubérculos de papa de las variedades **chaucha** (_Solanum phureja_) y **chola** (_Solanum tuberosum_) en la fase de poscosecha, en las categorías **"buen estado"** y **"defectuoso"**.

El modelo se basa en la arquitectura **Vision Transformer (ViT-Base/16)**, preentrenada en ImageNet-21k y ajustada sobre ImageNet-1K, y fue adaptada mediante dos fases de ajuste siguiendo la metodología **CRISP-ML(Q)**:

1. **Transfer Learning (TL):** congelamiento de las capas iniciales (extractoras de características) y reentrenamiento de la cabeza de clasificación.
2. **Fine-Tuning parcial (FTP):** congelamiento de las 6 primeras capas del encoder y reentrenamiento de las capas restantes junto con la cabeza de clasificación.

Adicionalmente, se incorporaron técnicas de _data augmentation_ (rotaciones, escalado, variaciones de brillo) y _random erasing_ para mejorar la generalización, y se evaluó la interpretabilidad del modelo mediante **Eigen-CAM** (`pytorch-grad-cam`).

El objetivo es reducir la subjetividad y el error humano en la clasificación poscosecha de variedades andinas, donde las arquitecturas CNN presentan limitaciones por su sesgo inductivo local.

---

## Resultados principales

Evaluación sobre el subconjunto de prueba (_n_ = 7 200 imágenes):

| Técnica             | Accuracy | MCC    | Loss   |
| ------------------- | -------- | ------ | ------ |
| Transfer Learning   | 0.9604   | 0.9209 | 0.1346 |
| Fine-Tuning parcial | 0.9999   | 0.9997 | 0.0007 |

**Matrices de confusión (n = 3 600 por clase):**

- _Transfer Learning:_ 3 427/3 600 "buen estado" correctas, 3 488/3 600 "defectuoso" correctas (173 FP, 112 FN).
- _Fine-Tuning parcial:_ clasificación correcta en la totalidad de las muestras, con un único falso negativo.

El análisis Eigen-CAM confirmó que las regiones de mayor activación coinciden con las áreas de defecto real (pudrición, cortes, brotes), diferenciándolas del fondo.

---

## Estructura del repositorio

```
TIC-2026/
├── manuscript/     # Documento pdf del manuscrito.
├── code/           # Notebooks de preprocesamiento y entrenamiento.
├── figures/        # Figuras del manuscrito (metodología, dataset, arquitectura, métricas, Eigen-CAM).
└── data/           # Referencias al dataset híbrido y modelos entrenados.
```

---

## 1. `manuscript/`

Manuscrito en pdf siguiendo el formato de la revista International Journal of Experimental Botany.

---

## 2. `code/`

### Requisitos

El modelo se entrenó en **Google Colab** con **PyTorch 2.10** sobre una GPU **NVIDIA Tesla T4**.

```bash
python -m venv venv
source venv/bin/activate   # En Windows: venv\Scripts\activate
pip install -r requirements.txt
```

Dependencias principales utilizadas en el proyecto:

```
torch==2.10
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

### a) Preprocesamiento — `code/preprocesamiento.ipynb`

Notebook de curación y organización del dataset híbrido: etiquetado, organización en directorios por clase con nomenclatura estandarizada, descarte de muestras duplicadas/desenfocadas/con iluminación extrema, redimensionamiento a 224×224 px y normalización con media (0.485, 0.456, 0.406) y desviación estándar (0.229, 0.224, 0.225) de ImageNet.

```bash
jupyter notebook code/preprocesamiento.ipynb
```

### b) Entrenamiento y evaluación — `code/ViT_calidad_papas.ipynb`

Notebook principal. Implementa el modelo `ClasificadorPapasViT` sobre ViT-Base/16 con las fases TL y FTP descritas arriba, evaluación de métricas (Accuracy, Precision, Recall, F1-Score, MCC, matriz de confusión) e interpretabilidad mediante Eigen-CAM.

**Hiperparámetros utilizados:**

| Parámetro       | Transfer Learning      | Fine-Tuning parcial    |
| --------------- | ---------------------- | ---------------------- |
| Learning rate   | 1×10⁻⁵                 | 1×10⁻⁶                 |
| Weight decay    | 1×10⁻²                 | 1×10⁻⁴                 |
| Batch size      | 32                     | 32                     |
| Épocas          | 30                     | 20                     |
| Optimizador     | AdamW (β = 0.9, 0.999) | AdamW (β = 0.9, 0.999) |
| Warmup fraction | 0.5                    | 0.5                    |
| Precisión       | Mixed precision        | Mixed precision        |

**Ejecución:**

```bash
jupyter notebook code/ViT_calidad_papas.ipynb
```

O en Google Colab (entorno usado originalmente):

- Subir el notebook a Colab.
- Configurar `NUM_WORKERS = 0` (requerido en Colab).
- Cargar el dataset desde HuggingFace Hub (ver sección `data/`).

**Salidas generadas:**

- Historial de entrenamiento (CSV) con métricas por época.
- Curvas de pérdida (train/val/test) por fase.
- Matrices de confusión y métricas (Accuracy, Precision, Recall, F1-Score, MCC) sobre el subconjunto de prueba.
- Mapas de calor Eigen-CAM superpuestos sobre imágenes de prueba.

---

## 3. `figures/`

Figuras del manuscrito, generadas por los notebooks o disponibles en repositorios externos:

| Figura | Descripción                                                                   | Fuente                                                   |
| ------ | ----------------------------------------------------------------------------- | -------------------------------------------------------- |
| Fig. 1 | Flujo metodológico CRISP-ML(Q)                                                | [Figshare](https://doi.org/10.6084/m9.figshare.32395359) |
| Fig. 2 | Muestras representativas del dataset (chaucha/chola × buen estado/defectuoso) | generada localmente                                      |
| Fig. 3 | Arquitectura ViT adaptada a clasificación binaria                             | generada localmente                                      |
| Fig. 4 | Curvas de pérdida (TL y FTP)                                                  | generada por `ViT_calidad_papas.ipynb`                   |
| Fig. 5 | Matrices de confusión (TL y FTP)                                              | generada por `ViT_calidad_papas.ipynb`                   |
| Fig. 6 | Mapas de calor Eigen-CAM                                                      | [Figshare](https://doi.org/10.6084/m9.figshare.32643123) |

---

## 4. `data/`

### Dataset híbrido

- **Tamaño total:** 36 000 imágenes balanceadas (18 000 "buen estado" / 18 000 "defectuoso"), de las cuales 32 292 son muestras locales y 3 708 provienen de repositorios públicos.
- **Particiones:** 23 040 entrenamiento (64%) / 5 760 validación (16%) / 7 200 prueba (20%).
- **Repositorio del dataset híbrido (Zenodo):** [10.5281/zenodo.20616991](https://doi.org/10.5281/zenodo.20616991)
- **Repositorio de los datasets utilizados (Hugging Face Hub):** [Dataset de tubérculos de papas](https://huggingface.co/datasets/Carlos012/dataset_papas)

### Fuentes públicas integradas en el dataset híbrido

- Mafi MMHM et al. _Potato Disease Recognition Dataset_. Mendeley Data, 2023. doi:10.17632/pmbc875pr7.1
- Mridha MH, Mridha NS. _Healthy Potato Image Dataset_. Mendeley Data, 2023. doi:10.17632/5m38z6jthb.1
- Islam S, Afrin T. _PotatoCare: Deep learning based potato disease dataset_. Mendeley Data, 2025. doi:10.17632/7vm7xskfg4.2

### Modelos entrenados

Modelos resultantes de las fases de TL y FTP, empaquetados (`.pt`):

- **DOI (Zenodo):** [10.5281/zenodo.20649945](https://doi.org/10.5281/zenodo.20649945)
- **Hugging Face Hub:** [Modelos ViT ajustados](https://huggingface.co/Carlos012/vit_papas)

---

## Citación

```bibtex
@article{ticpapas2026,
  title   = {Vision Transformer-Based Postharvest Grading of Chaucha and Chola Potato Varieties},
  author  = {Armijos-Sarango, Cristian and Chamba-Eras, Luis},
  year    = {2026},
  month   = {6},
  doi     = {10.32604/journal.2026.000000},
  note    = {Version June 11, 2026 submitted to Journal Not Specified}
}
```

---

## Licencia

Todo el contenido de este repositorio (manuscrito, código, figuras y datos) se distribuye bajo licencia **Creative Commons Attribution 4.0 International (CC BY 4.0)**, según se indica en el manuscrito ("© 2026 by the Authors. [CC BY 4.0] Creative Commons Attribution 4.0 International License").

Esto significa que se permite el uso, distribución y reproducción en cualquier medio, siempre que se cite apropiadamente el trabajo original.

---

## Disponibilidad de datos

Parte de los datos públicos utilizados están disponibles en los repositorios Mendeley listados arriba (_Potato Disease Recognition Dataset_, _Healthy Potato Image Dataset_, _PotatoCare: Deep learning based potato disease dataset_). El resto de los datos generados durante el estudio están disponibles a solicitud razonable al autor de correspondencia.

---

## Declaración sobre uso de IA generativa

Los autores declaran haber utilizado herramientas de IA generativa (ChatGPT, Gemini, Copilot, NotebookLM y Claude) como apoyo en la redacción, organización y formateo del manuscrito, así como en la extracción/análisis de información y validación bibliográfica. La interpretación de resultados y las conclusiones son responsabilidad exclusiva de los autores.

---

## Contacto

Autor: `Armijos-Sarango, Cristian` -- Universidad Nacional de Loja (UNL), Facultad de la Energía, las Industrias y los Recursos Naturales No Renovables, Carrera de Computación. `cristian.e.armijos@unl.edu.ec`

Coautor: `Chamba-Eras, Luis` -- Universidad Nacional de Loja (UNL), Facultad de la Energía, las Industrias y los Recursos Naturales No Renovables, Carrera de Computación. `lachamba@unl.edu.ec`.
