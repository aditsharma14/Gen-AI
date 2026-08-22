# HTML Header Text Splitter

This example demonstrates how to split HTML content into LangChain `Document`
objects while preserving the document's heading structure. It uses
`HTMLHeaderTextSplitter` from `langchain-text-splitters`.

## What the notebook covers

- Splitting an HTML string held in memory
- Tracking `h1`, `h2`, and `h3` headings as document metadata
- Inspecting the resulting `Document` objects
- Downloading and splitting an HTML page directly from a URL
- Handling headings through `h4` for a deeper page structure

The sample HTML contains this hierarchy:

```text
Foo
|- Bar main section
|  |- Bar subsection 1
|  `- Bar subsection 2
`- Baz
```

## How it works

Headings are configured as pairs containing the HTML tag and the metadata key:

```python
from langchain_text_splitters import HTMLHeaderTextSplitter

headers_to_split_on = [
	("h1", "Header 1"),
	("h2", "Header 2"),
	("h3", "Header 3"),
]

html_splitter = HTMLHeaderTextSplitter(headers_to_split_on)
documents = html_splitter.split_text(html_string)
```

Each returned `Document` contains the text for a section and metadata that
identifies its parent headings. Keeping this metadata makes the chunks easier
to filter, cite, and display in retrieval-augmented generation applications.

## Split a web page

The notebook also loads the Stanford Encyclopedia of Philosophy page about
Godel's incompleteness theorems:

```python
url = "https://plato.stanford.edu/entries/goedel/"

headers_to_split_on = [
	("h1", "Header 1"),
	("h2", "Header 2"),
	("h3", "Header 3"),
	("h4", "Header 4"),
]

html_splitter = HTMLHeaderTextSplitter(headers_to_split_on)
documents = html_splitter.split_text_from_url(url)
```

## Setup and usage

Install the required package in the project environment:

```powershell
python -m pip install langchain-text-splitters
```

Open `code.ipynb` in VS Code, select the environment containing the package as
the notebook kernel, and run the cells from top to bottom. The URL example
requires an active internet connection.

## Important note

`HTMLHeaderTextSplitter` is intended for heading-aware splitting. For a second
pass that enforces a maximum chunk size, combine the resulting documents with a
size-based splitter such as `RecursiveCharacterTextSplitter`.
