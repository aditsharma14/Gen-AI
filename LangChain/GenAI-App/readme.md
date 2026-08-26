# GenAI-App

A simple LangChain demo app that sends user questions to a locally-hosted LLM through [Ollama](https://ollama.com/) and streams the answer back through a Streamlit chat UI.

## How it works

1. A `ChatPromptTemplate` wraps the user's question with a system instruction ("Please respond to the question asked").
2. The prompt is piped into an `OllamaLLM` (running a local model such as `gemma:2b`, swappable for `llama2`) via LangChain's LCEL syntax: `prompt | llm | output_parser`.
3. `StrOutputParser` converts the model's response into a plain string.
4. Streamlit (`st.title`, `st.text_input`) provides the front-end for entering a question and displaying the response.

## Requirements

- Python 3.10+
- [Ollama](https://ollama.com/) installed and running locally, with the model pulled, e.g.:
  ```bash
  ollama pull gemma:2b
  ```
- Python packages:
  ```bash
  pip install langchain langchain-community langchain-ollama streamlit python-dotenv
  ```

## Setup

1. Create a `.env` file in this directory for any required environment variables (e.g. LangSmith tracing keys).
2. Activate the project virtual environment (`.venv`).
3. Make sure Ollama is running (`ollama serve`) and the target model is pulled.

## Usage

The core logic is in [code.ipynb](code.ipynb). To run it as a Streamlit app, extract the Streamlit + chain cells into a `.py` file (e.g. `app.py`) and run:

```bash
streamlit run app.py
```

Then open the local URL Streamlit prints, type a question, and view the model's response.

## Notes

- The notebook contains an earlier example using `langchain_community.llms.Ollama`, which is deprecated in favor of `langchain_ollama.OllamaLLM` (used in the final chain).
- Swap the `model=` argument in `OllamaLLM(...)` to use any model available in your local Ollama installation.
