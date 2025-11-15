<div align="center">

# 🔐 RAG Security Research Assistant

### *A Proposed Privacy-Preserving, Locally-Deployed RAG System for AI Security Research*

[![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![RAG](https://img.shields.io/badge/RAG-Retrieval%20Augmented%20Generation-orange.svg)]()
[![Security](https://img.shields.io/badge/Focus-AI%20Security-red.svg)]()

[Overview](#-overview) • [Features](#-key-features) • [Architecture](#-architecture) • [Evaluation Plan](#-evaluation-plan--expected-results) • [Documentation](#-documentation)

---

</div>

## 🎯 Overview

This project **proposes** a **secure RAG (Retrieval-Augmented Generation) system** specifically designed for AI security research. Built for IPHS 391, it will demonstrate best practices for trustworthy AI systems while addressing critical vulnerabilities in modern RAG pipelines.

### **The Problem**

Current RAG systems face serious security challenges:
- 🚨 **Prompt injection** and jailbreak attempts
- ☠️ **Data poisoning** attacks in retrieval corpora
- 🔓 **Privacy leakage** from unfiltered document recall
- 🎭 **Hallucinated citations** and incorrect citations
- ⚠️ **Stale or contradictory** retrieval results from unverified pipelines

### **The Solution**

A planned locally-deployed RAG assistant focused on summarizing and explaining academic work on *Security and Guardrails for RAG Systems* with:
- ✅ 100% offline operation with no external API calls
- ✅ Enterprise-grade security guardrails
- ✅ Accurate, cited responses with provenance tracking
- ✅ Open-source tools only

---

## 🎓 Use Cases & Audience

**Primary Users:**
- 🔬 **AI Researchers** studying trust, robustness, and safety in LLMs
- 🛡️ **IT and Security Teams** developing or auditing enterprise RAG pipelines
- 📚 **Students** learning about applied AI safety or trustworthy ML

**Success Criteria:**
- ≥85% factual accuracy compared to original source text
- ≤5 seconds average query latency on consumer hardware
- 100% of answers contain at least one correct citation
- 0% hallucinated or uncited factual claims in the evaluation set

---

## ✨ Planned Key Features

### **🔒 Security-First Design**
- **NeMo Guardrails** for blocking jailbreak patterns and checking for poisoning indicators
- **Local-only processing** - no data leaves your machine
- **Citation verification** - every answer will include source title + paragraph ID
- **Rejecting unclear citations** automatically

### **🛠️ Proposed Technical Stack**
- **Embeddings:** `sentence-transformers/all-MiniLM-L6-v2` (local HF Transformers)
- **Vector DB:** Chroma (persistent storage with metadata filtering)
- **Retrieval:** Dense vector search (top-5 cosine similarity)
- **LLM:** Mistral 7B via Ollama (private inference)
- **Guardrails:** NVIDIA NeMo Guardrails
- **100% Open Source**

---

## 🏗️ Proposed Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                     RAG Security Pipeline                    │
└─────────────────────────────────────────────────────────────┘

📄 Documents (~80 academic papers, ~1000 pages)
    ↓
🔪 Chunking (heading-based, max 512 tokens per chunk)
    ↓
🧮 Embedding (MiniLM-L6-v2, local)
    ↓
💾 Vector DB (Chroma with metadata)
    ↓
🔍 Retrieval (top-5 cosine similarity)
    ↓
🤖 LLM Generation (Mistral 7B, local)
    ↓
🛡️ Guardrails (NeMo - injection/poisoning checks)
    ↓
📎 Citation (source title + paragraph ID)
    ↓
✅ Final Answer with Sources
```

**Pipeline:** Ingestion → Chunking → Embedding → Vector DB → Retrieval → LLM → Guardrails → Answer → Citation

### **Component Selection**

| Component | Proposed Choice | Alternative | Why Selected |
|-----------|-----------------|-------------|--------------|
| **Vector DB** | ✅ Chroma | FAISS | Easier persistent storage + metadata queries |
| **Embeddings** | ✅ MiniLM | OpenAI Ada-002 | Offline, OSS, high cosine fidelity |
| **LLM** | ✅ Mistral 7B | GPT-4 | Fast, local, strong factuality |
| **Guardrails** | ✅ NeMo | Llama Guard | Native RAG-aware filtering |

---

## 📊 Planned Data & Corpus

**Sources:**
- ~80 academic papers (2023–2025) on RAG security topics
- arXiv, ACL Anthology, ACM Digital Library
- Formats: PDFs to be converted to `.txt` and `.md`
- Size: ~1000 pages

**Planned Preprocessing:**
- Remove boilerplate, references, licenses
- Normalize section headers for chunking
- Deduplicate abstracts and introductions
- Inject citation tags (`[AuthorYear-Section#]`) for evaluation tracing

**Topics Covered:**
- RAG security and robustness
- Privacy preservation
- Fairness & bias mitigation
- Explainability and provenance tracing
- Watermarking & accountability

**Constraints:**
- 100% local processing (no API calls)
- Open-source tools only
- Simple CLI for non-technical users
- Academic fair-use compliance

---

## 📈 Evaluation Plan & Expected Results

**Evaluation Design:**
- **Test Set:** 15 curated questions across major RAG security areas (privacy preservation, robustness against adversarial documents, detection of data poisoning, fairness & bias mitigation, explainability and provenance tracing, watermarking & accountability)
- **Approach:** Vector-only retrieval with NeMo guardrails

**Expected Baseline Results (EXAMPLE):**

| Metric | Expected Baseline |
|--------|-------------------|
| **Correct Answers** | ~13/15 (86.7%) |
| **With Citations** | ~12/15 (80%) |
| **Avg Latency** | ~3.4 seconds |

✅ **Expected baseline should meet success criteria:**
- ✅ ≥85% factual accuracy
- ✅ ≤5s latency requirement
- ✅ High citation completeness
- ✅ Strong reliability on single-document questions

**Notes:** The baseline approach is expected to show high factuality and reliability. Performance may decrease slightly on multi-document questions where answers span conflicting sections, suggesting the value of adding hybrid retrieval or cross-document reranking for future iterations.

---

## ⚠️ Anticipated Edge Cases & Limitations

**Anticipated Edge Cases:**
- Very long PDFs exceeding embedding window
- Acronym-heavy technical passages (e.g., KG-RAG, C-RAG, RAG-Ex)
- Contradictory claims from multiple sources
- Similar paragraphs across papers (retrieval confusion)

**Identified Risks:**
- Poisoned text embedded in retrieved chunks
- Outdated or retracted papers introducing factual drift
- Adversarial prompts circumventing guardrails

---

## 🚀 Future Work

Planned improvements to enhance the system:

- **Hybrid Retrieval:** Add BM25 + dense vector combination
- **Reranking:** Integrate Cohere or ColBERT v2 for top-k reordering
- **Conformal RAG (C-RAG):** Add abstention & uncertainty quantification
- **RAG-Ex:** Implement explainable evidence mapping
- **Privacy Watermarking:** Add dataset watermarking for provenance tracking (Ward 2024)

---

## 📚 Documentation

For detailed technical documentation, see:
- **[CAPB_report.md](CAPB_report.md)** - Complete project proposal with architecture details, evaluation plan, and analysis

---

## 📖 References

Key papers informing this implementation:

- *Towards Trustworthy Retrieval-Augmented Generation for Large Language Models: A Survey.* arXiv, 2025
- *RobustRAG: Certifiably Robust Retrieval-Augmented Generation.* arXiv, 2024
- *RAG-Thief: Scalable Extraction of Private Data from Retrieval-Augmented Generation Systems.* arXiv, 2024
- *TrustRAG: Enhancing Robustness and Trustworthiness in RAG.* arXiv, 2025
- NVIDIA NeMo Guardrails. GitHub Repository, 2025

---

## 👨‍💻 Author

**Peter Dunson**  
IPHS 391 - RAG Mini-Project

---

## 🙏 Acknowledgments

ChatGPT was used for assistance with system design reasoning, tech stack selection, formatting, and citation identification.

---

<div align="center">
