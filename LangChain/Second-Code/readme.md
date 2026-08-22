# LangChain Document Loaders

This notebook demonstrates how to load information from different sources
into LangChain `Document` objects. These documents can later be split,
embedded, stored in a vector database, and used in a RAG application.

## Sources covered

- `speech.txt` using `TextLoader`
- `attention.pdf` using `PyPDFLoader`
- A webpage using `WebBaseLoader`
- arXiv metadata and abstracts using the arXiv API
- Wikipedia pages using `WikipediaLoader`

## Project files

```text
Second-Code/
├── attention.pdf     # Local PDF example
├── code.ipynb        # Notebook
├── readme.md         # This guide
├── records.xml       # Supporting/local data file
└── speech.txt        # Local text example
```

## Setup

Run these commands from the project root:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

Select the `.venv` Python kernel in VS Code, then open `code.ipynb` and run
the cells from top to bottom. The notebook expects `speech.txt` and
`attention.pdf` to be in the same directory as the notebook.

## What each loader does

### TextLoader

```python
loader = TextLoader("speech.txt")
text_document = loader.load()
```

Reads a local text file and returns a list of LangChain `Document` objects.
The text is available in `text_document[0].page_content`.

### PyPDFLoader

```python
loader = PyPDFLoader("attention.pdf")
docs = loader.load()
```

Reads the PDF and normally returns one `Document` per page. Page metadata can
be used to identify where extracted text came from.

### WebBaseLoader

```python
loader = WebBaseLoader(
		web_path=("https://lilianweng.github.io/posts/2023-06-23-agent/",)
)
docs = loader.load()
```

Downloads and extracts text from the configured webpage. This requires an
internet connection and may stop working if the website changes or blocks
automated requests.

### arXiv

The notebook uses the HTTPS arXiv API and retrieves metadata plus abstracts:

```python
arxiv.Client.query_url_format = "https://export.arxiv.org/api/query?{}"
search = arxiv.Search(id_list=["1605.08386"], max_results=2)
results = list(arxiv.Client().results(search))
```

Each result is converted into a LangChain `Document`. The notebook does not
download PDFs because the PDF endpoint may be unavailable on restricted
networks, and older `ArxivLoader` versions can conflict with newer package
APIs.

### WikipediaLoader

```python
docs = WikipediaLoader(
		query="Generative AI",
		load_max_docs=2,
).load()
```

Searches Wikipedia and loads up to two matching pages. This also requires an
internet connection.

## Important variables

- `loader`: the currently selected loader
- `text_document`: documents loaded from `speech.txt`
- `docs`: documents loaded from the PDF, arXiv, or Wikipedia
- `results`: raw arXiv result objects

Because `loader` and `docs` are reused, run the notebook in order. Rerunning
only a later cell may use a value created by a different loader.

## Troubleshooting

- `FileNotFoundError`: check that `speech.txt` and `attention.pdf` are beside
	the notebook, or use an absolute path.
- PDF import errors: install dependencies from `requirements.txt`, including
	`pypdf` and `pymupdf`.
- Wikipedia or webpage errors: check the internet connection and retry later.
- arXiv API errors: confirm that HTTPS access to `export.arxiv.org` is allowed.
- `ModuleNotFoundError`: select the project `.venv` kernel and reinstall the
	requirements.

## Security notes

- Do not put API keys, passwords, or tokens in this notebook.
- Do not commit `.env` files or downloaded private documents.
- Treat webpage, Wikipedia, and arXiv content as untrusted input when passing
	it to an LLM. Retrieved text can contain instructions intended to manipulate
	the model.
