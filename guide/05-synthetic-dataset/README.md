# 5.1 Generating a synthetic dataset (Queries & Relevance Set)

Use this when you have a corpus but no labelled queries / qrels.

## 5.1.1 Generate Queries

Navigate to **Generate → Generate Queries**
(`/project/{id}/generate/queries`).

1. **Select Corpus** — pick the corpus you uploaded.
2. **Query Types** — tick one or more. Each type changes the LLM prompting
   style:
   - **Direct Questions** — directly answerable from the text.
   - **Paraphrased** — rephrased versions of direct questions.
   - **Specific** — detailed, content-specific questions.
   - **Related** — questions about adjacent topics.
   - **Keyword** — keyword-style search queries.
   - **Mimic** — questions that mimic the style of a provided example query
     set. If you tick this, also pick **Select Example Queries** (required)
     and optionally **Example Corpus** (the corpus those examples came from).
3. **Model Type** — choose **HuggingFace Model** *(default,*
   `Qwen/Qwen2.5-3B-Instruct`*)* or **OpenAI Compatible Endpoint** (then enter
   API URL + API Key).
4. **Temperature** (0–2, default 0.7), **Max Tokens** (default 256), and
   optionally cap with **Max Number of Queries**.
5. **Configure Languages** — pick from ISO 639-1 codes (`en`, `zh`, `fr`, `es`,
   `pt`, `de`, `it`, `ru`, `ja`, `ko`, `vi`, `th`, `ar`).
6. Set the resource block and click **Generate Queries**.

When the job finishes, a new query file appears under **Files → Query Files**.

## 5.1.2 Generate Relevance Set (qrels)

Navigate to **Generate → Generate Relevance Set**
(`/project/{id}/generate/qrels`).

This job uses an embedding model to retrieve top candidates per query, then
asks an LLM to label them.

1. **Select Corpus** and **Select Queries**.
2. **Select Embedding Model** — used for first-stage retrieval. Any project
   embedding model works; for best qrel quality, use a model already adapted to
   your domain (e.g. the Phase-1 model from
   [§5.2](../06-two-phase-training/README.md)).
3. **LLM Model Type** — HuggingFace or OpenAI-compatible (same fields as
   5.1.1).
4. **Qrel parameters:**
   - **Top-K** *(default 5)* — how many retrieved candidates per query the LLM
     judges.
   - **Absolute Margin** / **Relative Margin** *(default 0.1 each)* — filtering
     margins for the candidate scores.
5. Set the resource block and submit.

When done, a new qrel file appears under **Files → Relevance Set Files**. Go
to **IR Datasets** and assemble or update a dataset that uses all three files.

---

Next: [2-phase training for embedding models](../06-two-phase-training/README.md)
