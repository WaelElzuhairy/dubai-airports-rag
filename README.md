# Dubai Airports RAG Customer Service Assistant

A Retrieval-Augmented Generation (RAG) chatbot that answers passenger questions about Dubai International Airport (DXB) using a local knowledge base and open-source language models — no API keys required.

![Chat Demo](screenshots/Screenshot%202025-12-30%20001132.png)

---

## Features

- Semantic search over Dubai Airport's customer service knowledge base
- Local inference using Microsoft Phi-2 (~2.7B parameter LLM)
- Gradio web UI with real-time chat interface and public share link
- Pre-built FAISS vector index — no re-embedding needed on startup
- Covers: check-in counters, drop-off rules, smart gates, baggage services, VIP lounges, unaccompanied minors, passport issues, and more

---

## Tech Stack

| Component | Tool |
|---|---|
| Language | Python 3.x |
| Notebook | Jupyter |
| Embeddings | `sentence-transformers` (all-MiniLM-L6-v2) |
| Vector Store | FAISS (`faiss-cpu`) |
| LLM | Microsoft Phi-2 via Hugging Face `transformers` |
| Document Parsing | `python-docx` |
| Web UI | Gradio |
| Deep Learning | PyTorch |

---

## Installation

**Requirements:** Python 3.9+, ~4 GB disk space (for Phi-2 model download)

```bash
git clone https://github.com/YOUR_USERNAME/dubai-airports-rag.git
cd dubai-airports-rag
pip install -r requirements.txt
```

> **GPU optional:** The notebook runs on CPU. For faster inference, install the CUDA version of PyTorch from [pytorch.org](https://pytorch.org).

---

## Usage

1. Open the notebook in Jupyter or Google Colab:
   ```bash
   jupyter notebook DXB_RAG.ipynb
   ```

2. Run all cells sequentially. The first run will download the Phi-2 model (~5 GB via Hugging Face).

3. The final cells launch a Gradio web UI automatically:
   - A local URL (`http://127.0.0.1:7860`) opens in your browser
   - A public share link is also generated (valid for 72 hours)

4. Ask questions in natural language:
   - *"How long can I stop at the drop-off area?"*
   - *"When do check-in counters close?"*
   - *"Who is eligible for smart gates?"*
   - *"What do I do if my passport wasn't stamped?"*

> The vector index is pre-built in `artifacts/` — embedding steps are skipped on reuse.

---

## Project Structure

```
dubai-airports-rag/
├── DXB_RAG.ipynb              # Main notebook — full pipeline end-to-end
├── requirements.txt           # Python dependencies
├── data/
│   └── DA knowledge Articles.docx   # Source knowledge base (Dubai Airport docs)
├── artifacts/
│   ├── faiss-index/
│   │   ├── da_faiss.index     # Pre-built FAISS vector index
│   │   └── chunks_metadata.pkl
│   ├── processed_blocks.jsonl # Parsed document blocks
│   └── processed_chunks.jsonl # Final text chunks used for retrieval
├── docs/
│   ├── technical_report.pdf   # Full technical write-up
│   └── presentation.pptx      # Project presentation slides
└── screenshots/               # Demo screenshots for documentation
```

---

## How It Works

```
User Question
     │
     ▼
Embed with all-MiniLM-L6-v2
     │
     ▼
FAISS similarity search → top-5 relevant chunks
     │
     ▼
Build RAG prompt: [System] + [Context] + [Question]
     │
     ▼
Microsoft Phi-2 generates answer
     │
     ▼
Extract and return answer to user
```

The knowledge base (57 chunks) was extracted from Dubai Airport's internal customer service documentation, chunked by logical section, and embedded once — then saved for reuse.

---

## Limitations

- **Phi-2 is small:** Occasional hallucinations or off-topic answers on edge cases
- **Static knowledge base:** The DOCX is not updated automatically; re-run the embedding cells after any document change
- **No conversation memory:** Each question is answered independently (no multi-turn context)
- **English only:** The LLM and embeddings are English-trained
- **CPU inference is slow:** Phi-2 may take 30–90 seconds per response on CPU

---

## Future Improvements

- Replace Phi-2 with a stronger model (Mistral 7B, LLaMA 3, or a Claude API call)
- Add multi-turn conversation memory
- Support Arabic language queries
- Auto-refresh knowledge base from a live document source
- Deploy as a persistent web app (Hugging Face Spaces or Streamlit Cloud)
- Add confidence scoring and source citation per answer

---

## Documentation

- [Technical Report](docs/technical_report.pdf) — full write-up of methodology, evaluation, and results
- [Presentation](docs/presentation.pptx) — slide deck overview

---

## Author

Wael Zuhairy — built as a student AI project demonstrating RAG architecture with local LLMs.
