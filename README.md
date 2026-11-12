# Course Materials RAG System

A Retrieval-Augmented Generation (RAG) system designed to answer questions about course materials using semantic search and AI-powered responses.

## Status, provenance, and documentation

This repository is a **course-oriented learning/reference RAG application**. The
existing integrations are implemented in source; they are not evidence that all
application code or course content was independently authored by the portfolio
owner. Attribution and redistribution permissions remain separate questions.

- [Source provenance, credits, and unresolved permissions](documentation/PROVENANCE.md)
- [Native Python/FastAPI/uv setup notes](documentation/SETUP.md) — inferred from
  source, **NOT RUN** during this documentation cleanup
- [Architecture and accessible text alternative](documentation/ARCHITECTURE.md)
- [Editable Excalidraw map](documentation/architecture.excalidraw) and
  [static SVG preview](documentation/architecture.svg)
- [Local-work ledger and verification limits](documentation/WORK_LOG.md)

The original setup instructions below are retained. They describe intended use,
not a verified build or production-readiness claim. Application startup can
create local storage and download embedding-model assets; queries can send text
to Anthropic and incur charges. Review the setup notes before running anything.

## Overview

This application is a full-stack web application that enables users to query course materials and receive intelligent, context-aware responses. It uses ChromaDB for vector storage, Anthropic's Claude for AI generation, and provides a web interface for interaction.


## Prerequisites

- Python 3.13 or higher
- uv (Python package manager)
- An Anthropic API key (for Claude AI)
- **For Windows**: Use Git Bash to run the application commands - [Download Git for Windows](https://git-scm.com/downloads/win)

## Installation

1. **Install uv** (if not already installed)
   ```bash
   curl -LsSf https://astral.sh/uv/install.sh | sh
   ```

2. **Install Python dependencies**
   ```bash
   uv sync
   ```

3. **Set up environment variables**
   
   Create a `.env` file in the root directory:
   ```bash
   ANTHROPIC_API_KEY=your_anthropic_api_key_here
   ```

## Running the Application

### Quick Start

Use the provided shell script:
```bash
chmod +x run.sh
./run.sh
```

### Manual Start

```bash
cd backend
uv run uvicorn app:app --reload --port 8000
```

The application will be available at:
- Web Interface: `http://localhost:8000`
- API Documentation: `http://localhost:8000/docs`

