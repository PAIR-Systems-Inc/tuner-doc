# 2. Add a dataset (file upload)

A "dataset" in GoodMem Cloud is a **combination of three files**:

| File type | What it is | Required schema (JSONL, one object per line) |
|---|---|---|
| **Corpus** | The documents to search/embed | `{"id": "doc-001", "text": "..."}` |
| **Queries** | The search queries | `{"id": "query-001", "query": "...", "chunk_id": "doc-001"}` |
| **Relevance Set (qrels)** | Query↔document relevance labels | `{"query_id": "query-001", "chunk_id": "doc-001", "score": 1.0}` |

All three files are **`.jsonl` only**. Qrel `score` is typically `0.0`
(not relevant) to `1.0` (relevant).

## Step 2a — upload the three files

In the sidebar, open **Files** and upload each file type on its own page:

| Page | Sidebar item | Route |
|---|---|---|
| Corpus | **Corpus Files** | `/project/{id}/files/corpus` |
| Queries | **Query Files** | `/project/{id}/files/queries` |
| Qrels | **Relevance Set Files** | `/project/{id}/files/qrels` |

On each page click **Upload**, then drop in your `.jsonl` file. Each list shows
the file id, name, size, and a per-row **Download / Rename / Delete** menu. The
corpus uploader also exposes a **Load Sample Corpus** button if you just want
to try the platform.

> **Tip:** You don't need all three files to start. A corpus alone is enough to
> generate synthetic queries (see [§5.1](../05-synthetic-dataset/README.md)).
> Queries can then be combined with the corpus to generate qrels.

## Step 2b — assemble the dataset

Go to **IR Datasets** (`/project/{id}/dataset`) and click **Add Dataset**.
Give it a name and pick the corpus / queries / qrels from the dropdowns. This
named dataset is what fine-tune and benchmark forms select from.

---

Next: [Fine-tune an embedding model](../03-fine-tune/README.md)
