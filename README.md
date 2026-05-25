# Proyecto Primer Parcial — Redes Neuronales y Aprendizaje Profundo  
## Grupo 1 — RNN Elman para Modelado de Lenguaje  

**Integrantes:** Alvarez Roberto · Carlos Jeancarlos · Intriago Jorge · Menoscal Julleysi · Remache Steven  

**Paper asignado:** *Revisiting Simple Neural Probabilistic Language Models* — Sun & Iyyer, ACL 2021 · [arXiv:2104.03474](https://arxiv.org/abs/2104.03474)  

**Dataset:** WikiText-2 (~2M tokens, ~66K vocabulario)  

**Arquitectura:** RNN Elman - 2 capas implementada desde cero con BPTT  

---

## Estructura del repositorio

```text
├── notebook_grupo1_rnn_optimizado.ipynb        # Notebook principal con toda la implementación
├── informe_grupo1_rnn.pdf                      # Informe académico
├── README.md                                   # Este archivo
└── docs/
    └── notebook_grupo1_rnn_base.ipynb          # Versión anterior del notebook (referencia)
```

---

## Contenido del notebook

| Sección | Descripción |
|---------|-------------|
| 1 | Resumen del paper, ecuaciones, métrica perplexity |
| 2 | Descarga y EDA del dataset WikiText-2 |
| 3 | Implementación RNN Elman optimizada desde cero |
| 4 | Baseline n-grama + Entrenamiento con BPTT |
| 5 | Análisis crítico y vanishing gradient |
| 6 | Conclusiones |

---

## Entorno y librerías

Proyecto desarrollado en Google Colab.

Principales dependencias:
- torch 2.2.2
- torchtext 0.17.2
- torchdata 0.7.1
- numpy
- matplotlib
- tqdm
---
## Ejecución 

> ⚠️ **Este notebook fue desarrollado y ejecutado en Google Colab. Se recomienda abrirlo y ejecutarlo exclusivamente en ese entorno. (GPU T4)**  

> La instalación de dependencias, la descarga del dataset y el parche de compatibilidad MD5 están configurados para funcionar en Colab. No se garantiza el correcto funcionamiento en entornos locales sin ajustes adicionales.

---- > [![Abrir en Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1iFVUIYSlcAwgm6M3BRtPiOaIFLwEgEPe?usp=sharing)

> El notebook descarga y prepara automáticamente WikiText-2 mediante `torchtext.datasets.WikiText2` junto con un parche de compatibilidad para torchtext 0.17.2.

---

## Resultados

| Modelo | Val PPL | Test PPL |
|--------|---------|----------|
| Bigrama (baseline) | 10,825.5 | 10,867.7 |
| RNN Elman base | 546.3 | 588.8 |
| **RNN Elman optimizada** | **264.5** | **267.6** |
| NPLM (Sun & Iyyer, 2021) | 120.5 | 114.3 |
| Transformer (paper) | 117.6 | 111.1 |

El modelo optimizado reduce la perplexity un **54%** respecto a la versión base, gracias a las siguientes mejoras:

- RNN Elman de 2 capas con inicialización ortogonal en W_hh
- Weight Tying (embedding ↔ decoder)
- Activation Regularization (AR=0.3) y Temporal AR (TAR=0.15)
- Dropout p=0.4 entre capas
- Adam lr=3e-4 con weight_decay=1e-5 y gradient clipping=0.25
- LR decay automático (factor=0.5, patience=5)

---

## Limitaciones

- Vanishing gradient: la celda Elman no puede capturar dependencias a más de ~10–20 tokens de distancia.
- Procesamiento secuencial: no paralelizable como un Transformer.
- Capacidad de contexto limitada: todo el historial se comprime en un único vector oculto.
- La mejora propuesta es reemplazar la celda Elman por LSTM, que alcanza PPL ~65–85 en WikiText-2.

---

## Declaración de uso de IA

Partes de este informe y del notebook fueron desarrolladas con asistencia de herramientas de IA, conforme a las normas del proyecto que permiten el uso de IA siempre que se declare explícitamente.
