# Problem Statement

## 1. Background

Human Resources teams frequently manage large and evolving policy handbooks covering topics such as leave, compensation, benefits, conduct, grievance procedures, flexible working, and more. These documents are typically long, densely written, and distributed as static PDFs that employees must manually search through.

As a result, employees struggle to:
- Interpret policies quickly
- Locate the relevant section
- Confirm whether a policy applies to their situation
- Avoid incorrect assumptions or misinformation

HR teams often receive repetitive policy-related queries that could otherwise be self-served if information were easier to access.

---

## 2. Problem

Employees and managers lack a simple way to query HR policies and get accurate answers without reading or manually searching a lengthy PDF.

Key pain points include:
- Finding relevant answers takes too long
- Search terms may not match policy language
- Employees misinterpret policies or rely on outdated information
- HR teams spend unnecessary time responding to basic, repetitive questions

There is no real-time conversational interface grounded in the company’s approved policy documentation.

---

## 3. Objective

Build an interactive AI-powered assistant that can:
1. Accept an uploaded HR policy PDF  
2. Index and understand the content  
3. Allow employees to ask natural-language questions  
4. Provide answers sourced **only** from the uploaded document  
5. Avoid hallucination or speculation  
6. Support user feedback for continuous improvement

The solution should be user-friendly, easy to run, and demonstrate applied use of Retrieval-Augmented Generation (RAG).

---

## 4. Scope of Solution

This project delivers:
- A retrieval pipeline that loads, chunks, and embeds HR policy documents
- A vector search system to find the most relevant policy sections
- A constrained LLM that answers questions using retrieved context
- A web-based interface for:
  - Conversational Q&A
  - Document upload
  - Feedback capture

The first release focuses on a **single PDF per session** and **local memory** (no cloud database).

---

## 5. Success Criteria

The solution is considered successful if:
- A user can upload a new HR policy PDF
- Ask common HR questions (e.g., leave entitlement, sick pay, etc.)
- Receive accurate answers that reference the uploaded PDF
- The system responds “I don’t know” when the answer is not present
- HR teams can review logged feedback to improve policy clarity or FAQ lists

---

## 6. Constraints / Assumptions

- The model must rely only on the policy text supplied
- No proprietary or confidential documents are committed to the repo
- Embedding and vector search run locally (ChromaDB, no external DB)
- Users must supply their own API key for Gemini access
- The tool is designed for demonstration and learning, not immediate production deployment

---

## 7. Who Benefits

- **Employees:** get instant answers without digging through PDFs  
- **HR Teams:** reduce repetitive workloads and focus on higher-value support  
- **Organizations:** improve policy clarity and consistency  

---

## 8. Future Enhancements

Beyond the course scope, the assistant could evolve to support:
- Multiple documents and policy categories
- Persistent vector databases (PostgreSQL + pgvector / Pinecone)
- Role-based access control (employee vs manager)
- Analytics on question patterns
- Language and regional variants of policies
- Integration with Microsoft Teams, Slack, or an internal portal

---

> This project forms a course-end demonstration that brings together modern AI workflows — retrieval, embeddings, generative models, vector stores, and a simple user interface — to solve a real workplace problem.
