# Q&A Chatbot

A Retrieval-Augmented Generation (RAG) chatbot built with LangChain that answers questions about web content and supports multi-turn conversation with chat history.

## Overview

The chatbot loads a blog post, indexes it into a vector store, and uses retrieval-augmented generation to answer questions grounded in that content. It's extended with a history-aware retriever so follow-up questions ("tell me more about it?") are reformulated using prior conversation context before retrieval.

## How it works

1. **Load & chunk** — `WebBaseLoader` fetches the source page (Lilian Weng's ["LLM Powered Autonomous Agents"](https://lilianweng.github.io/posts/2023-06-23-agent/) post) and extracts the post content, title, and header via `bs4.SoupStrainer`.
2. **Embed & index** — Content is split with `RecursiveCharacterTextSplitter` (1000-char chunks, 200-char overlap) and embedded using `HuggingFaceEmbeddings` (`all-MiniLM-L6-v2`), then stored in a `Chroma` vector store.
3. **Retrieve & answer** — A `create_stuff_documents_chain` combines retrieved chunks with a concise Q&A system prompt, wired to the retriever via `create_retrieval_chain`.
4. **Chat history** — `create_history_aware_retriever` reformulates follow-up questions into standalone queries using prior turns, and `RunnableWithMessageHistory` persists per-session conversation history in memory, keyed by `session_id`.

## Tech stack

- **LLM**: Groq (`openai/gpt-oss-120b` via `langchain_groq.ChatGroq`)
- **Embeddings**: HuggingFace (`all-MiniLM-L6-v2`)
- **Vector store**: Chroma
- **Framework**: LangChain (`langchain-classic`, `langchain-community`, `langchain-text-splitters`)

## Setup

1. Create a `.env` file with your API keys:
   ```
   groq_api=<your Groq API key>
   hf_token=<your HuggingFace token>
   ```
2. Install dependencies:
   ```
   pip install -U bs4 langchain langchain-community langchain-groq langchain-huggingface langchain-chroma langchain-text-splitters langchain-classic
   ```
3. Run through [code.ipynb](code.ipynb) to build the index and start querying.

## Usage

```python
conversational_rag_chain.invoke(
    {"input": "What is Task Decomposition?"},
    config={"configurable": {"session_id": "abc123"}},
)["answer"]
```

Each `session_id` maintains its own chat history, so follow-up questions within the same session are automatically contextualized.
