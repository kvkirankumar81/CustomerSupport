Customer Support LLM Agent (simplerLLM)
=====================================

Overview
--------
This repository contains a minimal Python project that demonstrates an LLM-based agent (using `simplerLLM` if available) and two actions:

- Action-1: Use a fine-tuned BERT model to predict the customer query type.
- Action-2: Use RAG-style semantic search to find relevant documents.

Important TODOs
---------------
- Provide a path to your fine-tuned BERT model in `config.yaml` under `bert.model_path`.
- Provide a document corpus (directory of text files) at `rag.docs_path` or supply a saved FAISS index at `rag.vectorstore_path`.
- Install and configure your `simplerLLM` package and add any credentials/config in `config.yaml`.

Note on `simplerLLM` integration
--------------------------------
This repo includes a `SimplerLLMWrapper` in `src/agent/agent.py` that attempts to import and call `simplerllm`.
- A placeholder API key is in `config.yaml` under `llm.api_key` (value: `sk-REPLACE_ME`). Replace it with your real key.
- The wrapper tries multiple common client and function names (e.g. `Client`, `SimpleClient`, `complete`, `generate`, `chat`). If your `simplerllm` package uses different names or requires extra configuration, adapt `SimplerLLMWrapper` accordingly.

Security note: keep your API key secret and do not commit it to public repositories. Use environment variables or secret managers for production deployments.

Quick install (Windows PowerShell)
---------------------------------
```powershell
python -m venv .venv; .\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

Running the demo
----------------
```powershell
python -m src.main --query "My order hasn't arrived"
```

Files
-----
- `config.yaml` — project configuration (fill in model paths and data locations)
- `requirements.txt` — required Python packages
- `src/agent/agent.py` — LLM wrapper (uses `simplerllm` if installed)
- `src/actions/bert_predictor.py` — Action-1: BERT prediction (requires `transformers` model)
- `src/actions/rag_search.py` — Action-2: RAG semantic search (requires `sentence-transformers` + `faiss`)
- `src/main.py` — Orchestration that runs both actions and prints combined JSON

Web UI & Reindexing
--------------------
- A minimal Flask-based web UI is available at `src/webapp.py`. Install Flask (`pip install flask`) and run:

```powershell
python -m src.webapp
```

- To avoid rebuilding the FAISS index on every run, use the `--no-reindex` flag when running `src.main`:

```powershell
python -m src.main --query "My laptop screen is flickering" --no-reindex
```

	This requires a previously-built FAISS index file and its metadata present at the path configured by `rag.vectorstore_path` (or the default index file saved next to your CSV). If `--no-reindex` is provided but no index exists, the program will return a message indicating the missing index.

Notes
-----
The code includes clear `TODO` responses when required resources (fine-tuned model, document corpus, or simplerLLM config) are missing. Fill those in, then re-run.
