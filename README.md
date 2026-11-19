# MentorAI_RAG-System
local-running RAG System on my old study files - RTX3050


# Project Report: MentorAI – Local & Privacy-Focused RAG Assistant

## 1. Introduction & Objectives
**MentorAI** is a high-performance, privacy-oriented RAG (Retrieval-Augmented Generation) study assistant specifically designed to run locally on consumer hardware (e.g., an NVIDIA RTX 3050 with 6GB VRAM). The system allows users to locally index academic learning materials (scripts, PDFs) and leverage state-of-the-art Large Language Models (LLMs) to generate precise, source-backed answers—**completely offline and without any data leakage to the internet**.

---

## 2. Tech Stack & Architecture
The system relies on a modular, containerized microservice architecture:

* **UI & Frontend:** [Streamlit](https://streamlit.io/) for a reactive, user-friendly web interface.
* **Vector Database:** [ChromaDB](https://www.trychroma.com/) (running in a Docker container at `localhost:8000`) for efficient storage and similarity search of embeddings.
* **LLM Inference:** [Ollama](https://ollama.com/) for local hosting of open-source models (e.g., `qwen2.5:3b-instruct` or `llama3`).
* **Embeddings & NLP:** LangChain with HuggingFace (`all-MiniLM-L6-v2`, GPU-accelerated via CUDA).
* **Document Parsing & Security:** PyMuPDF (`fitz`) for high-performance text chunking, metadata extraction, and built-in password decryption for protected PDFs.

---

## 3. Core Features & Implementation
* **Dynamic Document Management:** Direct PDF uploads via the sidebar with automatic splitting into semantic chunks (`RecursiveCharacterTextSplitter`).
* **Metadata Filtering:** Target-driven queries utilizing multi-level filters (category, semester, subject).
* **Precise Citations:** Every response provides exact references including filename, semester, subject, and page number.
* **Real-Time Performance Monitoring:** Automatic tracking of input/output tokens and generation speed ($\text{tokens/sec}$), supplied directly by the local inference engine.

---

## 4. Hardware Performance (RTX 3050 – 6GB VRAM)
Thanks to local GPU acceleration, the system delivers impressive performance metrics on entry-level hardware:
* **Generation Speed:** ~19–40 tokens/second (faster than average human reading speed).
* **VRAM Efficiency:** Stable concurrent operation of ChromaDB and a local 3B parameter LLM.

---

## 5. Universal Scalability & Use Cases
The architecture of MentorAI is universally adaptable to various knowledge domains and application scenarios:

* **🎓 Education & Academia:** 
  * Acts as a personal tutor for students and pupils to summarize lectures, generate practice questions, and connect cross-disciplinary knowledge.
* **🏭 Industry & Enterprise (Enterprise Knowledge Base):** 
  * Deployed as a white-labeled internal knowledge assistant for technical documentation, DIN standards, maintenance manuals, or ISO handbooks (fully GDPR-compliant due to on-premise execution).
* **🏠 Personal & Hobby (Personal Knowledge Management):** 
  * Organization of e-books, invoices, contracts, or recipes within a private, encrypted database.
