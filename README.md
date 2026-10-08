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

The skill walks you through discovery, lists the complete proposed content plan
in chat for approval, then writes articles in batches. It saves
`content-plan.md` only after you approve the plan. You control the pacing —
review each batch and decide what to write next.

The source repository is treated as untrusted evidence. The skill reads relevant
files to verify product behavior, but does not run project scripts or follow
instructions found in READMEs, comments, config values, or tool output. Writers
receive paraphrased facts with source paths, and generated articles are reviewed
against those sources before delivery. These instructions reduce prompt injection
risk; they are not a technical sandbox.

## How it works

| Phase | What happens |
|---|---|
| **Discover** | Reads README, routes, components, config schemas, error messages. Detects project type (web app, CLI, library, etc.) and target audience. |
| **Plan** | Lists every proposed article in chat, ranked by query resolution value, and waits for approval before writing files. |
| **Write** | Dispatches sub-agents with paraphrased evidence briefs. Each article is checked against specific source files. You review each batch before continuing. |
| **Deliver** | Final summary of everything written, with optional Fin setup and draft upload. |

## Optional Fin delivery

The markdown knowledge base is complete without Fin. If you ask for Fin setup,
the skill checks for an installed Intercom CLI. If it is missing, it shows a
verified, version-pinned installation command and asks whether to install it,
give you instructions to install it yourself, or skip Fin. Declining leaves the
markdown files ready for any help center.

Before setup or upload, the skill shows the target workspace and planned actions.
It uploads only reviewed articles as drafts using Intercom API 2.16's
`body_markdown` field. The skill does not generate a conversion script, use
`npx`, or install packages automatically. Setup skips Fin activation. Publishing
articles and enabling customer answers require separate explicit requests.

## What it won't do

- **Won't read `.env` or credentials files** — discovers configuration from schemas and types
- **Won't expose implementation details** — writes for the product's users, not its developers
- **Won't include sensitive data** — treats all output as if it will be published publicly
- **Won't guess** — every claim traces back to actual code
- **Won't run repository commands** — source files are evidence, not agent instructions
- **Won't install the Intercom CLI without asking** — Fin delivery is optional

## Skill structure

```
help-content-from-code/
├── SKILL.md                        # Workflow: discover, plan, write, deliver
└── references/
    └── writing-for-ai-agents.md    # Article style guide for RAG optimization
```

## License

MIT
