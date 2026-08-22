# Character Text Splitter

This example demonstrates how to load documents with LangChain and split their
content into smaller chunks using `CharacterTextSplitter`. Text chunks are
useful when preparing documents for embeddings, vector stores, and retrieval-
augmented generation (RAG) applications.

## What the notebook covers

- Loading a local text file with `TextLoader`
- Loading a PDF with `PyPDFLoader`
- Loading a web page with `WebBaseLoader`
- Loading arXiv results as LangChain `Document` objects
- Loading Wikipedia pages with `WikipediaLoader`
- Splitting existing `Document` objects with `split_documents()`
- Splitting a plain string with `create_documents()`

The notebook uses these splitter configurations:

```python
CharacterTextSplitter(
	separator="\n\n",
	chunk_size=600,
	chunk_overlap=50,
)
```

For the speech excerpt, it also demonstrates smaller chunks:

```python
CharacterTextSplitter(
	separator="\n\n",
	chunk_size=100,
	chunk_overlap=20,
)
```

`separator` determines where the text is split, `chunk_size` limits the target
size of each chunk, and `chunk_overlap` preserves context between neighboring
chunks.

## Project files

- `code.ipynb` - executable examples
- `speech.txt` - local text used with `TextLoader` and `create_documents()`
- `attention.pdf` - local PDF used with `PyPDFLoader`
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
and run the cells from top to bottom. Run the notebook with its working
directory set to this folder so `speech.txt` and `attention.pdf` can be found.

The web, arXiv, and Wikipedia examples require an internet connection. The
first cell that loads the PDF also requires `attention.pdf` to be present in
this directory.

## Example: split a string

```python
from langchain_text_splitters import CharacterTextSplitter

with open("speech.txt", encoding="utf-8") as file:
	speech = file.read()

splitter = CharacterTextSplitter(
	separator="\n\n",
	chunk_size=100,
	chunk_overlap=20,
)
chunks = splitter.create_documents([speech])

print(chunks[0])
```
