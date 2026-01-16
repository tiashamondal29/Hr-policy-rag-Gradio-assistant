# Technical Overview

## 1. Project goal

The HR Policy RAG Assistant is a conversational app that answers questions **only using information from an uploaded HR policy PDF**. It uses a Retrieval-Augmented Generation (RAG) pipeline so that the LLM’s responses are grounded in the actual document instead of relying on its own “memory”.

---

## 2. High-level architecture

1. **Document ingestion** – user uploads an HR policy PDF.
2. **Pre-processing & chunking** – PDF is split into overlapping text chunks.
3. **Embeddings & vector store** – each chunk is embedded and stored in a vector database.
4. **Retrieval** – user question is converted to an embedding and the most similar chunks are retrieved.
5. **LLM generation (RAG)** – retrieved chunks are passed to a Gemini model with an instruction prompt to generate a grounded answer.
6. **UI layer** – a Gradio interface exposes:
   - Chat with the assistant  
   - Upload new policy documents  
   - Provide feedback on answers

Everything is orchestrated in a single notebook: `Crafting_an_AI_Powered_HR_Assistant.ipynb`.

---

## 3. Core components

### 3.1 PDF ingestion & text splitting

- **Loader**: a PDF loader (e.g. `PyPDFLoader` / `pypdf`) reads the uploaded HR policy.
- **Text splitter**: a `RecursiveCharacterTextSplitter` breaks the document into chunks, e.g.:

  - `chunk_size ≈ 1000` characters  
  - `chunk_overlap ≈ 200` characters  

This overlap helps preserve context across chunk boundaries so answers don’t miss important lines.

---

### 3.2 Embeddings & vector store

- **Embedding model**: HuggingFace `sentence-transformers/all-MiniLM-L6-v2`.
- **Vector store**: ChromaDB.

For each chunk:

1. The raw text is converted into a dense vector embedding.
2. The vector and original text are stored in Chroma so that they can be searched by semantic similarity.

---

### 3.3 Retrieval

At query time:

1. The user’s question is embedded using the same sentence-transformer model.
2. A similarity search is performed against the Chroma vector store.
3. The top-k most relevant chunks (e.g. k = 4–6) are selected as **context** for the LLM.

This retrieval step is what makes the system “RAG” instead of plain chat.

---

### 3.4 LLM: Gemini with RAG prompt

- **Model**: Gemini via `google-generativeai` and `langchain-google-genai`.
- The retriever’s chunks are formatted into a prompt template that:

  - Shows the extracted HR policy context.
  - Instructs the model to **only** answer using that context.
  - Asks it to say *“I don’t know”* if the answer is not present in the document.

LangChain is used to wire these pieces together into a `RetrievalQA` / similar chain that handles retrieval + generation in a single call.

---

### 3.5 Gradio UI

The application is exposed through a Gradio Blocks UI with three main areas:

1. **Chat tab**
   - Text box for user questions.
   - Chat history display.
   - Calls the RAG chain under the hood.

2. **Upload Document tab**
   - File upload component for HR policy PDFs.
   - Triggers the ingestion + chunking + embedding pipeline and rebuilds the vector store.

3. **Feedback tab**
   - Simple form where users can rate answers or leave comments.
   - Feedback is appended to a log file (e.g. `feedback_log.txt`) with a timestamp for later analysis.

---

## 4. Data flow summary

1. User uploads `policy.pdf` → app loads and splits it into chunks.
2. Each chunk → embedding → stored in Chroma vector store.
3. User asks a question in the Chat tab.
4. Question → embedding → similarity search in Chroma → top-k chunks returned.
5. Chunks + question are combined into a RAG prompt.
6. Gemini generates an answer based only on this context.
7. Answer is shown in the Gradio chat window; user may provide feedback.

---

## 5. Configuration & environment

Key configuration is handled via environment variables:

- `GOOGLE_API_KEY` – Gemini API key (must be set by the user).

The notebook is designed to run locally or in Colab. For deployment, the same logic can be wrapped in a script (e.g. `app.py`) using:

```python
import os
port = int(os.environ.get("PORT", 7860))
demo.launch(server_name="0.0.0.0", server_port=port)
