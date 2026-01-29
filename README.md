# cs5588-week2-Applied RAG for Product & Venture Development
Student: Ailing Nan
Date: 01/29/2026

---

# Product Name: LegalClause Finder: Missouri Landlord–Tenant Law Clause Search & Verification

## 1. Product Overview

### Problem Statement
Missouri landlord–tenant laws are distributed across lengthy statutes and legal codes that are difficult for non-experts to navigate. Tenants and landlords often struggle to locate the exact legal clauses governing issues such as eviction notices, security deposits, and tenant rights. Generic AI chatbots may hallucinate, paraphrase inaccurately, or apply laws from the wrong jurisdiction, which can lead to serious legal misunderstandings.

### Target Users
- Missouri tenants seeking to understand housing-related legal obligations
- Missouri landlords needing to reference statutory requirements
- Students studying housing policy or state law
- Non-legal professionals requiring accurate citation of Missouri landlord–tenant statutes

### Value Proposition
LegalClause Finder helps users locate, verify, and cite authoritative Missouri landlord–tenant legal clauses directly from official statutory sources. Rather than offering legal advice, the system focuses on precise clause-level retrieval, explicit citations, and safe abstention when legal applicability is unclear, reducing the risk of misinformation compared to generic AI assistants.

### Why AI + RAG Is Needed
Legal questions require jurisdiction-specific grounding in authoritative legal texts. A standalone language model may fabricate clauses or incorrectly generalize laws across states, while keyword search alone often fails due to complex statutory language. Retrieval-Augmented Generation (RAG) enables accurate retrieval of Missouri-specific legal clauses and supports natural language explanations grounded strictly in source documents.

### If the System Fails, Who Gets Hurt?
If the system provides incorrect or hallucinated Missouri landlord–tenant legal information, users may misunderstand their legal rights or obligations, potentially resulting in unlawful eviction actions, financial penalties, or legal disputes.

---

## 2. Dataset Reality

### Data Source & Ownership
- Source: Publicly available Missouri statutes related to landlord–tenant law
- Owner: State of Missouri (government-published legal documents)

### Sensitivity & Ethics
- Sensitivity level: Public but high-stakes
- Ethical Considerations: Although Missouri statutes are publicly accessible, incorrect interpretation or overconfident summarization may cause legal harm. The system must clearly avoid presenting legal advice and must communicate uncertainty when evidence is insufficient.

### Document Types
- Missouri statutory texts
- Landlord–tenant law sections and clauses
- Official legal code documents

### Expected Scale in Production
In a realistic deployment, the system would manage hundreds of Missouri landlord–tenant legal clauses, maintaining a controlled and jurisdiction-specific dataset.

---

## 3. User Stories & Risk Awareness

### U1 — Normal Use Case
> As a Missouri tenant, I want to find the relevant legal clause related to housing requirements so that I can understand my rights and responsibilities.

**Acceptable Evidence:**  
Direct citation of the relevant Missouri statutory clause
**Correct Answer Criteria:**  
The clause is accurately retrieved, clearly cited, and jurisdiction-specific.

### U2 — High-Stakes Case
> As a Missouri tenant, I want to verify eviction notice requirements so that I can avoid unlawful eviction or legal disputes.

**Why This Is High Risk:**  
Incorrect information could result in illegal eviction actions or legal consequences.
**Acceptable Evidence:**  
Official Missouri landlord–tenant statutory language
**Correct Answer Criteria:**  
Only information explicitly supported by Missouri legal statutes is presented.

### U3 — Ambiguous / Failure Case
> As a user, I want to know whether a Missouri landlord–tenant legal clause applies to my specific situation when statutory language is unclear or conditional.

**What Could Go Wrong:**  
The system may overgeneralize legal applicability or provide misleading certainty.
**Safeguard Needed:**  
The system must abstain and clearly respond with “Not enough evidence”, recommending consultation with official Missouri legal resources or a qualified legal professional.

---

## 4-1. Trust & Risk Mapping

| Risk                 | Example Failure                | Real-World Consequence            | Safeguard Idea                                 |
| -------------------- | ------------------------------ | --------------------------------- | ---------------------------------------------- |
| Hallucination        | Invented Missouri legal clause | Legal or financial harm           | Force citation from official Missouri statutes |
| Misinterpretation    | Overconfident paraphrasing     | User takes incorrect legal action | Restrict answers to retrieved clauses          |
| Outdated Information | Superseded statute retrieved   | Non-compliance with current law   | Source validation and statute date checks      |
| Overreach            | System provides legal advice   | Ethical and legal liability       | Explicit disclaimer and abstention policy      |

---

## 4-2. System Architecture (Product View)

### Chunking Strategy
- Fixed or Semantic: Semantic, clause-level chunking
- Chunk size / overlap: One Missouri landlord–tenant legal clause per chunk, including statute and section metadata (no overlap)
- Why this fits your product users: Missouri statutes are structured around sections and clauses. Clause-level chunking ensures precise, citable, and jurisdiction-specific retrieval, minimizing ambiguity and reducing hallucination risk.

