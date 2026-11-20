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

## ⚡ Core Features (v1)

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
| **v1** | Local text RAG, metadata filters, live metrics (VRAM/tokens), & PDF security. | Validation of answer accuracy & retriever hit-rate via a golden test set. | ✅ **Completed** |
| **v2** | Multimodal image processing (diagrams/charts) & cross-encoder re-ranking. | Measurement of precision gains (Precision@K) & image caption verification. | 🚀 **In Progress** |
| **v3** | Handwriting OCR for handwritten lecture notes and sketches. | Benchmarking text recognition accuracy using Word Error Rate (WER). | 🔮 **Planned** |

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

Good example:
<img width="1854" height="810" alt="V1_Testlauf" src="https://github.com/user-attachments/assets/5fa3dfaf-192a-4b23-9d6a-52c536a86ca2" />
Why: correct background and content

Bad example:
<img width="1865" height="789" alt="V1_Testlauf2" src="https://github.com/user-attachments/assets/301fc809-8c19-48bc-bdfc-a201a4d0a977" />
Why: wrong background and correct content

Next steps: preparation of a dedicated test-set to evaluate the quality, performance and so on.
Hypothesis: Retriever is falling, despite using vector/chromaDB?
