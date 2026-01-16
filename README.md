# HR Policy RAG Assistant 🧑‍💼🤖

A conversational HR assistant that answers questions **only** using information from an uploaded HR policy PDF.

The system uses a Retrieval-Augmented Generation (RAG) pipeline:

1. Load and chunk the HR policy PDF  
2. Create dense embeddings for each chunk using a HuggingFace `sentence-transformers` model  
3. Store embeddings in a Chroma vector database  
4. Retrieve the most relevant chunks for a user query  
5. Ask a Gemini chat model to generate an answer **grounded only in the retrieved policy text**  

The assistant is exposed via a Gradio web UI with:
- A **Chat** tab to ask HR questions
- An **Upload Document** tab to index a new HR policy PDF
- A **Feedback** tab to log user feedback for improvements :contentReference[oaicite:1]{index=1}

> This project was built as a course-end project to demonstrate end-to-end application of modern AI tooling (RAG, embeddings, LLMs, Gradio UI) on a realistic HR use case. :contentReference[oaicite:2]{index=2}

---

## 🌟 Features

- PDF-grounded Q&A (no hallucinated answers where possible)
- RAG architecture with ChromaDB vector store
- HuggingFace sentence-transformer embeddings (`all-MiniLM-L6-v2`)
- Gemini chat model for generation (via `google-generativeai`)
- Simple Gradio interface with:
  - Chat history
  - Document upload & indexing
  - Feedback capture with timestamps

---

## 🧱 Tech Stack

- **Language:** Python
- **UI:** Gradio
- **RAG / Orchestration:** LangChain (`langchain`, `langchain-community`, `langchain-google-genai`)
- **PDF Loading:** `pypdf`, `PyPDFLoader`
- **Text Splitting:** `RecursiveCharacterTextSplitter`
- **Embeddings:** HuggingFace `sentence-transformers/all-MiniLM-L6-v2`
- **Vector Store:** ChromaDB
- **LLM:** Gemini (`gemini-2.5-flash-lite`)

---
## Quickstart

```bash
git clone https://github.com/tiashamondal29/Hr-policy-rag-Gradio-assistant.git
cd Hr-policy-rag-Gradio-assistant
python -m venv .venv
source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt
export GOOGLE_API_KEY="your_key_here"
jupyter notebook Crafting_an_AI_Powered_HR_Assistant.ipynb
---
