# RAGDB

[![Python 3.11](https://img.shields.io/badge/python-3.11-3776AB?logo=python&logoColor=white)](https://www.python.org/downloads/release/python-3110/) [![PySide6](https://img.shields.io/badge/UI-PySide6-41CD52?logo=qt&logoColor=white)](https://doc.qt.io/qtforpython/) [![License: MIT](https://img.shields.io/badge/license-MIT-yellow.svg)](LICENSE) [![Latest release](https://img.shields.io/github/v/release/Qorq-byte/RAG-desktop?display_name=tag)](https://github.com/Qorq-byte/RAG-desktop/releases) [![Windows](https://img.shields.io/badge/platform-Windows%20x64-0078D6?logo=windows&logoColor=white)](https://github.com/Qorq-byte/RAG-desktop/releases)

**A local-first desktop RAG knowledge base for importing, searching, citing, and learning from your own documents.**

[简体中文](README.zh-CN.md) · [User guide](docs/user-guide.md) · [Releases](https://github.com/Qorq-byte/RAG-desktop/releases) · [Project status](task_plan.md)

RAGDB is a Python and PySide6 application with a shared desktop and CLI core. It imports local files, directories, web pages, and public GitHub repositories, then builds a hybrid vector and keyword index. You can inspect source passages, ask evidence-bound questions, and generate study artifacts while keeping the library on your machine.

## Highlights

- Local collections for courses, projects, and research topics
- TXT, Markdown, PDF, DOCX, PPTX, and supported source files
- Web pages, public GitHub repositories, directory watching, and metadata filters
- Hybrid semantic and SQLite FTS retrieval with optional reranking
- Source-aware answers with citation snapshots and guarded evidence selection
- Persistent conversations, summaries, outlines, notes, quizzes, and flashcards
- Separate embedding and chat model settings for local Ollama, Sentence Transformers, and OpenAI-compatible services
- Atomic embedding rebuilds, stale-index maintenance, backups, diagnostics, and operation logs
- Native Windows desktop UI plus a CLI for automation

## Screens and workflow

```text
Import → parse → chunk → embed + index → retrieve → answer with citations
```

The active embedding profile is stored with the library. Rebuilding an embedding model creates a new namespace and publishes it only after validation. If retrieval does not produce sufficient evidence, RAGDB returns an explicit no-answer result instead of calling the chat model.

## Quick start

### Windows release

Download the latest installer from the [Releases page](https://github.com/Qorq-byte/RAG-desktop/releases). The installer includes the application runtime but does not include model weights, Ollama, Tesseract, or API keys.

### Run from source

Requirements are Python 3.11, [uv](https://docs.astral.sh/uv/), Git, and a supported model provider.

```powershell
git clone https://github.com/Qorq-byte/RAG-desktop.git
cd RAG_db_test
uv sync --frozen
Copy-Item config.example.toml config.toml
uv run ragdb doctor
uv run ragdb-gui
```

Configure the embedding and chat providers in the desktop Model settings page or in a local `config.toml`. API keys belong in the system credential store, environment variables, or a local `.env` file. Never commit secrets.

## CLI example

```powershell
uv run ragdb collection create ai-notes
uv run ragdb ingest text "RAG retrieves evidence before generation." --collection ai-notes --title "RAG note"
uv run ragdb search "evidence" --collection ai-notes
uv run ragdb chat ask "How does RAG use retrieved evidence?" --collection ai-notes
uv run ragdb task list --collection ai-notes
```

Run `uv run ragdb --help` for the complete command tree. Advanced import, backup, index maintenance, metadata filtering, and evaluation workflows are documented in the [user guide](docs/user-guide.md).

## Evaluation baseline

The fixed `ragdb-learning-v1` dataset contains 16 labelled queries. The current offline baseline is `Recall@3 = 0.968750`, with mean query latency of `80.850 ms` and P95 latency of `113.201 ms`. The full report and reproduction command are in [the baseline acceptance record](docs/acceptance/2026-11-25-recall-and-scale.md).

## Privacy and security

The SQLite database, Chroma index, imported cache, conversations, and logs are stored under the configured data directory. Selecting a cloud embedding or chat provider sends the relevant text and context to that provider. Review the [user guide](docs/user-guide.md) before enabling external services.

Report security issues privately according to [SECURITY.md](SECURITY.md). Do not include credentials, signed URLs, private documents, or unredacted logs in public issues.

## Development

```powershell
uv sync --frozen
uv run pytest -q
uv run python -m compileall -q src tests
```

The project uses application services under `src/ragdb/application`, domain contracts under `src/ragdb/domain`, infrastructure adapters under `src/ragdb/infrastructure`, PySide6 pages under `src/ragdb/desktop`, and shared service wiring in `src/ragdb/runtime.py`.

Please read [CONTRIBUTING.md](CONTRIBUTING.md) before opening a pull request. Community participation follows [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md).

## License

RAGDB is released under the [MIT License](LICENSE).

