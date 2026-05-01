# Help Content from Code

Analyze any codebase and generate a complete help center knowledge base as structured markdown files, optimized for AI agent retrieval.

Works with Claude Code, GitHub Copilot, Cursor, Codex, Gemini CLI, and any agent that supports the [Agent Skills](https://agentskills.io) standard.

**[See the demo →](https://help-content-from-code.gustavscirulis.com)** — this skill read three open-source codebases (Ollama, QMD, Pearcleaner) and produced every article on the demo site from the source code alone. You can browse the full generated content — articles, categories, sections.

Point it at a repo, and it will:
1. **Discover** what the product does by reading the code
2. **Plan** which articles to write, prioritized by which user questions they'd resolve
3. **Write** the articles in batches, grounded in what the code actually does
4. **Deliver** a folder of markdown files ready to import into any help center

## What it produces

```
your-product-help-content/
├── content-plan.md
├── getting-started/
│   ├── what-is-your-product.md
│   ├── getting-started.md
│   └── key-concepts.md
├── features/
│   ├── feature-name.md
│   └── ...
├── configuration/
│   ├── setting-area.md
│   └── ...
└── troubleshooting/
    ├── common-issue.md
    └── ...
```

Plain markdown files. No vendor lock-in. Import them into any help center that accepts markdown.

## Why articles are written this way

The articles are optimized for RAG-based AI agents based on research into how these systems retrieve and use content:

- **Titles match how users phrase questions** — semantic search finds them
- **Each section is self-contained** — AI agents may retrieve a single section, not the full article
- **High entity density** — key terms repeated in body text so retrieval actually works
- **Front-loaded answers** — the first paragraph answers the question, not the last
- **600-1,200 words per article** — the sweet spot for RAG chunking
- **No implementation details** — describes the product's interface, not its internals

## Install

```bash
npx skills add gustavscirulis/help-content-from-code
```

## Usage

Open your agent in any repo and ask:

```
Write help center articles for this product
```

Or be more specific:

```
Generate a knowledge base for this codebase, optimized for AI agents
```

```
What help content should we write for this project?
```

The skill walks you through discovery, presents a content plan for approval, then writes articles in batches. You control the pacing — review each batch and decide what to write next.

## How it works

| Phase | What happens |
|---|---|
| **Discover** | Reads README, routes, components, config schemas, error messages. Detects project type (web app, CLI, library, etc.) and target audience. |
| **Plan** | Creates a prioritized content plan. Articles ranked by query resolution value — which user questions would this article answer? |
| **Write** | Dispatches sub-agents to write articles in parallel. Each article grounded in specific source files. You review each batch before continuing. |
| **Deliver** | Final summary of everything written, with next steps for deployment. |

## What it won't do

- **Won't read `.env` or credentials files** — discovers configuration from schemas and types
- **Won't expose implementation details** — writes for the product's users, not its developers
- **Won't include sensitive data** — treats all output as if it will be published publicly
- **Won't guess** — every claim traces back to actual code

## Skill structure

```
help-content-from-code/
├── SKILL.md                        # Workflow: discover, plan, write, deliver
└── references/
    └── writing-for-ai-agents.md    # Article style guide for RAG optimization
```

## License

MIT
