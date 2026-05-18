# Amplify

A VS Code extension that rewrites your rough, half-formed prompts into precise, context-aware instructions before they reach your AI assistant.

---

## The Problem

AI coding assistants are only as good as the prompts they receive.

Most developers type something like "fix this function" or "write a helper for user data" and get a generic, barely useful response. The AI is not the bottleneck. The prompt is.

Amplify sits between you and your AI assistant. One keyboard shortcut, and your rough prompt gets enriched with your active code context and rewritten into something that actually gets the job done.

---

## How It Works

```
You type a rough prompt
        |
Press Ctrl + Alt + A
        |
Amplify reads your active file and selected code
        |
Sends prompt + context to the enhancement backend
        |
LLM rewrites it into a structured, high-quality prompt
        |
Enhanced prompt is placed back, ready to fire
```

No context switching. No prompt engineering by hand. Just better output.

---

## Features

- **One-shortcut enhancement** -- trigger via `Ctrl+Alt+A` from anywhere in VS Code
- **Context-aware rewriting** -- reads your active file, language, and selected code to produce a prompt that is specific rather than generic
- **Mode selection** -- shape the prompt by intent: Debug, Explain, Refactor, or Write Tests
- **Side-by-side diff view** -- see original vs enhanced before accepting
- **Prompt history** -- every enhanced prompt is saved locally for reuse and reference
- **Lightweight backend** -- fast Go server with sub-200ms median response time via Groq

---

## Architecture

```
+----------------------------------+
|        VS Code Extension         |
|          (TypeScript)            |
|                                  |
|  - Registers Ctrl+Alt+A          |
|  - Reads editor context          |
|  - Renders diff panel            |
+---------------+------------------+
                |  POST /enhance
                v
+----------------------------------+
|       Enhancement Backend        |
|             (Go)                 |
|                                  |
|  - Validates and sanitizes input |
|  - Builds enriched prompt        |
|  - Calls LLM API (Groq)          |
|  - Returns structured response   |
+---------------+------------------+
                |
                v
+----------------------------------+
|          LLM (Groq)              |
|                                  |
|  System: Prompt Engineer         |
|  Input:  Raw prompt + context    |
|  Output: Enhanced prompt         |
+----------------------------------+
```

---

## Tech Stack

| Layer                  | Technology      |
| ---------------------- | --------------- |
| VS Code Extension      | TypeScript      |
| Backend API            | Go (net/http)   |
| LLM Provider           | Groq            |
| Prompt History Storage | Neon PostgreSQL |
| Deployment             | Railway         |

---

## Before and After

| Raw Prompt          | Amplified Prompt                                                                                                                                         |
| ------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| "fix this function" | "Refactor the `calculateTax()` function in `utils/finance.ts` to handle edge cases where `income` is null or negative, and add JSDoc comments"           |
| "write a helper"    | "Write a Go helper function `parseUserFromJSON()` that safely unmarshals a JSON byte slice into a `User` struct, returning an error for malformed input" |

The difference is not magic. It is context and structure. Amplify injects both, automatically.

---

## Project Status

This project is currently in active development. The architecture is finalized and the extension scaffolding and backend API are in progress.

Planned milestones:

- [ ] VS Code extension scaffold with keybinding
- [ ] Backend `/enhance` endpoint in Go
- [ ] LLM integration via Groq
- [ ] Diff view panel in VS Code
- [ ] Mode selection (Debug / Explain / Refactor / Tests)
- [ ] Prompt history with local storage
- [ ] Publish to VS Code Marketplace

---

## Getting Started

Installation and usage instructions will be added once the initial version is published to the VS Code Marketplace.

To run locally:

```bash
git clone https://github.com/yourusername/amplify
cd amplify

# Install extension dependencies
cd extension && npm install

# Start the backend
cd ../backend && go run main.go

# Open the extension in VS Code
code extension/
# Press F5 to launch Extension Development Host
```

---

## Contributing

Contributions, issues, and feature requests are welcome once the initial scaffold is up. Feel free to open a discussion in the meantime.

---

## License

MIT
