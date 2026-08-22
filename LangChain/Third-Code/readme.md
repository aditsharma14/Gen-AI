# Third-Code — LangChain Document Loaders & Text Splitters

This module explores LangChain's document ingestion pipeline: loading data from
multiple source types into a common `Document` format, then splitting that data
into smaller chunks suitable for embedding and retrieval.

## Contents

- [code.ipynb](code.ipynb) — walkthrough notebook covering loaders and splitters
- [speech.txt](speech.txt) — sample text file (Woodrow Wilson's WWI declaration speech)
- [attention.pdf](attention.pdf) — sample PDF ("Attention Is All You Need") for PDF loading
- [requirements.txt](requirements.txt) — Python dependencies

## Topics Covered

### Document Loaders
- **TextLoader** — load plain text files (`speech.txt`)
- **PyPDFLoader** — load and parse PDF documents (`attention.pdf`)
- **WebBaseLoader** — scrape and load content from a web page
- **ArxivLoader / `arxiv` client** — fetch paper metadata and abstracts from arXiv
- **WikipediaLoader** — fetch article summaries and content from Wikipedia

### Text Splitters
- **RecursiveCharacterTextSplitter** — split loaded documents (or raw strings)
  into overlapping chunks (`chunk_size` / `chunk_overlap`) for downstream
  embedding and retrieval

## Setup

Install dependencies from the project root:

```bash
pip install -r requirements.txt
```

Some loaders (e.g. `WebBaseLoader`) may require a `USER_AGENT` environment
variable to be set to avoid warnings when scraping pages.

## Usage

Open [code.ipynb](code.ipynb) in Jupyter and run the cells in order. Each
section is self-contained and demonstrates loading documents from a different
source, followed by splitting them into chunks with `RecursiveCharacterTextSplitter`.
