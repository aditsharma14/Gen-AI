# LangChain Updated

This folder holds the newer, more advanced LangChain programs — built on top of what I learned in the [`LangChain/`](../LangChain/) folder.

## What is LangChain?

LangChain is a framework for building applications powered by large language models (LLMs). Instead of calling an LLM API directly and manually stitching together prompts, retrieval, memory, and output parsing, LangChain gives you composable building blocks — prompt templates, chat models, output parsers, retrievers, document loaders, text splitters, and chains — that can be linked together with the `|` (pipe) operator into a "Runnable" pipeline. It also plugs into vector stores and embedding models for retrieval-augmented generation (RAG), and into LangSmith for tracing/observability.

## Past work in `LangChain/`

The `LangChain/` folder was my learning ground, working through the fundamentals step by step:

- **First Code / Second-Code / Third-Code** — starting basics: setting up `.env` keys, calling models via `Ollama` (local `llama3`) and `ChatGroq` (`openai/gpt-oss-20b`, `openai/gpt-oss-120b`), building `ChatPromptTemplate`s, chaining `prompt | llm | output_parser`, and loading documents from text files, PDFs, web pages, arXiv, and Wikipedia.
- **CharacterTextSplitter / HTMLTextSplitter / JSONSplitter** — different document-chunking strategies: `RecursiveCharacterTextSplitter`, `CharacterTextSplitter`, `HTMLHeaderTextSplitter` (splitting HTML by header tags), and `RecursiveJsonSplitter` (chunking nested JSON like the LangSmith OpenAPI spec).
- **VectorRetriever** — embedding documents with `HuggingFaceEmbeddings` (`all-MiniLM-L6-v2`), storing/searching them with `Chroma` and `FAISS`, comparing `similarity_search` vs `similarity_search_with_score`, wrapping a vector store as a `Retriever`, saving/reloading a FAISS index from disk, and building a minimal RAG chain (`{"context": retriever, "question": ...} | prompt | model`).
- **Chatbot** — conversational memory using `ChatMessageHistory` + `RunnableWithMessageHistory`, keyed by `session_id` so different chats don't bleed into each other; adding a `language` variable to the prompt; and trimming long conversation history with `trim_messages` to stay within a token budget.
- **Q&AChatbot** — a full conversational RAG pipeline: scraping a blog post with `WebBaseLoader` + `bs4`, chunking with `RecursiveCharacterTextSplitter`, embedding into `Chroma`, building a `history_aware_retriever` (rewrites follow-up questions using chat history) combined with `create_stuff_documents_chain` + `create_retrieval_chain`, and wrapping the whole thing in `RunnableWithMessageHistory` for multi-turn Q&A over the document.
- **GenAI-App** — a small Streamlit app (`app.py`) that wires a local Ollama model (`gemma:2b`) behind a prompt template and text input box, with LangSmith tracing enabled via environment variables.

## What goes here

Building on those fundamentals, this folder is for more advanced patterns — things like agents, tool calling, LangGraph-based flows, multi-step/multi-agent chains, structured output, and more involved RAG pipelines.
