# MentorAI – Your Local & Privacy-Focused AI Study Assistant

**MentorAI** is a high-performance, fully local RAG (Retrieval-Augmented Generation) study assistant designed to run efficiently on consumer hardware (e.g., an NVIDIA RTX 3050 with 6GB VRAM). It allows users to locally index academic learning materials and leverage state-of-the-art Large Language Models to generate precise, source-backed answers—completely offline and with zero data leakage to the cloud.

---

## Tech Stack & Architecture

MentorAI relies on a modular, containerized microservice architecture:

* **UI & Frontend:** [Streamlit](https://streamlit.io/) for a reactive, user-friendly web interface.
* **Vector Database:** [ChromaDB](https://www.trychroma.com/) (running locally via Docker) for efficient similarity search and embedding storage.
* **LLM Inference:** [Ollama](https://ollama.com/) for running open-source models locally (`llama3`, `qwen2.5:3b-instruct`).
* **Embeddings & NLP:** LangChain with HuggingFace (`all-MiniLM-L6-v2`, GPU-accelerated via CUDA).
* **Document Parsing & Security:** PyMuPDF (`fitz`) for text chunking, metadata extraction, and built-in password decryption for protected PDFs.

---

## Core Features (v1)

- **100% Local & Private:** Your study notes and personal documents never leave your local machine.
- **Dynamic Document Management:** Direct PDF uploads via the sidebar with automatic semantic splitting (`RecursiveCharacterTextSplitter`).
- **Metadata Filtering:** Target-driven queries utilizing multi-level filters (category, semester, subject).
- **Precise Citations:** Every response includes exact references (filename, semester, subject, and page number).
- **Real-Time Performance Monitoring:** Live tracking of input/output tokens, generation speed (tokens/sec), Time-to-First-Token (TTFT), and VRAM allocation.
- **PDF Security:** Automatic detection and decryption of password-protected lecture materials.

---

## Hardware Performance (RTX 3050 – 6GB VRAM)

Optimized for entry-level GPU hardware, MentorAI achieves stable concurrent operation of ChromaDB and local 3B–8B parameter models:
- **Generation Speed:** ~19–40 tokens/second (exceeding average human reading speed).
- **VRAM Efficiency:** Smooth execution while maintaining a low footprint (~4.7 GB VRAM utilization).

---

## Roadmap & Versioning

| Version | Focus / Core Features | Evaluation & Quality Assurance | Status |
| :--- | :--- | :--- | :--- |
| **v1** | Local text RAG, metadata filters, live metrics (VRAM/tokens), & PDF security. | Validation of answer accuracy & retriever hit-rate via a golden test set. | **Completed** |
| **v2** | Multimodal image processing (diagrams/charts) & cross-encoder re-ranking. | Measurement of precision gains (Precision@K) & image caption verification. | **In Progress** |
| **v3** | Handwriting OCR for handwritten lecture notes and sketches. | Benchmarking text recognition accuracy using Word Error Rate (WER). | **Planned** |

---

## Quick Start

### 1. Prerequisites
Ensure you have **Docker**, **Python 3.8+**, and an **NVIDIA GPU Driver** (with NVIDIA Container Toolkit) installed.

### 2. Start ChromaDB (Docker)
Run the following command in your terminal to spin up the vector database:
```bash
docker compose up -d
```

### **3. Start ChromaDB (Docker)**
```bash
pip install -r requirements.txt
```


### **4. Run Streamlit App**
```bash
streamlit run app.py
```

### **5. LLM Setting & Testing**

Both examples has been tested with llama3.
Technically every model could be used. My GPU ist limited to 6GB VRAM, so I started with llama3.[also is already had this modell on my SSD]
Further tests and experiments with other modells are planned.

Good example:
<img width="1854" height="810" alt="V1_Testlauf" src="https://github.com/user-attachments/assets/5fa3dfaf-192a-4b23-9d6a-52c536a86ca2" />
Why: correct background and content

Bad example:
<img width="1865" height="789" alt="V1_Testlauf2" src="https://github.com/user-attachments/assets/301fc809-8c19-48bc-bdfc-a201a4d0a977" />
Why: wrong background (Nicolaus August Otto is the correct inventor -  "Hartmann was an auditor of the one pdf-file of the knowledge database.) Furthermore the content is a bit clumsy. - The answer does not sounds satisfying to me.

Next steps: preparation of a dedicated test-set to evaluate the quality, performance and so on.
Hypothesis: Retriever is falling, despite using vector/chromaDB?


### **6. Otto is the inventor again**


<img width="1463" height="698" alt="V4_Testlauf" src="https://github.com/user-attachments/assets/8d1ce05f-8000-4322-b262-41c231c0d1c0" />


1. The Initial Challenge & Goal
Starting Point: You developed a local RAG study assistant using Streamlit, ChromaDB via Docker, Ollama (Llama 3/Qwen), and HuggingFace embeddings on an RTX 3050, organizing academic scripts hierarchically (category/semester/subject/).

The Core Problem: Early hallucinations (such as the "Hartmann bug," where the LLM generated incorrect inventors like Hartmann or Daimler) highlighted that the system tended to guess from its internal training data rather than strictly sticking to the provided documents.

2. Implemented Milestones & Solutions
Determinism & System Prompt:
>Switched to temperature=0.0 and enforced a strict system prompt forbidding the LLM from inventing outside facts.

Metadata & Ingestion (ingest.py):
>Automated extraction of semesters, subjects, and page numbers from folder structures.
>Replaced .add() with .upsert() to ensure clean, idempotent updates when reloading PDFs.

Retrieval Control & Distance Threshold:
>Added a dynamic Distance Threshold slider in the Streamlit sidebar (defaulting to 0.65) to rigorously filter out noisy or irrelevant chunks (e.g., diesel engine scripts when asking about Otto engines).

The Python Safety Net:
>Implemented a programmatic check before calling the LLM: if filtering leaves zero relevant chunks, the script aborts immediately and outputs a reliable fallback message ("Dazu habe ich in den aktuellen Lernmaterialien keine Informationen gefunden"). This prevents the LLM from falling back on internal knowledge when context is empty.

Observability:
>Added an interactive UI expander in the chat to transparently display the exact chunks consulted, file paths, page numbers, and distance scores for every response.
