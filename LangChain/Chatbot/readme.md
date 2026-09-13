# Chatbot with Message History (LangChain + Groq)

A notebook-based demo of building a conversational chatbot using LangChain's message history utilities and the Groq-hosted `openai/gpt-oss-20b` model.

## What it covers

- **Basic chat calls** — invoking a `ChatGroq` model with `HumanMessage`/`AIMessage` objects.
- **Session-based memory** — using `ChatMessageHistory` and `RunnableWithMessageHistory` to persist conversation state per `session_id`, so the model can recall earlier messages (e.g. a user's name) within the same session.
- **Prompt templates** — wrapping the model in a `ChatPromptTemplate` with a system message and a `MessagesPlaceholder`, including a variant that supports a `{language}` variable for multilingual responses.
- **Message trimming** — using `trim_messages` to cap conversation history by token count (`max_tokens`, `strategy="last"`) so long conversations stay within context limits while preserving the system message.
- **Combined pipeline** — chaining `RunnablePassthrough.assign` (to trim messages), the prompt template, and the model together, then wrapping the whole chain in `RunnableWithMessageHistory` for a stateful, language-aware chatbot.

## Files

- [code.ipynb](code.ipynb) — the main notebook with all chatbot experiments.
- [requirements.txt](requirements.txt) — Python dependencies.
- `.env` — holds the `GROQ_API_KEY` (or `groq_api`) used to authenticate with Groq.

## Setup

1. Install dependencies:
   ```
   pip install -r requirements.txt
   ```
2. Create a `.env` file in this folder (or the parent `LangChain/` folder) with:
   ```
   GROQ_API_KEY=your_key_here
   ```
3. Open [code.ipynb](code.ipynb) and run the cells in order.

## Notes

- `RunnableWithMessageHistory` is deprecated upstream in favor of LangGraph's built-in persistence — this notebook uses it for learning/demo purposes.
- Conversation history is stored in-memory (`store` dict) and is not persisted across kernel restarts.
