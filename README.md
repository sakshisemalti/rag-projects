# Agentic RAG

A collection of Retrieval-Augmented Generation (RAG) projects exploring different pieces of the RAG + agentic stack — from a straightforward document Q&A pipeline, to that same pipeline wrapped as an MCP tool for an autonomous decision-making agent, to a separate semantic product-search app.

Each project is self-contained, with its own `README.md`, dependencies, and Docker setup.

---

## 📦 Projects

### [`company-knowledge-assistant/`](./company-knowledge-assistant)

A RAG application for answering questions over internal company documents, plus an agentic layer built on top of it.

- **RAG pipeline:** FastAPI + LangChain + OpenAI + Cohere Rerank + PostgreSQL/pgvector + Redis Semantic Cache + LangSmith + RAGAS. Ingests PDFs/Markdown/DOCX/TXT and answers grounded questions.
- **Agentic layer:** An MCP server exposes the RAG pipeline as a `rag_ask` tool, alongside `approve`/`reject` action tools. A Streamlit app runs an HR Expense Compliance Agent that reads a batch of claims, looks up policy via `rag_ask`, and autonomously approves or rejects each one.

Demonstrates: retrieval quality (reranking, caching, evaluation), and turning a RAG pipeline into a tool an LLM agent can call to take real actions via MCP.

→ See [`company-knowledge-assistant/README.md`](./company-knowledge-assistant/README.md) for setup and architecture.

### [`smartfind/`](./smartfind)

SmartFind is a full-stack semantic product search app. Upload a CSV of products, generate OpenAI embeddings for each, store them in PostgreSQL with `pgvector`, and search them with natural-language queries via LangChain.

Demonstrates: a minimal end-to-end vector search pipeline (Flask instead of FastAPI, no reranking/caching/agent layer) — useful as a simpler reference alongside the more layered Company Knowledge Assistant.

→ See [`smartfind/README.md`](./smartfind/README.md) for setup and architecture.

---

## 🧭 How the projects relate

| | Company Knowledge Assistant | SmartFind |
|---|---|---|
| Domain | Internal company documents | Product catalog |
| Web framework | FastAPI | Flask |
| Vector store | PostgreSQL + pgvector (HNSW) | PostgreSQL + pgvector |
| Reranking | Cohere Rerank | — |
| Caching | Redis Semantic Cache | — |
| Observability | LangSmith | — |
| Evaluation | RAGAS | — |
| Agentic layer | MCP server + LangChain agent (approve/reject workflow) | — |
| Interface | Web UI + Streamlit agent UI | Web UI |

The Company Knowledge Assistant is the more complete reference implementation (retrieval quality tooling, an agent that acts on retrieval results via MCP). SmartFind is a leaner example focused purely on embedding generation and vector search.

---

## 🛠 Common Stack

Both projects share the same core building blocks:

- **LangChain** for orchestration
- **OpenAI** for embeddings (and generation, in the Company Knowledge Assistant)
- **PostgreSQL + pgvector** for vector storage
- **Docker / Docker Compose** for local development

---

## 📂 Repository Structure

```text
.
├── README.md                        # you are here
├── company-knowledge-assistant/     # RAG + agentic (MCP) expense compliance demo
│   ├── README.md
│   ├── app/
│   ├── agent/
│   ├── data/
│   ├── init-db/
│   └── seed/
└── smartfind/                       # semantic product search
    ├── README.md
    ├── app/
    ├── init-db/
    └── product_real_data.csv
```

---

## 🚀 Getting Started

Each project runs independently. Pick one, `cd` into it, and follow its own README:

```bash
cd company-knowledge-assistant
# see company-knowledge-assistant/README.md

cd smartfind
# see smartfind/README.md
```

---

## 🧠 Future Improvements

- Shared `docker-compose.yml` at the root to spin up both projects together
- Extract common RAG/embedding utilities into a shared internal package
