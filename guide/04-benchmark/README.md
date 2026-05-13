# 4. Benchmark the model

Navigate to **Benchmarks → Run Benchmark** (`/project/{id}/benchmarks`).

## Required fields

- **Dataset** — must include qrels for retrieval metrics to be computed.
- **Model** — the embedding or reranker model to evaluate.

## Embedding-model options

- **Device** — Auto / CPU / CUDA
- **Batch Size** — default 32
- **Top-K** — optional
- **Host model on vLLM server** — when ticked, the model runs as a vLLM API
  server for inference.

## Reranker options

In addition to the above, rerankers have:

- **First-Stage Retrieval Results** *(optional)* — pick a previous **embedding**
  benchmark result on the same dataset. The reranker then re-orders that
  candidate set. If left blank, the reranker scores **all** documents from the
  qrels — much slower, useful only for small datasets.

Set the resource block (same as fine-tune) and click **Run Benchmark**.

## Viewing results

- **Benchmarks → Results** (`/project/{id}/benchmarks/results`) — per-run
  metrics (recall@k, nDCG, etc.).
- **Benchmarks → Compare Models** (`/project/{id}/benchmarks/compare`) — side
  by side comparison across runs on the same dataset.

---

Next: [Generating a synthetic dataset](../05-synthetic-dataset/README.md)
