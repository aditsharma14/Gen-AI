# Vector Store and Retriever

This example demonstrates how to embed documents, store them in a vector
store, and query them through a LangChain `Retriever` — the core building
block behind retrieval-augmented generation (RAG).

## What the notebook covers

- Embedding text with `HuggingFaceEmbeddings`
  (`sentence-transformers/all-MiniLM-L6-v2`)
- Indexing `Document` objects in a `Chroma` vector store
- Similarity search, including `similarity_search_with_score` and the async
  `asimilarity_search`
- Turning a vector store into a `Runnable` retriever with
  `vectorstore.as_retriever()`
- Wiring a retriever into a simple RAG chain with `ChatGroq`
- Building a `FAISS` index from a real document (`speech.txt`), split with
  `RecursiveCharacterTextSplitter`, and persisting/reloading it from disk

## How it works

```python
from langchain_huggingface import HuggingFaceEmbeddings
from langchain_chroma import Chroma

embeddings = HuggingFaceEmbeddings(model_name="sentence-transformers/all-MiniLM-L6-v2")
vectorstore = Chroma.from_documents(documents, embedding=embeddings)

retriever = vectorstore.as_retriever(search_type="similarity", search_kwargs={"k": 1})
retriever.batch(["cat", "goldfish"])
```

## Project files

- `code.ipynb` - executable examples
- `speech.txt` - sample document used to build the FAISS index
- `requirements.txt` - Python dependencies
- `.env` - holds `groq_api` / `GROQ_API_KEY` (not committed)

## Setup

From this directory, create or activate a Python environment and install the
dependencies:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
```

Open `code.ipynb` in VS Code, select the environment as the notebook kernel,
and run the cells from top to bottom.
