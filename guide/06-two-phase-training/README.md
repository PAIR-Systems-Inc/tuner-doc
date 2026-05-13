# 5.2 2-phase training for embedding models

"2-phase training" is the recommended pattern when you only start with a
corpus. The two phases share the **Fine-tune Model** form — what differs is
whether **Train with Hard Negatives (Phase 2)** is enabled and what data you
feed in.

```
       Phase 1                       Mining                       Phase 2
   ┌────────────────┐         ┌────────────────────┐         ┌────────────────────┐
   │ Generate       │         │ Generate Relevance │         │ Fine-tune again,   │
   │ Queries on     │  ──►    │ Set using the      │  ──►    │ this time with     │
   │ corpus         │         │ Phase-1 model      │         │ Hard Negatives ON  │
   └────────────────┘         └────────────────────┘         └────────────────────┘
        │                            │                              │
        ▼                            ▼                              ▼
   Phase-1 dataset             High-quality qrels             Phase-2 model
   (corpus + queries,          mined from Phase-1             (final, domain-adapted
    no qrels yet)              embeddings                      embedding model)
```

## Phase 1 — bootstrap an in-domain embedding model

1. Upload your **corpus** (see [§2](../02-dataset-upload/README.md)).
2. Run **Generate Queries**
   ([§5.1.1](../05-synthetic-dataset/README.md#511-generate-queries)) to
   produce synthetic queries.
3. In **IR Datasets**, create a dataset that combines the corpus and the
   generated queries. *Leave qrels empty for now.*
4. Open **Models → Fine-tune** and run a normal embedding fine-tune on this
   dataset. **Leave "Train with Hard Negatives" unchecked.** This is Phase 1.

The resulting model has learned the domain's vocabulary but has only seen
in-batch random negatives.

## Mining — generate qrels with the Phase-1 model

1. Open **Generate → Generate Relevance Set**
   ([§5.1.2](../05-synthetic-dataset/README.md#512-generate-relevance-set-qrels)).
2. Set **Select Embedding Model** to the Phase-1 model. Using your own
   in-domain model here produces much more useful candidate rankings than a
   generic off-the-shelf embedder.
3. Run the job. A new qrel file is produced.
4. Back in **IR Datasets**, attach this qrel file to the dataset from Phase 1
   (or create a new dataset with all three files).

## Phase 2 — fine-tune with hard negatives

1. Open **Models → Fine-tune** again.
2. **Base Model** — tick **"Use a model from this project"** and select the
   Phase-1 model. Phase 2 continues training from Phase 1.
3. **Dataset** — choose the dataset that now has qrels attached.
4. Tick **Train with Hard Negatives (Phase 2)** under *Hard Negative Training*.
   The checkbox is disabled until the selected dataset has a qrels file
   attached.
5. Set the hard-negative parameters:
   - **Max Negatives per Query** *(default 2)* — also acts as the minimum;
     queries with fewer qualifying negatives are dropped.
   - **Positive Threshold** *(default 1.0)* — min qrel score to count as a
     positive.
   - **Negative Threshold** *(default 0.25)* — max qrel score to count as a
     negative.
   - Scores between the two are discarded. **Positive must be greater than
     negative**, or the form refuses to submit.
6. Submit.

Under the hood, for each query the highest-scored document becomes the
positive and other low-scoring documents become explicit hard negatives,
yielding a substantially harder training signal than Phase 1.

## Evaluating the improvement

Run **Benchmarks → Run Benchmark** on the same dataset for both the Phase-1
and Phase-2 models, then open **Benchmarks → Compare Models** to see the
metric lift side by side.
