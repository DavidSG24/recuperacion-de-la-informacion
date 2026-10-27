# Recuperación de información

Actividades del curso **Recuperación de información 2026-2** (MCDI · INFOTEC), autor: David Segundo García.

| Actividad | Carpeta | Contenido |
|---|---|---|
| **2A** · Motor de búsqueda léxico y BM25 | [`Entrega_2A/`](Entrega_2A/) | Preprocesamiento, índice invertido y ranking BM25 implementados desde cero sobre un corpus propio de 120 documentos; análisis de aciertos y del límite de la búsqueda exacta. |
| **3A** · Índices de vecinos más cercanos aproximados | [`Entrega_3B/`](Entrega_3B/) | LSH, IVF-PQ (FAISS) y HNSW (hnswlib) frente a búsqueda exhaustiva sobre GloVe 6B (10k y 100k vectores): barridos de hiperparámetros, recall@10 / tiempo / memoria, punto de operación, escalabilidad e interfaz con ipywidgets. |

Cada carpeta contiene el notebook ejecutado, el reporte en PDF, su fuente LaTeX y un `requirements.txt`.

## Setup

Requiere Python ≥ 3.10.

```bash
python -m venv .venv && source .venv/bin/activate
pip install -r Entrega_3B/requirements.txt          # o Entrega_2A/requirements.txt
jupyter lab
```

No se necesitan variables de entorno ni credenciales.

## Ejecución (runbook)

- **2A:** abrir `Entrega_2A/motor_busqueda_bm25.ipynb` y ejecutar todas las celdas. El corpus está en `corpus/corpus.json`.
- **3A:** abrir `Entrega_3B/Actividad3A_Indices_ANN.ipynb` y ejecutar todas las celdas. La primera ejecución descarga GloVe 6B
  (~822 MB comprimido) a `Entrega_3B/data/`, carpeta excluida del repositorio. El barrido completo tarda ~1 min en un solo hilo y
  regenera `results/*.csv` y `figures/*.png`.
- **Reportes:** dentro de cada carpeta, `latexmk -pdf reporte.tex` (en 3A: `latexmk -pdf -jobname=Reporte_Actividad3A reporte.tex`).

## Datos

- GloVe 6B (Pennington, Socher y Manning, 2014), dominio público: <https://nlp.stanford.edu/projects/glove/>.
