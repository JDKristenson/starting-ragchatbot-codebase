# Native setup notes — inferred, NOT RUN

These notes standardize the documented path without changing the application,
manifest, lockfile, launch script, or original README instructions. No install,
build, test, application, provider call, or credential check was performed for
this cleanup. Commands below are instructions for a future operator, not results.

## Stack and prerequisites

- Python 3.13 or newer in `pyproject.toml`; `.python-version` selects 3.13.
- `uv` manages the existing `pyproject.toml` and `uv.lock`. Obtain it using the
  official [uv installation documentation](https://docs.astral.sh/uv/getting-started/installation/)
  and review installation instructions rather than blindly piping remote scripts.
- FastAPI 0.116.1, Uvicorn 0.35.0, ChromaDB 1.0.15, Anthropic SDK 0.58.2,
  sentence-transformers 5.0.0, python-dotenv 1.1.1 and python-multipart 0.0.20
  are the direct pinned dependencies. The lockfile records transitive resolution;
  installation/platform compatibility and dependency licenses are not validated here.
- Vanilla HTML/CSS/JavaScript frontend: no Node package manifest or frontend
  bundler is tracked. This is a Python web app, not a native desktop/mobile app.
- An operator-controlled Anthropic account/key is needed for generation. Model
  access, availability and billing have not been checked. The configured model is
  `claude-sonnet-4-20250514`; do not assume this dated identifier is available.
- The existing launch script expects Bash; the original README recommends Git
  Bash on Windows. Windows/macOS/Linux compatibility has not been exercised.

## Proposed local workflow

Run from the repository root unless the command says otherwise. **NOT RUN.**

1. With Python and uv available, synchronize the tracked lockfile:

   ```bash
   uv sync --locked
   ```

   This is deliberately lock-preserving: if manifest/lock compatibility fails,
   investigate rather than silently regenerating the lock. It may download
   packages/interpreters and create `.venv`; it is not an offline operation.

2. Review the tracked `.env.example` locally, then create a root `.env` containing
   your own `ANTHROPIC_API_KEY` through a private editor or secret manager. Do not
   paste keys into documentation, terminals captured in screenshots, or commits.
   `backend/config.py` uses `load_dotenv()`. Both root and backend `.env` paths
   are ignored; avoid keeping duplicate files with confusing precedence.

3. Review permissions for the input material before running. Startup scans the
   existing `docs/` corpus. No course text needs to be copied into these notes.
   The parser is for course-formatted UTF-8 text. Although the folder scanner
   accepts `.pdf` and `.docx` suffixes, there is no format-specific parser; do not
   claim general PDF/DOCX support.

4. Start using the existing wrapper, without changing its executable bit:

   ```bash
   bash run.sh
   ```

   Or use its equivalent manual path:

   ```bash
   cd backend
   uv run --locked uvicorn app:app --reload --host 127.0.0.1 --port 8000
   ```

   The wrapper runs `uv run uvicorn app:app --reload --port 8000` after changing
   into `backend`. The manual variant adds explicit loopback binding and lock
   enforcement; neither variant was run here. Relative static, corpus and database
   paths depend on the backend working directory. `main.py` is a placeholder,
   **not** the application entry point. `--reload` is development-only.

5. Intended local URLs: `http://127.0.0.1:8000/` for the interface and
   `http://127.0.0.1:8000/docs` for FastAPI documentation. Their availability is
   unverified. Stop a foreground server with Ctrl-C.

## Side effects and external boundaries

- Importing the server initializes the RAG system and persistent Chroma client.
  With the documented working directory, storage is `backend/chroma_db/`.
- The embedding function uses `all-MiniLM-L6-v2`; first use may download model
  assets. Dependency/model acquisition needs network access even though vector
  storage and embedding computation are local in the source design.
- Startup ingests the existing course corpus, skipping titles already present.
  Edited inputs may therefore need a deliberate reindexing plan. These notes do
  not instruct deletion or rebuilding of existing data.
- Query generation sends the question and available conversation history to
  Anthropic; a tool-use round trip also sends retrieved text. Calls may incur
  charges. Do not submit private material without appropriate authority and
  disclosure. The browser also loads the existing third-party Markdown script.
- Conversation history is process memory, not a persistent user account store.
  Source review is not an authentication, isolation or security assurance.

## Verification boundary

No test suite or CI workflow is tracked at the reviewed base. No smoke check,
provider request, dependency install, build, application import or platform test
was run. A future authorized runtime validation should separately record the
platform, Python/uv versions, locked install result, startup/model acquisition,
UI/API behavior and authorized provider billing boundary. Production hardening,
runtime fixes and dependency updates are outside this documentation-only change.
