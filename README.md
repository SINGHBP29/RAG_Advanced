## IFC RAG System (Streamlit)

### 1) Create / activate venv

```bash
python3.14 -m venv .venv
source .venv/bin/activate
```

### 2) Install dependencies

This project has a large dependency graph; `uv` is the most reliable installer:

```bash
uv pip install -r requirements.txt --python .venv/bin/python
```

### 3) Configure Vertex AI (required)

Create a `.env` (or export env vars) with:

```bash
GCP_PROJECT_ID="your-gcp-project-id"
GCP_LOCATION="us-central1"
```

Authenticate with Application Default Credentials (ADC), e.g.:

```bash
gcloud auth application-default login
```

### 4) Optional: run local services

For the Qdrant vector store and Langfuse tracing:

```bash
docker compose up -d qdrant langfuse-server langfuse-db
```

### 5) Run the app

```bash
.venv/bin/python -m streamlit run app.py
```

### Notes

- The app needs outbound network access to call Vertex AI for embeddings and generation.
- Multimodal mode (tables + optional images) requires a prebuilt index: `python phase5_multimodal_rag.py --ingest [--include-images]`.
- Phase 6 (ColPali-like patch retrieval) can run offline in `hash` mode:
  - Build: `python phase6_colpali_like.py build --encoder hash --max-pages 2`
  - Query + attribution: `python phase6_colpali_like.py query --encoder hash -q "net income 2024" --annotate`
  - UI: `streamlit run ui/streamlit_app_phase6.py`
