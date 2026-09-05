# RAG-Based-Chatbot
A Retrieval-Augmented Generation (RAG) pipeline that answers Python programming questions — including debugging broken code — by grounding an LLM's responses in official documentation and trusted tutorials.

How it works
Ingest — Scrapes Python error/exception documentation from official docs, W3Schools, and Real Python using WebBaseLoader.
Chunk — Splits the scraped pages into overlapping chunks with RecursiveCharacterTextSplitter for better retrieval granularity.
Embed & Store — Converts chunks into vector embeddings using the free, local sentence-transformers/all-MiniLM-L6-v2 model and stores them in a persistent Chroma vector database (./chroma_db), so ingestion only needs to run once.
Retrieve & Generate — On each question, retrieves the top-k most relevant chunks and passes them as context to an LLM (via the Groq API, currently openai/gpt-oss-20b) to generate a grounded answer.
Features
General Q&A — Ask conceptual questions about Python errors and exception handling.
Debug-my-code mode — Paste a broken code snippet and the assistant identifies and explains the fix, using the same retrieval pipeline.
Persistent vector store — Embeddings are saved to disk, so you don't need to re-scrape and re-embed every session.
Tech Stack
Component	Tool
Orchestration	LangChain
Document loading	WebBaseLoader
Text splitting	RecursiveCharacterTextSplitter
Embeddings	HuggingFace sentence-transformers/all-MiniLM-L6-v2
Vector store	ChromaDB
LLM inference	Groq API
Setup
1. Clone the repo
bash
git clone https://github.com/yourusername/your-repo.git
cd your-repo
2. Install dependencies
bash
pip install langchain_community langchainhub chromadb langchain sentence-transformers groq
3. Get a Groq API key

Sign up at console.groq.com and generate a free API key.

4. Run the notebook

Open Rag_application.ipynb in Jupyter and run the cells top to bottom. You'll be prompted to enter your Groq API key via getpass — it is never hardcoded in the notebook.

python
os.environ["GROQ_API_KEY"] = getpass.getpass("Enter Groq API Key: ")
5. Ask questions
python
response = ask_rag("What are the different types of errors in python?")
print(response)

Or debug code directly:

python
response = ask_rag(f"Find and fix the bug in this code:\n\n{buggy_code}")
print(response)
Notes
The Chroma database (chroma_db/) is excluded from version control via .gitignore — each user builds their own locally by running the ingestion cells once.
The LLM model name may need updating over time as Groq deprecates older models; check Groq's model list if you hit a model_not_found error.
Never commit API keys. Use environment variables or getpass as shown above.
Future Improvements
 Expand the knowledge base beyond Python error handling to broader language topics
 Add a simple UI (Streamlit) for non-notebook use
 Support multi-turn conversation with memory
 Add source citations to responses (which doc a chunk came from)
