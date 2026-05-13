# GoodMem Cloud — User Guide

A walkthrough of the GoodMem Cloud platform: log in, create a project, upload
data, fine-tune and benchmark an embedding model, then explore advanced flows
for synthetic data generation and 2-phase training.

The guide is split into one folder per section. Each section folder has its
own `assets/` directory for screenshots and diagrams.

## Sections

1. [Log in and create a project](guide/01-login-and-project/README.md)
2. [Add a dataset (file upload)](guide/02-dataset-upload/README.md)
3. [Fine-tune an embedding model](guide/03-fine-tune/README.md)
4. [Benchmark the model](guide/04-benchmark/README.md)
5. Advanced topics
   - [5.1 Generating a synthetic dataset (Queries & Relevance Set)](guide/05-synthetic-dataset/README.md)
   - [5.2 2-phase training for embedding models](guide/06-two-phase-training/README.md)

---

## Appendix — Where to find things

| You want to… | Go to |
|---|---|
| Monitor a running job / tail logs | **Jobs** (`/project/{id}/jobs`) |
| List, rename, delete models | **Models → Model List** |
| Compare benchmark runs | **Benchmarks → Compare Models** |
| Calibrate / visualize a reranker | **Calibration** (sub-pages: Calibrate Reranker, Calibrators, Visualize) |
| Manage org / billing / profile | User dropdown (top-right): **Profile**, **My Organization**, **Billing** |
| Interactive API reference | `http://<host>:8888/docs` (Swagger) or `/redoc` |
