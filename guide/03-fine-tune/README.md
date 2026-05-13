# 3. Fine-tune an embedding model

Navigate to **Models → Fine-tune** (`/project/{id}/models/finetune`).

## Required fields

- **Dataset** — pick one from [§2](../02-dataset-upload/README.md).
- **Base Model** — either a HuggingFace model name (e.g.
  `sentence-transformers/all-MiniLM-L6-v2`), or tick **"Use a model from this
  project"** to pick a previously fine-tuned model stored in the project.
- **Model Class** — `Embedding` (use this for normal dense retrievers).
  `Reranker` is also available; `Sparse` is disabled.
- **Output Model Name** — what to call the resulting model.

## Primary training parameters

| Field | Default | Notes |
|---|---|---|
| Epochs | 3 | |
| Batch Size (Effective Samples) | 16 | |
| Learning Rate | 2e-5 | |
| Max Sequence Length | 512 | Tick **Auto** to derive from corpus/query p95 token stats |

## Advanced options (collapsible)

- **Advanced Training** — gradient accumulation steps, optimizer, FP16/BF16/TF32.
- **LoRA Fine-Tuning** — toggle **Use LoRA**, then set `r`, `alpha`, `dropout`,
  and target modules (default `q_proj,v_proj`).
- **Matryoshka** — toggle on, then list dimensions (default
  `768,512,256,128,64`).
- **Loss Function** — default `CachedMultipleNegativesRankingLoss`; mini batch
  size can be set to **Auto**.
- **Hard Negative Training** — see
  [§5.2](../06-two-phase-training/README.md).

## Resource configuration

At the bottom of the form:

- **Accelerators** (default `RTX4090:1`)
- **Memory** (default `16+`)
- **CPUs** (default `4+`)
- **Retry Until Up** (default on)

Click **Fine-tune Model**. You'll see a success toast with the Job ID; jump to
**Jobs** (`/project/{id}/jobs`) to watch real-time logs (streamed via SSE).
When the job finishes, the new model appears under **Models → Model List**.

---

Next: [Benchmark the model](../04-benchmark/README.md)
