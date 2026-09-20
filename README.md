#  Kestrel Labs Multi-Agent RAG System

A stateful, multi-agent Retrieval-Augmented Generation (RAG) pipeline built with **LangGraph** to strictly and safely answer questions against Kestrel Labs' internal documentation. 

This system features multi-turn memory, strict hallucination guardrails, publication-date conflict resolution, and programmatic LangSmith evaluations.

### 📊 Observability & Traces
**LangSmith Project Link:** `[https://smith.langchain.com/o/42a1d80d-bbdd-4399-b905-b00c4267841b/projects/p/d2231b03-5f4a-46cb-b99c-558551a51d6e?timeModel=%7B%22duration%22%3A%221h%22%7D&tab=0]`
*(Reviewer: Click the link above to view the 15-question dataset evaluation and runtime traces)*

---

## 🏗️ Architecture Overviewx
The pipeline routes user queries through a specialized graph of agents to ensure high-fidelity answers without hallucination. For a complete visual flowchart and orchestration details, see `design_document.md`.

1. **Router:** Classifies intent (casual chat vs. research) and resolves ambiguous pronouns using multi-turn memory.
2. **Retriever:** Queries a local `ChromaDB` vector store using `sentence-transformers`.
3. **Verifier:** A critic node that compares retrieved chunks against the query and issues a strict verdict (`supported`, `partially_supported`, `conflicting_evidence`, or `insufficient_evidence`). It also handles date-based conflict resolution.
4. **Synthesizer:** Generates the final answer with strict `[chunk_id]` inline citations. If the Verifier issues an `insufficient_evidence` verdict, the Synthesizer executes a hard-coded bypass to refuse the prompt entirely, ensuring 100% zero-hallucination on unsupported queries.

---

## 📂 Repository Structure
This repository is structured according to the Kestrel Labs submission specifications:

```text
/
├── .env.example                 # Template for required environment variables
├── .gitignore                   # Ignores sensitive keys and local ChromaDB
├── README.md                    # Setup and execution instructions (This file)
├── design_document.md           # Architecture diagram and orchestration rationale
├── reflection.md                # Trade-offs, latency observations, and future improvements
├── corpus.jsonl                 # The original, unchanged Kestrel documentation
├── kestrel_agent_notebook.ipynb # The primary pipeline and evaluation code
└── results/                     # Programmatic benchmark outputs
    ├── eval_questions.jsonl
    ├── eval_results.jsonl
    ├── metrics_summary.json
    └── improvement.md