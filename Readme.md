# Retrieval-Augmented Generation (RAG) — Notebook 1 + Notebook 2

This repo contains two small Colab-style notebooks that demonstrate a basic RAG pipeline:
**chunk text → embed → FAISS retrieve → (optional) LLM answer using retrieved context**.

## Contents

- Notebook 1 (File KB RAG): `Untitled0.ipynb` + `External_KB_for_RAG.txt`
- Notebook 2 (Web-crawled RAG): `Untitled1.ipynb`

## Common setup (Colab)

```bash
pip install sentence-transformers faiss-cpu langchain_community replicate requests beautifulsoup4
```

Note: Installing `replicate`/`langchain_community` may upgrade `requests` and produce a Colab warning about version mismatches. If you want to avoid it, remove `requests` from the install line (Colab already includes it).

## Notebook 1 — File Knowledge Base RAG (`Untitled0.ipynb`)

**Purpose:** compare an LLM answer **without** retrieval vs **with** retrieval from `External_KB_for_RAG.txt`.

**What you do**

1. Install deps and run imports.
2. Add your Replicate token in Colab secrets.
3. When prompted, enter a question.
4. Upload `External_KB_for_RAG.txt` when requested.
5. Review the final output: initial answer (no RAG) vs context-augmented answer (RAG).

**What it does (under the hood)**

- Chunks `External_KB_for_RAG.txt` (overlapping windows)
- Embeds chunks with `all-MiniLM-L6-v2`
- Builds a FAISS index and retrieves top matches for the query
- Calls an LLM via Replicate using retrieved chunks as context

**Models**

- LLM (via Replicate): `ibm-granite/granite-3.2-8b-instruct`
- Embeddings: `all-MiniLM-L6-v2`

**Replicate token**

The notebook reads the token from Colab secrets using `userdata.get('REPLICATE_api_token')`, so the secret name should be:

- `REPLICATE_api_token`

## Notebook 2 — Web-crawled RAG (`Untitled1.ipynb`)

**Purpose:** build a retriever over a website and print the retrieved passages for a query.

**What you do**

1. Install deps and run imports.
2. Run the crawl cell (default: `https://thealliance.ai/`), which saves `thealliance_ai_content.txt`.
3. Run chunking + embeddings + FAISS indexing.
4. Enter a query and review the retrieved chunks that the notebook prints.

**What it does (under the hood)**

- Fetches a web page, strips script/style/noscript, and extracts visible text
- Chunks the text, embeds with `all-MiniLM-L6-v2`, and indexes with FAISS
- Retrieves top-k chunks for your query and prints them

This notebook focuses on retrieval output; you can optionally feed the retrieved chunks into an LLM prompt (same pattern as Notebook 1).
