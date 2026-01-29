# cs5588-week2-Applied RAG for Product & Venture Development
Student: Ailing Nan
Date: 01/29/2026

---

# Product Name: LegalClause Finder: Applied RAG for Legal Clause Search & Verification

## 1. Product Overview
### Problem Statement
Legal documents such as statutes, regulations, and codes are long, complex, and difficult for non-experts to search accurately. Users often struggle to locate the exact legal clauses relevant to their questions, and generic AI chatbots may hallucinate or paraphrase legal language incorrectly, which can lead to serious misunderstandings.

### Target Users
- General public seeking to understand legal texts
- Students studying law or public policy
- Compliance analysts or non-legal professionals who need to reference legal clauses

### Value Proposition
LegalClause Finder helps users locate and cite authoritative legal clauses directly from source documents. Instead of providing legal advice, the system focuses on accurate retrieval, explicit citations, and safe abstention when evidence is insufficient, reducing the risk of misinformation compared to generic AI assistants.

### Why AI + RAG Is Needed
Legal questions require exact grounding in authoritative documents. A standalone language model may fabricate or misinterpret clauses, while keyword search alone often fails due to complex legal language. Retrieval-Augmented Generation (RAG) enables the system to combine precise document retrieval with natural language explanations, while ensuring answers are evidence-based.

### If the System Fails, Who Gets Hurt?
If the system provides incorrect or hallucinated legal information, users may misunderstand their rights or obligations, potentially leading to legal, financial, or compliance-related harm.

---

## 2. Dataset Reality
### Data Source & Ownership
- Source: Publicly available legal documents (e.g., statutes, regulations, legal codes)
- Owner: Government or public institutions

### Sensitivity & Ethics
- Sensitivity level: Public but high-stakes
- Ethical Considerations: Although the data is public, legal text must be interpreted carefully. The system must avoid presenting itself as legal advice and should clearly communicate uncertainty when applicable.

### Document Types
- Statutory texts
- Regulatory documents
- Legal codes and sections

### Expected Scale in Production
In a real deployment, the system could manage hundreds to thousands of legal documents, depending on jurisdiction and domain scope.

---

## 3. User Stories & Risk Awareness

### U1 — Normal Use Case
> As a user, I want to find the relevant legal clause for a specific question so that I can understand the applicable law.

**Acceptable Evidence:**  
**Correct Answer Criteria:**  

### U2 — High-Stakes Case
> As a ___, I want to ___ so that I can ___.

**Why This Is High Risk:**  
**Acceptable Evidence:**  
**Correct Answer Criteria:**  

### U3 — Ambiguous / Failure Case
> As a ___, I want to ___ so that I can ___.

**What Could Go Wrong:**  
**Safeguard Needed:**  

---

## 4. System Architecture (Product View)

### Chunking Strategy
- Fixed or Semantic:
- Chunk size / overlap:
- Why this fits your product users:

### Retrieval Design
- Keyword layer (TF-IDF / BM25):
- Vector layer (embedding model + index):
- Hybrid α value(s):

### Governance Layer
- Re-ranking method (Cross-Encoder / LLM Judge / None):
- What risk this layer reduces:

### Generation Layer
- Model used:
- Grounding & citation strategy:
- Abstention policy (“Not enough evidence” behavior):

---

## 5. Results

| User Story | Method (Keyword / Vector / Hybrid) | Precision@5 | Recall@10 | Trust Score (1–5) | Confidence Score (1–5) |
|------------|-----------------------------------|-------------|-----------|-------------------|-------------------------|
| U1         |                                   |             |           |                   |                         |
| U2         |                                   |             |           |                   |                         |
| U3         |                                   |             |           |                   |                         |

---

## 6. Failure Case & Venture Fix

### Observed Failure
Describe one real failure you observed in your system.

### Real-World Consequence
What could happen if this system were deployed as-is? (legal, financial, ethical, safety, trust, etc.)

### Proposed System-Level Fix
What would you change next?
- Data
- Chunking
- Hybrid α
- Re-ranking
- Human-in-the-loop review

---

## 7. Evidence of Grounding

Paste one **RAG-grounded answer** below with citations.

> **Answer:**  
>  
> **Citations:** [Chunk 1], [Chunk 2]

---

## 8. Reflection (3–5 Sentences)
What did you learn about the difference between **building a model** and **building a trustworthy product**?

---

## Reproducibility Checklist
- [ ] Project dataset included or linked
- [ ] Notebook runs end-to-end
- [ ] User stories + rubric completed
- [ ] Results table filled
- [ ] Screenshots or logs included

---

> *CS 5588 — UMKC School of Science & Engineering*  
> *“We are not building models. We are building products people can trust.”*
