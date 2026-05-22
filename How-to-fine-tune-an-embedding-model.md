# How to fine-tune an embedding model

## 1. Introduction

Fine-tuning adapts a base embedding model to your own data, so it retrieves
better on your domain than any off-the-shelf model. You give Tuner a dataset
and a base model; it trains and produces a new model that appears in your
**Model List**, ready to benchmark.

> **Tip — do you have enough data?** Fine-tuning needs a substantial corpus to
> beat a good off-the-shelf model. As a rough guide, aim for at least 10,000
> unique chunks after deduplication — the more, the better. With a smaller
> corpus you are usually better off
> [choosing a strong existing model](How-to-choose-your-embedding-model)
> instead.

**Before you start:** assemble a dataset on the **IR Datasets** page. If you do
not have a labelled dataset, see
[How to synthesize a dataset](How-to-synthesize-a-dataset).

## 2. Start a fine-tune job

Navigate to **Models → Fine-tune** (`/project/{id}/models/finetune`).

1. Choose your **Dataset**.
2. Set the **Base Model** — a HuggingFace model identifier (for example,
   `sentence-transformers/all-MiniLM-L6-v2`). To fine-tune a model already in
   this project instead, tick **Local model** and pick it from the dropdown.
3. Set **Model Class** to **Embedding**.
4. Give the result an **Output Model Name**.
5. Adjust the training settings if needed — the defaults are a good starting
   point.
6. Pick a GPU under **Accelerator (GPU)**.
7. Click **Start Fine-tuning**.

The core fields:

| Field | Required | Default | Notes |
|---|---|---|---|
| **Dataset** | Yes | — | The dataset to train on |
| **Base Model** | Yes | — | A HuggingFace model ID, or a project model via **Local model** |
| **Model Class** | Yes | — | **Embedding** |
| **Output Model Name** | Yes | — | Name for the resulting model |
| **Epochs** | No | `3` | Passes over the dataset |
| **Batch Size (Effective Samples)** | No | `16` | Samples per training step |
| **Learning Rate** | No | `0.00002` | How fast the model updates |
| **Max Sequence Length** | No | `512` | Tick **Auto** to resolve it from your data |
| **Accelerator (GPU)** | Yes | RTX4090 | Pick the GPU in the **Select a GPU** dialog |
| **Retry until up** | No | On | Keep retrying if the chosen GPU is busy |

The form also has a collapsed **Advanced Options** section — optimizer, mixed
precision (FP16 / BF16 / TF32), gradient checkpointing, **LoRA**
parameter-efficient fine-tuning, and **Matryoshka** multi-dimensional
embeddings. The defaults work well; leave it collapsed unless you have a
specific reason to change it. For a deeper walkthrough, open the **Embedding
Fine-Tuning Guide** button on the form.

<img src="assets/how-to-fine-tune-an-embedding-model/fine-tune-form.png" alt="Fine-tune Model form" width="600">

## 3. Track the job and use your model

Fine-tuning runs as a background job — you will see "Fine-tuning job started
successfully!" and can follow it in the **Active Fine-tuning Jobs** card below
the form.

When it finishes, the new model appears under **Models → Model List**. From
there, benchmark it against other models to confirm it improved — see
[How to choose your embedding model](How-to-choose-your-embedding-model).
