## LangChain Practice Suite (Gemini, RAG, Agents, Tools)

Hands-on scripts exploring LangChain v0.2 features with Google Gemini (via `langchain-google-genai`), Chroma for vector search, prompt templates, chains (including branching/parallel), agents + tools, and basic web scraping.

### What’s inside

- 1_chat_models: minimal chat calls with Gemini
- 2_prompt_templates: templated prompts and multi-message prompts
- 3_chains: chains basics, under-the-hood, branching and parallel LCEL
- 4_rag: end-to-end RAG with text loaders, splitting, embeddings, Chroma persistence, conversational RAG, and web scraping
- 5_agents_and_tools: ReAct-style agent using tools (time example), agent deep dive


## Prerequisites

- Python 3.10+ (Windows, macOS, or Linux)
- Internet access for LLMs, embeddings, and (optional) web scraping


## Quick start (Windows PowerShell)

1) Create and activate a virtual environment

```powershell
py -m venv .venv
./.venv/Scripts/Activate.ps1
```

2) Install dependencies

```powershell
pip install -r requirements.txt
```

3) Copy env template and add your keys

```powershell
Copy-Item .env.example .env
# Then open .env and fill in GOOGLE_API_KEY, and optionally others
```

4) Run an example

```powershell
# Basic chat with Gemini
python 1_chat_models/1_chat_model_basic.py

# Prompt templates + chat model
python 2_prompt_templates/2_prompt_template_with_chat_model.py

# RAG (creates a Chroma DB from Odyssey if not present)
python 4_rag/1a_rag_basics.py

# Conversational RAG (expects an existing DB under 4_rag/db/chroma_db_with_metadata)
python 4_rag/7_rag_conversational.py

# ReAct agent using a simple Time tool
python 5_agents_and_tools/1_agent_and_tools_basics.py
```


## Environment variables (.env)

Most examples use Google Gemini via `langchain-google-genai`:

- GOOGLE_API_KEY — required for Gemini chat/embeddings

Optional keys used by specific scripts:

- OPENAI_API_KEY — required for OpenAI embeddings (e.g., `4_rag/8_rag_web_scrape_firecrawl.py`)
- FIRECRAWL_API_KEY — required for the Firecrawl web scraping example
- LANGCHAIN_TRACING_V2=true and LANGCHAIN_API_KEY or LANGSMITH_API_KEY — optional tracing/telemetry

See `.env.example` for a template.


## Notable examples and notes

- 1_chat_models/1_chat_model_basic.py
	- Minimal Gemini chat: prints both the full result object and `result.content`.

- 2_prompt_templates/2_prompt_template_with_chat_model.py
	- Demonstrates `ChatPromptTemplate` with single and multi-message prompts.

- 3_chains/4_chains_parallel.py
	- Parallel branching with LCEL (`RunnableParallel`) to produce pros/cons, then combines results.

- 4_rag/1a_rag_basics.py
	- Loads `books/odyssey.txt`, splits text, creates embeddings with `GoogleGenerativeAIEmbeddings`, and persists to Chroma under `4_rag/db/chroma_db`.
	- Safe to run repeatedly; it will skip re-initialization if the DB exists.

- 4_rag/4_rag_embedding_deep_dive.py
	- Demonstrates creating/querying different vector stores. Includes commented Hugging Face example.

- 4_rag/7_rag_conversational.py
	- History-aware retriever + QA chain for conversational RAG. Expects an existing Chroma DB at `4_rag/db/chroma_db_with_metadata`.

- 4_rag/8_rag_web_scrape_basic.py
	- Scrapes Apple.com with `WebBaseLoader`, splits, embeds (Gemini), persists to Chroma, then queries.

- 4_rag/8_rag_web_scrape_firecrawl.py
	- Uses Firecrawl to scrape, OpenAI embeddings for vectors. Requires `FIRECRAWL_API_KEY` and `OPENAI_API_KEY`.

- 5_agents_and_tools/1_agent_and_tools_basics.py
	- ReAct agent from the LangChain Hub prompt (`hwchase17/react`) with a simple time tool using Gemini.


## Data and vector stores

- Book texts for RAG live under `4_rag/books/`.
- Chroma vector stores are persisted under `4_rag/db/*` folders. Re-running scripts will reuse existing stores when present.


## Optional extras

These scripts are optional and require extra packages or keys:

- Firecrawl scraping (4_rag/8_rag_web_scrape_firecrawl.py)
	- Requires `FIRECRAWL_API_KEY`
	- Install: `pip install firecrawl-py` (or the latest Firecrawl client for Python)

- Hugging Face local embeddings (commented in 4_rag/4_rag_embedding_deep_dive.py)
	- Install: `pip install sentence-transformers`


## Troubleshooting

- ModuleNotFoundError: Ensure the virtual environment is active and `pip install -r requirements.txt` has completed.
- Missing API key errors:
	- `GOOGLE_API_KEY` for Gemini (chat and embeddings)
	- `OPENAI_API_KEY` for OpenAI embeddings scripts
	- `FIRECRAWL_API_KEY` for the Firecrawl example
- Chroma DB already exists: This is expected on re-runs; scripts skip re-initialization by design.
- Web loaders on corporate networks: Proxies or firewalls may block requests; try another network or configure proxies.


## Repository structure (abridged)

```
1_chat_models/
2_prompt_templates/
3_chains/
4_rag/
5_agents_and_tools/
```


## License

For learning/demo purposes. Add a license if distributing.

