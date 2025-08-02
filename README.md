# 📄 Project Documentation: Hybrid RAG Chatbot with Groq + LangChain + Streamlit

## 📌 Project Title:

**Hybrid Chatbot using Groq LLaMA3 and RAG (Retrieval-Augmented Generation)**

---

## 🔍 Overview

This project is a conversational AI chatbot built with:

* **Groq's LLaMA3-8B model** for general language understanding and generation.
* **LangChain** for LLM chaining, prompt management, and integration with vector databases.
* **FAISS** and **HuggingFaceEmbeddings** for document chunking and similarity search (RAG).
* **Streamlit** as the web-based UI for chatting and file upload.

It allows users to:

* Ask **general knowledge questions** (like ChatGPT).
* Upload a **PDF** and get document-based answers using retrieval.
* Maintain **chat memory** during a session.

---

## 🧠 How It Works

### 1. **User Input → Prompt Construction**

* Chat history is appended to the prompt.
* If a PDF is uploaded, top-k similar chunks (via FAISS) are retrieved.
* These chunks are added as context before the user question.

### 2. **Prompt → Groq LLaMA3-8B via LangChain**

* The constructed prompt is passed to Groq's hosted LLaMA3 model.
* The model generates a response using both pretraining knowledge and provided document context.

### 3. **Response → UI**

* The response is rendered in Streamlit using chat-style bubbles.
* Previous messages are remembered and shown above the input box.

---

## ⚙️ Tech Stack

### 🔸 Language Model

* **Groq API** with model `llama3-8b-8192`

### 🔸 Frameworks & Libraries

* `streamlit`
* `langchain`
* `groq`
* `python-dotenv`
* `PyMuPDF` (for PDF parsing)
* `faiss-cpu` (for local vector search)
* `sentence-transformers` or `transformers` (for HuggingFaceEmbeddings)

### 🔸 Other Tools

* `.env` file to store API key securely
* `tempfile` to manage uploaded files without saving permanently

---

## 🧩 Code Breakdown

### `load_llm()`

* Loads Groq API key from `.env`
* Returns `ChatGroq` LLM instance using LLaMA3

### `build_chain(llm)`

* Creates a prompt pipeline: prompt → LLM → string output

### `build_context_prompt(user_input, retriever)`

* Appends chat history and PDF context (if available) to the user input

### `chat_with_bot(chain, full_prompt)`

* Invokes the LangChain pipeline with the final composed prompt

### `main()`

Handles:

* Streamlit layout setup
* PDF upload and processing (splitting, embedding, FAISS storage)
* Chat UI and history
* Prompt construction and response display

---

## 📥 File Structure

```
HybridChatBot/
├── app.py                  # Main Streamlit app
├── .env                   # Stores GROQ_API_KEY
└── requirements.txt       # Python dependencies
```

---

## 🔐 .env Format

```
GROQ_API_KEY=your_groq_key_here
```

---

## 🧪 How to Run

1. Clone or download the project.
2. Create `.env` and add your Groq API key.
3. Install dependencies:

```bash
pip install -r requirements.txt
```

4. Run the app:

```bash
streamlit run app.py
```

---

## ✅ Features

* Conversational UI (chat-style)
* Memory of current session
* Real-time PDF upload and parsing
* Hybrid answering: general + document-specific
* Clear chat button
* Groq-powered inference (fast and cost-efficient)

---

## 🛠 Future Improvements

* Support for multiple PDFs
* Source highlighting or citations
* Persistent storage of chat history
* Use of cloud vector DB like ChromaDB or Pinecone
* Voice input/output

---

## 🧑‍💻 Author

This chatbot project was created and documented by Dinesh Halder as part of a personal exploration into combining RAG with high-performance LLMs like Groq's LLaMA3.

---

## 📎 License

This project is open for educational and personal use. Please credit the author if reused.
