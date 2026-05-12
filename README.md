# 🧠 Segmentación de Lesiones Isquémicas mediante DWI y ADC 🧠

![Portada/Banner Placeholder](https://github.com/JeiGeek/dwi-adc-stroke-segmentacion/blob/main/src/banner.jpg)

Integrantes:
- Jeison Fernando Guarguati Anaya
- Alejandro
- Julian

---

# Descripción del Proyecto

Proyecto enfocado en la segmentación automática de lesiones causadas por accidente cerebrovascular (ACV) isquémico utilizando modelos de Deep Learning aplicados sobre imágenes de resonancia magnética cerebral (MRI).

El objetivo principal es detectar regiones lesionadas de manera automática mediante modelos de segmentación semántica utilizando información multimodal proveniente de imágenes DWI y ADC.

# Overview

El accidente cerebrovascular isquémico es una de las principales causas de mortalidad y discapacidad neurológica. La identificación temprana de las lesiones cerebrales es fundamental para apoyar el diagnóstico clínico y acelerar la toma de decisiones médicas.

En este proyecto se aborda el problema de segmentación binaria de lesiones isquémicas utilizando el dataset ISLES y modelos de segmentación profunda.

Características principales del enfoque:

- Segmentación binaria de lesiones de ACV isquémico.
- Uso del dataset ISLES.
- Modalidades MRI utilizadas:
  - DWI (Diffusion Weighted Imaging)
  - ADC (Apparent Diffusion Coefficient)
- Procesamiento basado en slices 2D independientes.
- Entrada dual-channel combinando DWI y ADC.
- Comparación entre arquitecturas U-Net y DeepLabV3+.
- Evaluación de diferentes funciones de pérdida para manejar el desbalance entre fondo y lesión.

---

# Dataset

## ISLES Dataset

Dataset utilizado:
- ISLES (Ischemic Stroke Lesion Segmentation)

Enlace:
- https://www.kaggle.com/datasets/orvile/isles-2022-brain-stoke-dataset

Descripción general:
- Dataset médico de resonancia magnética cerebral (MRI).
- Diseñado para tareas de segmentación de lesiones isquémicas.
- Incluye máscaras de segmentación manual realizadas por expertos clínicos.

Modalidades utilizadas:
- DWI
- ADC

Ground Truth:
- Máscaras binarias de lesión.
- Valor `1` representa lesión.
- Valor `0` representa fondo.

Organización de datos:
- División en entrenamiento, validación y prueba.
- Conversión de volúmenes 3D a slices 2D independientes.

---

# Metodología

## Preprocessing

Se aplicaron las siguientes etapas de preprocesamiento:

- Normalización de intensidades en rango `[0,1]`.
- Redimensionamiento de imágenes a `128×128`.
- Conversión de volúmenes MRI 3D a slices 2D.
- Creación de entrada dual-channel:
  - Canal 1 - DWI
  - Canal 2 - ADC
- Eliminación de máscaras de 1 píxel consideradas ruido.
- División train / validation / test.

Shape de entrada:

```python
(128, 128, 2)
```

## Representación de la entrada

- Se utilizan 2 canales de entrada correspondientes a imágenes de resonancia magnética: DWI y ADC.
- Cada muestra tiene una forma de tensor de **(128, 128, 2)**.
- El modelo trabaja con *slices* 2D independientes, es decir, cada corte del volumen se procesa como una imagen individual sin información 3D explícita.

## Arquitectura del modelo

- Se emplean arquitecturas de segmentación como **U-Net** y **DeepLabV3+**.
- En U-Net se utiliza un encoder con bloques convolucionales dobles y *downsampling* progresivo.
- En DeepLabV3+ se utiliza **MobileNetV2 como backbone** junto con un módulo ASPP.
- La entrada del modelo tiene 2 canales (DWI + ADC).
- La salida es una máscara binaria con activación sigmoide (1 canal).

## Función de pérdida

Se utilizan diferentes funciones de pérdida según el experimento:

- Entropía cruzada binaria (Binary Crossentropy)
- Dice Loss
- Combinación de Binary Crossentropy + Dice Loss

## Métricas de evaluación

- Dice Score
- IoU (Intersection over Union)
- Sensibilidad (Recall)
- Especificidad
- Precisión

---

# Configuración de entrenamiento

- Optimizador: Adam  
- Tasa de aprendizaje: 0.0001
- Batch size: 16  
- Épocas: hasta 50 con Early Stopping  
- Hardware: GPU (entorno tipo Google Colab)

---

# Resultados

## Resultados cuantitativos

## Comparación de modelos
| Modelo                          | Loss                  | Dice   | IoU    | Sensibilidad | Especificidad | Precisión |
|--------------------------------|----------------------|--------|--------|--------------|---------------|-----------|
| U-Net                          | Combined Loss        | 0.8802 | 0.7860 | 0.8679       | 0.9995        | 0.8928    |
| U-Net                          | Binary Crossentropy  | 0.8786 | 0.7835 | 0.8605       | 0.9995        | 0.8974    |
| DeepLabV3+                     | Combined Loss        | 0.7621 | 0.6157 | 0.6993       | 0.9994        | 0.8373    |
## Resultados cualitativos

Se comparan visualmente:

- Imagen DWI  
- Imagen ADC  
- Máscara real (ground truth)  
- Predicción del modelo  

El modelo muestra mejor desempeño en lesiones medianas y grandes, mientras que las lesiones pequeñas siguen siendo las más difíciles de segmentar.
