# Recursive JSON Splitter

This example demonstrates how to split large, nested JSON data into smaller
chunks using `RecursiveJsonSplitter` from `langchain-text-splitters`. It keeps
each chunk under a maximum size while trying to preserve the nested JSON
structure, which is useful when preparing JSON data for embeddings, vector
stores, and retrieval-augmented generation (RAG) applications.

## What the notebook covers

- Fetching a real-world nested JSON document (the LangSmith OpenAPI spec)
- Splitting it into JSON-shaped chunks with `split_json()`
- Converting the JSON chunks into LangChain `Document` objects with
  `create_documents()`
- Splitting directly into JSON-formatted strings with `split_text()`

## How it works

```python
from langchain_text_splitters import RecursiveJsonSplitter

json_splitter = RecursiveJsonSplitter(max_chunk_size=300)

# Chunks as Python dicts
json_chunks = json_splitter.split_json(json_data)

# Chunks as LangChain Document objects
docs = json_splitter.create_documents(texts=[json_data])

# Chunks as JSON-formatted strings
texts = json_splitter.split_text(json_data)
```

`max_chunk_size` caps how large each chunk can grow. The splitter walks the
JSON tree and groups nested keys/values together as long as the serialized
chunk stays under that limit, splitting further into separate chunks when it
would not.

## Project files

- `code.ipynb` - executable examples
- `requirements.txt` - Python dependencies

## Setup

From this directory, create or activate a Python environment and install the
dependencies:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
```

Open `code.ipynb` in VS Code, select the environment as the notebook kernel,
and run the cells from top to bottom. The notebook fetches
`https://api.smith.langchain.com/openapi.json` over the network, so it
requires an active internet connection.

## Example: split fetched JSON

```python
import requests
from langchain_text_splitters import RecursiveJsonSplitter

json_data = requests.get("https://api.smith.langchain.com/openapi.json").json()

json_splitter = RecursiveJsonSplitter(max_chunk_size=300)
texts = json_splitter.split_text(json_data)

print(texts[0])
```