### Retrieval Design
- Keyword layer (TF-IDF / BM25): BM25
- Vector layer (embedding model + index): Sentence-level embeddings with FAISS index
- Hybrid α value(s): α = 0.5 (balanced keyword and semantic retrieval)
**Design rationale:**
Keyword retrieval captures exact statutory terms, while vector retrieval supports semantically phrased user queries. Hybrid retrieval balances precision and recall, reducing the risk of missing relevant Missouri landlord–tenant clauses due to phrasing differences.

### Governance Layer
- Re-ranking method: Cross-encoder re-ranking
- What risk this layer reduces: This layer reduces hallucination and jurisdiction mismatch by prioritizing Missouri-specific legal clauses that are most semantically aligned with the user query.

### Generation Layer
- Model used: Large Language Model (LLM) used strictly for evidence-grounded synthesis
- Grounding & citation strategy: The LLM is constrained to generate answers solely from retrieved and re-ranked Missouri landlord–tenant clauses, with explicit clause-level citations.
- Abstention policy (“Not enough evidence” behavior): If retrieved evidence is insufficient, outdated, or ambiguous, the system explicitly responds with “Not enough evidence” and advises users to consult official Missouri legal resources or a qualified legal professional.
---

## 5. Results
### How we measured:
- Precision@5 / Recall@10 were computed using three user-story queries with a small, manually-labeled relevant set of clause IDs (demo dataset has 5 clauses).
- Trust Score and Decision Confidence Score were collected via lightweight human review (self-rating using the rubric: citations present, relevance of evidence, and whether the system abstains appropriately when evidence is insufficient).

| User Story                       | Method (Keyword / Vector / Hybrid)                  | Precision@5 | Recall@10 | Trust Score (1–5) | Confidence Score (1–5) |
| -------------------------------- | --------------------------------------------------- | ----------- | --------- | ----------------- | ---------------------- |
| U1 (Security deposit limit)      | Hybrid (α=0.5) + Cross-Encoder Re-rank              | 0.40        | 1.00      | 4                 | 4                      |
| U2 (Notice to terminate tenancy) | Hybrid (α=0.5) + Cross-Encoder Re-rank              | 0.20        | 1.00      | 4                 | 3                      |
| U3 (Mid-lease rent increase)     | Hybrid (α=0.5) + Abstention (“Not enough evidence”) | 0.00        | 0.00      | 5                 | 2                      |


---

## 6. Failure Case & Venture Fix

### Observed Failure
For the query “How much notice must a landlord give to terminate a tenancy in Missouri?”, the BM25 and Hybrid (α=0.5) retrieval ranked mo-441.030 (termination for nonpayment) above the more relevant mo-441.020 (30-day written notice). This happened because keyword-heavy retrieval over-weighted overlapping terms like “terminate” and “notice,” causing a misleading top result.

### Real-World Consequence
If deployed as-is, a tenant or landlord could rely on the wrong statute and misunderstand notice requirements, potentially leading to unlawful termination actions, legal disputes, financial losses, and reduced trust in the product. In legal contexts, confident but incorrect retrieval is a high-stakes failure mode.

### Proposed System-Level Fix
- Governance / Re-ranking: Keep the cross-encoder re-ranking layer (already implemented) to promote the truly relevant clause; it successfully moved mo-441.020 to the top.
- Hybrid α tuning: Reduce keyword dominance by shifting α lower (more weight on vector similarity) for natural-language questions.
- Data improvements: Expand Missouri landlord–tenant clause coverage (more sections and edge cases) to improve precision and reduce “accidental matches.”
- Evidence filtering: Limit displayed evidence to the top-1 (or apply a re-rank threshold) to avoid showing unrelated clauses that confuse users.
- Human-in-the-loop (high stakes): For eviction/termination queries, add a review escalation path (or stronger abstention) when evidence confidence is low.

---

## 7. Evidence of Grounding
### Query:How much notice must a landlord give to terminate a tenancy in Missouri?
Answer:
Based on Missouri landlord–tenant statutes, a landlord may terminate a tenancy at will or by sufferance by giving the tenant written notice at least thirty days prior to the termination date.
Citations: 
[mo-441.020]

---

## 8. Reflection (3–5 Sentences)
This hands-on showed that building a trustworthy product is not just about generating fluent text—it requires retrieving the correct evidence and controlling model behavior. Keyword retrieval and even hybrid retrieval can still mis-rank clauses due to overlapping legal terms, so a governance layer (re-ranking) is needed to reduce dangerous mistakes. I also learned that citations and showing statute text inline improve transparency, while an abstention policy (“Not enough evidence”) is essential for high-stakes or uncovered questions. Overall, RAG product quality depends on dataset scope, retrieval design, and safety policies—not only the LLM.

---

## Reproducibility Checklist
- [✅] Project dataset included or linked
- [✅] Notebook runs end-to-end
- [✅] User stories + rubric completed
- [✅] Results table filled
- [✅] Screenshots or logs included

---

> *CS 5588 — UMKC School of Science & Engineering*  
> *“We are not building models. We are building products people can trust.”*
