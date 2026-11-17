# Recuperación de información

Actividades del curso **Recuperación de información 2026-2** (MCDI · INFOTEC), autor: David Segundo García.

| Actividad | Carpeta | Contenido |
|---|---|---|
| **2A** · Motor de búsqueda léxico y BM25 | [`Entrega_2A/`](Entrega_2A/) | Preprocesamiento, índice invertido y ranking BM25 implementados desde cero sobre un corpus propio de 120 documentos; análisis de aciertos y del límite de la búsqueda exacta. |
| **3A** · Índices de vecinos más cercanos aproximados | [`Entrega_3B/`](Entrega_3B/) | LSH, IVF-PQ (FAISS) y HNSW (hnswlib) frente a búsqueda exhaustiva sobre GloVe 6B (10k y 100k vectores): barridos de hiperparámetros, recall@10 / tiempo / memoria, punto de operación, escalabilidad e interfaz con ipywidgets. |
| **4A** · Buscador semántico híbrido y *re-ranking* con Transformers | [`Entrega_4A/`](Entrega_4A/) | Mismo corpus de la 2A codificado con dos *bi-encoders* (MiniLM 384 d, E5 768 d), Top-20 por coseno en HNSW, *re-ranking* con dos *cross-encoders* (mMARCO, BGE), fusión híbrida RRF, 20 consultas con juicios graduados (nDCG@5, MRR, recall@5), latencia por etapa y tablero comparativo con ipywidgets. |
| **6A** · Agrupamiento, tópicos y visualización | [`Entrega_6A/`](Entrega_6A/) | Mismo corpus sin etiquetas: 4 representaciones densas (MiniLM y E5 de la 4A, E5-`query:`, mpnet), K-Means / Ward / UMAP+HDBSCAN con silueta coseno, estabilidad y regla de selección; tópicos por c-TF-IDF (NPMI, diversidad); UMAP 2D en dos configuraciones (trustworthiness, Spearman) e interfaz con ipywidgets + plotly; auditoría externa (ARI/NMI). |

Cada carpeta contiene el notebook ejecutado, el reporte en PDF, su fuente LaTeX y un `requirements.txt`.

## Setup

Requiere Python ≥ 3.10.

```bash
python -m venv .venv && source .venv/bin/activate
pip install -r Entrega_6A/requirements.txt          # o Entrega_4A/ · Entrega_3B/ · Entrega_2A/
jupyter lab
```

No se necesitan variables de entorno ni credenciales.

## Ejecución (runbook)

- **2A:** abrir `Entrega_2A/motor_busqueda_bm25.ipynb` y ejecutar todas las celdas. El corpus está en `corpus/corpus.json`.
- **3A:** abrir `Entrega_3B/Actividad3A_Indices_ANN.ipynb` y ejecutar todas las celdas. La primera ejecución descarga GloVe 6B
  (~822 MB comprimido) a `Entrega_3B/data/`, carpeta excluida del repositorio. El barrido completo tarda ~1 min en un solo hilo y
  regenera `results/*.csv` y `figures/*.png`.
- **4A:** abrir `Entrega_4A/Actividad4A_Buscador_Semantico.ipynb` y ejecutar todas las celdas (o
  `jupyter nbconvert --to notebook --execute --inplace Actividad4A_Buscador_Semantico.ipynb` desde `Entrega_4A/`).
  La primera ejecución descarga de Hugging Face cuatro modelos (~4 GB) a la caché local (`~/.cache/huggingface`);
  después no necesita red. Tarda ~5 min en CPU, casi todo en medir la latencia del *re-ranker* BGE, y regenera
  `results/*.csv` y `figures/*.png`.
- **6A:** abrir `Entrega_6A/Actividad6A_Agrupamiento_Topicos.ipynb` y ejecutar todas las celdas (o
  `jupyter nbconvert --to notebook --execute --inplace Actividad6A_Agrupamiento_Topicos.ipynb` desde `Entrega_6A/`).
  Usa tres modelos de Hugging Face (MiniLM y E5 de la 4A, más `paraphrase-multilingual-mpnet-base-v2`); tarda ~1 min en CPU
  y regenera `results/` (incluidos los *embeddings* `.npy`) y `figures/` (incluido el mapa interactivo `mapa_corpus.html`).
  La interfaz con *widgets* necesita un kernel activo; el notebook guarda además una versión estática del mapa.
- **Reportes:** dentro de cada carpeta, `latexmk -pdf reporte.tex` (en 3A y 4A: `latexmk -pdf -jobname=Reporte_Actividad3A reporte.tex`
  `latexmk -pdf -jobname=Reporte_Actividad4A reporte.tex` y `latexmk -pdf -jobname=Reporte_Actividad6A reporte.tex`).

## Datos

- GloVe 6B (Pennington, Socher y Manning, 2014), dominio público: <https://nlp.stanford.edu/projects/glove/>.
- Modelos de la 4A (Hugging Face): `sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2`, `intfloat/multilingual-e5-base`,
  `cross-encoder/mmarco-mMiniLMv2-L12-H384-v1` y `BAAI/bge-reranker-v2-m3`.
- Modelo adicional de la 6A: `sentence-transformers/paraphrase-multilingual-mpnet-base-v2`.
