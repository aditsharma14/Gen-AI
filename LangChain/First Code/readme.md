# First LangChain Example

This notebook demonstrates a basic LangChain workflow using a local Ollama
model. It covers direct model invocation, prompt templates, LangChain
expression language, and string output parsing.

## Prerequisites

- Python 3.10 or later
- [Ollama](https://ollama.com/) installed and running
- The `llama3` model downloaded locally

Install and prepare the Python environment from the project root:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
python -m ipykernel install --user --name langchain-first-code
```

Download the model once with Ollama:

```powershell
ollama pull llama3
```

## Environment variables

Create a `.env` file in the project root if you want LangSmith tracing:

```env
OPENAI_API_KEY=your-openai-api-key
LANGCHAIN_API_KEY=your-langsmith-api-key
LANGCHAIN_PROJECT=first-langchain-example
```

The notebook loads these values with `python-dotenv`. The model itself runs
locally through Ollama; the OpenAI key is not used for the `llama3` calls.

## Notebook walkthrough

1. Load environment variables and configure LangSmith tracing.
2. Create an `Ollama` language model using `llama3`.
3. Invoke the model directly with a question about RAG and generative AI.
4. Build a reusable `ChatPromptTemplate` with system and user messages.
5. Compose the prompt and model with the `|` operator.
6. Add `StrOutputParser` to return the model response as a plain string.

Open `code.ipynb`, select the `langchain-first-code` kernel, and run the cells
in order. Keep Ollama running while executing the notebook.

## Troubleshooting

- `connection refused`: start Ollama and try again.
- `model not found`: run `ollama pull llama3`.
- missing Python imports: activate the virtual environment and rerun
	`pip install -r requirements.txt`.
- no LangSmith traces: verify `LANGCHAIN_API_KEY` and the project name in
	`.env`, then rerun the environment setup cell.
