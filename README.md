#  Unthinkable_Assignment_7KnowledgeHub

**Local RAG for PDFs — LangChain + Ollama**

This repository demonstrates a fully local Retrieval-Augmented Generation (RAG) pipeline built with **LangChain** and **Ollama**. It enables question-answering over uploaded PDF documents (for example, the NIDM Cyclone Disaster Manual) — all running on your machine so your documents never leave your control.

---

##  Features

*  **Fully local processing** — embeddings, vector store, and LLM inference run locally (Ollama).
*  **PDF ingestion** with intelligent chunking and metadata preservation.
*  **Multi-query retrieval** and context-aware prompt construction to improve answer accuracy.
*  **LangChain-based RAG** orchestration for clean modularity and extensibility..
* **Jupyter Notebook** for reproducible experimentation and debugging.

---

##  Repository Structure

```
Unthinkable_Assignment_7KnowledgeHub/
├─ README.md
├─ requirements.txt
├─ .env.example
├─ app/                      # Streamlit app (interface)
│  ├─ streamlit_app.py
│  └─ static/
├─ ingestion/                # PDF ingestion and chunking
│  ├─ ingest.py
│  └─ utils.py
├─ rag/                      # LangChain pipeline, retriever, prompt templates
│  ├─ pipeline.py
│  └─ prompts.py
├─ notebooks/
│  └─ experiments.ipynb
├─ examples/
│  └─ NIDM_Cyclone_Manual_sample.pdf
└─ README.md
```

---

##  Quickstart (Local)

> These instructions assume you have Python 3.10+ and Docker (optional for Ollama) installed.

1. **Clone repository**

```bash
git clone <your-repo-url>
cd Unthinkable_Assignment_7KnowledgeHub
```

2. **Create virtual environment & install**

```bash
python -m venv .venv
source .venv/bin/activate    # macOS / Linux
.\.venv\Scripts\activate   # Windows
pip install -r requirements.txt
```

3. **Configure environment**

Copy `.env.example` to `.env` and adapt if needed. The project is designed to run without external API keys as Ollama is a local LLM runner. If you use a different LLM provider you can add keys in `.env`.

```bash
cp .env.example .env
# edit .env if necessary
```

4. **Run Ollama (local LLM)**

If you use Ollama, follow Ollama instructions to install and serve your preferred model locally. Typically:

```bash
# Example (follow Ollama's official docs):
ollama pull <model-name>
ollama serve <model-name>
```

5. **Ingest a PDF**

```bash
python ingestion/ingest.py --pdf examples/NIDM_Cyclone_Manual_sample.pdf --persist_dir ./vectorstore
```

This will:

* Extract text
* Split into intelligent chunks with overlap
* Create embeddings and persist a local vector store (FAISS / Chroma depending on config)

6. **Run the Streamlit app**

```bash
streamlit run app/streamlit_app.py
```

Open `http://localhost:8501` and upload a PDF or select an already-ingested document. Ask natural-language questions and see local RAG answers.

---

##  Configuration

Key configuration options live in `ingestion/utils.py` and `rag/pipeline.py`:

* `chunk_size` and `chunk_overlap` — controls how text is split.
* Vectorstore backend — switchable (Chroma, FAISS, Milvus with local config).
* Retriever strategy — top-k, hybrid, or semantic + keyword.
* `temperature`, `max_tokens` — LLM generation settings when calling Ollama.

---

## How the RAG pipeline works (high level)

1. **PDF ingestion** — extract text from PDF pages and store source metadata.
2. **Chunking** — split text into overlapping chunks for context preservation.
3. **Embedding** — compute embeddings locally for each chunk and persist vectorstore.
4. **Retrieval** — for each user query, perform multi-query retrieval to collect top-k chunks.
5. **Prompt assembly** — create a single context-aware prompt with retrieved chunks and the user question.
6. **Generation** — call Ollama (local LLM) to produce grounded answers; include citations (source chunk page numbers).

---

##  Jupyter Notebook

Open `notebooks/experiments.ipynb` to:

* Run the ingestion pipeline step-by-step
* Visualize chunk distribution
* Inspect embeddings and similarity scores

---

##  Tips for Better Answers

* Ingest the whole document instead of relying on a single page.
* Tune `chunk_size` and `overlap` for documents with long sentences or tables.
* Use multi-query retrieval (fact + context queries) for complex topics.

---

*Happy local RAGging!* 🚀
