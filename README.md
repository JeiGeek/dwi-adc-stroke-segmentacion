# Project Title

Breve descripción del proyecto.

---

# Overview

Descripción general del problema:
- Segmentación binaria de lesiones de ACV isquémico.
- Uso del dataset ISLES.
- Modalidades MRI utilizadas: DWI y ADC.
- Enfoque basado en slices 2D independientes.
- Objetivo principal del proyecto.

---

# Dataset

## ISLES Dataset

Descripción breve del dataset:
- Tipo de imágenes.
- Modalidades utilizadas.
- Ground truth.
- Organización de datos.

## Preprocessing

Pasos de preprocesamiento:
- Normalización.
- Redimensionamiento.
- Conversión a slices 2D.
- Creación de dual-channel input (DWI + ADC).
- Data augmentation (si aplica).

---

# Methodology

## Input Representation

Explicar:
- Entrada de 2 canales.
- Forma del tensor.
- Uso de slices independientes.

## Model Architecture

Descripción del modelo:
- U-Net / DeepLabV3+ / Attention U-Net / etc.
- Encoder utilizado.
- Número de canales.
- Output binario.

## Loss Function

Ejemplo:
- Binary Crossentropy
- Dice Loss
- BCE + Dice

## Metrics

Métricas reportadas:
- Dice Score
- IoU
- Sensitivity
- Specificity
- Accuracy

---

# Training Configuration

Parámetros de entrenamiento:
- Optimizer
- Learning rate
- Batch size
- Epochs
- Hardware utilizado

---

# Results

## Quantitative Results

Tabla de métricas.

| Metric | Value |
|---|---|
| Dice | |
| IoU | |
| Sensitivity | |
| Specificity | |

## Qualitative Results

Agregar imágenes:
- Input DWI
- Input ADC
- Ground truth
- Prediction

---

# Project Structure

```bash
project/
│
├── data/
├── notebooks/
├── models/
├── training/
├── evaluation/
├── utils/
├── outputs/
├── README.md
└── requirements.txt
