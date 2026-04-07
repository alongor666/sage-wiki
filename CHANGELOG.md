# Changelog

## 0.1.0 — 2026-04-07

First public release of sage-wiki, an LLM-compiled personal knowledge base.

### Core

- **5-pass compiler pipeline** — diff detection, summarization, concept extraction, article writing, and image captioning. Supports parallel LLM calls with checkpoint/resume.
- **Multi-format source extraction** — Markdown, PDF, Word (.docx), Excel (.xlsx), PowerPoint (.pptx), CSV, EPUB, email (.eml), plain text, transcripts (.vtt/.srt), images (via vision LLM), and code files.
- **Hybrid search** — Reciprocal Rank Fusion combining BM25 (FTS5) + cosine vector similarity + tag boost + recency decay.
- **Ontology graph** — Typed entity-relation graph with BFS traversal, cycle detection, and concept interlinking via `[[wikilinks]]`.
- **Q&A agent** — Natural language questions answered with LLM synthesis, source citations, and auto-filed output articles.
- **Watch mode** — File system watcher with debounce, polling fallback for WSL2/network drives.

### LLM Support

- **Providers** — Anthropic, OpenAI, Gemini, Ollama, and any OpenAI-compatible API (OpenRouter, Azure, etc.).
- **Streaming** — Native SSE streaming for all providers (OpenAI, Anthropic, Gemini).
- **Per-task model routing** — Configure different models for summarize, extract, write, lint, and query tasks.
- **Embedding cascade** — Provider API embeddings with Ollama fallback. Auto-detect dimensions for unknown models.
- **Rate limiting** — Token bucket rate limiter with exponential backoff on 429s.

### Web UI

- **Article browser** — Rendered markdown with syntax highlighting, clickable `[[wikilinks]]`, frontmatter badges, and breadcrumb navigation.
- **Knowledge graph** — Interactive force-directed visualization with node coloring by type, neighborhood queries, and click-to-navigate.
- **Streaming Q&A** — Ask questions in the browser with real-time token streaming and source citations. Answers auto-filed to outputs/.
- **Search** — Debounced hybrid search with ranked results and snippets.
- **Table of contents** — Scroll-spy with active heading highlight, toggleable with graph view.
- **Dark/light mode** — Toggle with system preference detection and localStorage persistence.
- **Broken link detection** — Missing article links shown in gray with tooltip.
- **Hot reload** — WebSocket-based auto-refresh when wiki files change (pairs with `compile --watch`).
- **Keyboard shortcuts** — `/` focuses search, `Esc` clears.
- **Embedded in binary** — Preact + Tailwind CSS via `go:embed` with build tag. Binary works without web UI when built without `-tags webui`.

### MCP Server

- **14 tools** — 5 read (search, read, status, graph, list), 7 write (add source, write summary, write article, add ontology, learn, commit, compile diff), 2 compound (compile, lint).
- **Transports** — stdio (for Claude Code, Cursor, etc.) and SSE (for network clients).
- **Path traversal protection** — All file operations validated with `isSubpath`.

### CLI

- `sage-wiki init [--vault] [--prompts]` — Greenfield or Obsidian vault overlay setup.
- `sage-wiki compile [--watch] [--dry-run] [--fresh] [--re-embed] [--re-extract]` — Full compiler with multiple modes.
- `sage-wiki serve [--ui] [--transport stdio|sse] [--port] [--bind]` — MCP server or web UI.
- `sage-wiki search`, `query`, `ingest`, `lint`, `status`, `doctor` — Full CLI toolkit.
- **Customizable prompts** — `sage-wiki init --prompts` scaffolds editable prompt templates.

### Linting

- **7 passes** — Completeness, style (with auto-fix), orphans, consistency, connections, impute, and staleness.
- **Learning integration** — Dedup via SHA-256, 500 cap, 180-day TTL, keyword recall.

### Quality

- Zero CGO. Pure Go. Single binary. Cross-platform (Linux, macOS, Windows — amd64 + arm64).
- SQLite with WAL + single-writer mutex for concurrent safety.
- CSRF protection, SSRF validation, request body limits, file type allowlists.
- 20 test packages, all passing.

### Binaries

| Platform | Binary |
|----------|--------|
| Linux amd64 | `sage-wiki-linux-amd64` |
| Linux arm64 | `sage-wiki-linux-arm64` |
| macOS amd64 (Intel) | `sage-wiki-darwin-amd64` |
| macOS arm64 (Apple Silicon) | `sage-wiki-darwin-arm64` |
| Windows amd64 | `sage-wiki-windows-amd64.exe` |
| Windows arm64 | `sage-wiki-windows-arm64.exe` |
