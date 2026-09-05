# RAG-Based Chatbot for Python Debugging

A Retrieval-Augmented Generation (RAG) chatbot that answers Python programming questions and debugs broken code by grounding LLM responses in official Python documentation and trusted educational resources.

## Overview

This project implements a complete RAG pipeline using **LangChain**, **ChromaDB**, **Hugging Face embeddings**, and the **Groq API**. Instead of relying only on an LLM's internal knowledge, the chatbot first retrieves relevant documentation and then generates context-aware, grounded responses.

The knowledge base is built from:

* Official Python Documentation
* W3Schools Python Tutorials
* Real Python Articles

## Features

* **Python Q&A** — Ask conceptual questions about errors, exceptions, and debugging.
* **Debug My Code** — Paste faulty Python code and receive an explanation with the corrected approach.
* **Retrieval-Augmented Generation** — Responses are generated using retrieved documentation rather than pure LLM memory.
* **Persistent Vector Database** — Documents are embedded once and stored locally using ChromaDB for faster future queries.

## RAG Pipeline

1. **Ingest** — Scrape Python documentation using `WebBaseLoader`.
2. **Chunk** — Split documents into overlapping text chunks with `RecursiveCharacterTextSplitter`.
3. **Embed** — Generate embeddings using `sentence-transformers/all-MiniLM-L6-v2`.
4. **Store** — Save embeddings in a persistent Chroma vector database.
5. **Retrieve** — Fetch the most relevant document chunks for a user's query.
6. **Generate** — Provide a grounded response using Groq's LLM.

## Tech Stack

| Component       | Tool                            |
| --------------- | ------------------------------- |
| Framework       | LangChain                       |
| Document Loader | WebBaseLoader                   |
| Text Splitter   | RecursiveCharacterTextSplitter  |
| Embeddings      | Hugging Face `all-MiniLM-L6-v2` |
| Vector Database | ChromaDB                        |
| LLM             | Groq API (`openai/gpt-oss-20b`) |
| Environment     | Jupyter Notebook                |

## Project Structure

```text
RAG-Based-Chatbot/
│── Rag_application.ipynb
│── README.md
│── .gitignore
└── chroma_db/      # generated locally (not tracked)
```

## Installation

### 1. Clone the repository

```bash
git clone https://github.com/pavithra335/RAG-Based-Chatbot.git
cd RAG-Based-Chatbot
```

### 2. Install dependencies

```bash
pip install langchain langchain-community chromadb sentence-transformers groq
```

### 3. Configure your API key

Create a free API key from **Groq Console**.

The notebook securely requests it using:

```python
import os, getpass

os.environ["GROQ_API_KEY"] = getpass.getpass("Enter Groq API Key: ")
```

> Your API key is never stored in the notebook.

## Usage

Open **`Rag_application.ipynb`** in Jupyter Notebook and run the cells from top to bottom.

### Ask a question

```python
response = ask_rag(
    "What are the different types of errors in Python?"
)

print(response)
```

### Debug broken code

```python
response = ask_rag(
    f"Find and fix the bug in this code:\n\n{buggy_code}"
)

print(response)
```

## Example Workflow

| User Query              | Retrieved Source        | LLM Output                 |
| ----------------------- | ----------------------- | -------------------------- |
| What is IndexError?     | Python Docs             | Explanation + examples     |
| Why is my loop failing? | Real Python             | Debugging guidance         |
| Fix this exception      | Python Docs + W3Schools | Corrected code + reasoning |

## Notes

* The `chroma_db/` folder is excluded through `.gitignore`.
* Run the ingestion cells once to build your local vector database.
* If Groq updates model availability, replace the model name with a supported one.
* Never commit API keys to GitHub.

## Future Improvements

* Expand the knowledge base beyond Python exceptions
* Build a Streamlit web interface
* Add conversational memory for multi-turn interactions
* Display source citations for retrieved documents

## Author

**Pavithra**

This project was built as part of my learning journey in **Retrieval-Augmented Generation (RAG)** and **LLM application development**.

