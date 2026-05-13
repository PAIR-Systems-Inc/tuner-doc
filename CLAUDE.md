# CLAUDE.md

Guidance for Claude Code when updating the GoodMem Cloud user guide.

## What this repo is

This repo contains a user-facing markdown walkthrough of the **GoodMem Cloud**
platform. It is *not* the platform's source code — it documents an external
project.

Layout:

- [README.md](README.md) — top-level index with section links and the
  cross-cutting appendix.
- `guide/<NN>-<slug>/README.md` — one folder per section. Each folder has its
  own `assets/` directory for screenshots and diagrams referenced from that
  section's `README.md`.

Section folders:

| § | Folder |
|---|---|
| 1 | [guide/01-login-and-project/](guide/01-login-and-project/README.md) |
| 2 | [guide/02-dataset-upload/](guide/02-dataset-upload/README.md) |
| 3 | [guide/03-fine-tune/](guide/03-fine-tune/README.md) |
| 4 | [guide/04-benchmark/](guide/04-benchmark/README.md) |
| 5.1 | [guide/05-synthetic-dataset/](guide/05-synthetic-dataset/README.md) |
| 5.2 | [guide/06-two-phase-training/](guide/06-two-phase-training/README.md) |

## Source of truth

The platform's authoritative source is the GitHub repository:

**https://github.com/PAIR-Systems-Inc/goodmem-cloud**

When the user asks you to **update, fix, or extend** the guide, always
re-verify against that repository (default branch `master`) before editing.
UI labels, routes, field names, and defaults change there, and this guide
must follow.

Use `gh` or `WebFetch` against `github.com/PAIR-Systems-Inc/goodmem-cloud`
to read the relevant files. Examples:

```bash
# Read a file from the default branch
gh api repos/PAIR-Systems-Inc/goodmem-cloud/contents/web/react-app/src/components/models/ModelFinetune.tsx \
  --jq '.content' | base64 -d

# Look at recent commits affecting the frontend
gh api repos/PAIR-Systems-Inc/goodmem-cloud/commits --jq '.[].commit.message' | head
```

A local clone may also exist at `/data/rogger/goodmem-cloud/` for convenience,
but **do not treat the local checkout as authoritative** — it may be stale or
have local edits. If you use it, run `git -C /data/rogger/goodmem-cloud fetch
&& git -C /data/rogger/goodmem-cloud log --oneline origin/master -5` first to
confirm it's reasonably current, and prefer the GitHub copy if there's any
doubt.

The platform's own `CLAUDE.md` (at the repo root on GitHub) has the
architecture overview — read it first for context.

## Where each guide section maps in the source

The React SPA frontend lives under `web/react-app/src/` in the goodmem-cloud
repo. Key files per section:

| Guide section | Source files to check |
|---|---|
| §1 Login | `src/components/auth/Login.tsx`, `src/components/auth/AuthCallback.tsx` |
| §1 Create project | `src/components/Dashboard.tsx` (the "New Project" modal) |
| §2 File uploads | `src/components/files/CorpusFiles.tsx`, `QueryFiles.tsx`, `QrelFiles.tsx` |
| §2 Dataset assembly | `src/components/dataset/DatasetsView.tsx` |
| §3 Fine-tune | `src/components/models/ModelFinetune.tsx` |
| §4 Benchmark | `src/components/benchmark/BenchmarksRun.tsx`, `BenchmarksResults.tsx`, `CompareModels.tsx` |
| §5.1 Generate Queries | `src/components/generation/QueryGeneration.tsx` |
| §5.1 Generate Qrels | `src/components/generation/QrelGeneration.tsx` |
| §5.2 2-phase training | `ModelFinetune.tsx` — search for `useHardNegatives` / `"Phase 2"` |
| Sidebar / nav | `src/components/Layout.tsx` |

Backend routes (if you need to confirm API behavior referenced in the guide)
are under `routes/`. Background job logic is under `tasks/`.

## How to update the guide

1. Identify which section the change affects, and open that section's
   `guide/<NN>-<slug>/README.md`.
2. Open the corresponding file(s) on GitHub and confirm:
   - Exact button / field labels (quote them verbatim in the guide).
   - The route path (`/project/{projectId}/...`).
   - File-format requirements (JSONL schemas).
   - Default values for numeric fields.
3. Edit the section `README.md` in place — prefer `Edit` over a full rewrite.
4. Reference screenshots from the section's own `assets/` folder using
   relative paths (e.g. `![...](assets/foo.png)`).
5. If the sidebar navigation changed, update the **Appendix** table in
   the top-level `README.md` and any "Navigate to …" lines in section files
   that reference the old path.
6. Only edit the top-level `README.md` itself when changing the section index
   or the cross-cutting appendix.

## Editorial conventions for this guide

- Audience: end users of the platform, not developers. No source file paths
  in the guide body.
- Refer to UI elements by their **visible label** in bold (e.g.
  **New Project**, **Train with Hard Negatives (Phase 2)**).
- Routes are shown as `/project/{id}/...` with `{id}` as a placeholder.
- All file uploads are JSONL. When showing a schema, use a fenced ` ```json `
  block with one example object (not the full JSONL).
- Keep tables for option/default summaries; keep numbered lists for
  step-by-step flows.
- Do not add emojis.
- Do not invent features. If a feature is referenced in the source but not yet
  exposed in the UI, leave it out of the guide.
