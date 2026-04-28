# Writing Help Articles for AI Agent Retrieval

These articles will be served by AI agents (like Intercom's Fin) to answer user questions.
The agent finds relevant articles through semantic search, reads them, and synthesizes an
answer. This means articles must be optimized for retrieval and machine comprehension.

---

## Article Template

Every article follows this structure:

```markdown
# [Clear, specific title matching how users phrase questions]

[First paragraph: 1-2 sentence overview + what problems this solves.
Front-load the answer. This paragraph may be the only chunk retrieved.]

## [H2 header as a user question or clear sub-topic]

[200-500 words per section. Self-contained — must make sense if retrieved alone.]

## [Next H2]

[...]
```

---

## Writing Rules

### 1. Front-load the answer

The first paragraph is the TL;DR. The AI agent may only retrieve one chunk from the
article — make the opening paragraph useful on its own. State what the feature is,
what it does, and the most important thing the user needs to know.

Bad: "In this article, we'll walk through the notification system and its various options."
Good: "Email notifications alert you when a new order is placed. Notifications are enabled
by default and can be configured in the Settings page under Notifications."

### 2. Entity density

Repeat key terms in the body text. Never use "it," "this feature," "the above," or
"the system" when you mean a specific thing. Always name the actual feature, setting,
or concept.

Bad: "Click the button to reset it. Once it's done, you'll see a confirmation."
Good: "Click the Reset Password button to reset your password. Once the password reset
is complete, a confirmation email is sent to your registered email address."

This directly impacts retrieval accuracy. When a user asks about "password reset," the
AI agent searches for chunks containing those words. Pronouns don't match.

### 3. Titles mirror customer language

Write titles the way a user would phrase a question or search query.

Good titles:
- "How to configure email notifications"
- "Setting up single sign-on with SAML"
- "Understanding usage limits and rate quotas"
- "Troubleshooting failed webhook deliveries"

Bad titles:
- "Notifications" (too vague for retrieval)
- "SAML Module" (internal/technical framing)
- "Limits" (ambiguous — which limits?)
- "Errors" (matches everything, resolves nothing)

### 4. H2 headers as user questions

Section headers define chunk boundaries in RAG systems. Write them as questions users
would ask, not as feature names.

Good: "How do I change the notification frequency?"
Bad: "Frequency Settings"

Good: "What happens when the storage limit is reached?"
Bad: "Storage Limits"

### 5. Self-contained sections

Each H2 section must make complete sense if retrieved in isolation. The AI agent may
pull a single section, not the full article.

- Never write "as mentioned above" or "see the previous section"
- Never write "as described in [Other Article]" — no cross-references
- If a section depends on context from elsewhere, repeat that context inline
- Each section should open with enough context to orient the reader

### 6. Numbered lists for procedures

Use numbered lists for step-by-step instructions. AI agents handle numbered steps
correctly and present them in order. Use bullet points for non-sequential lists
(options, features, alternatives).

### 7. Specific values

Include exact defaults, ranges, option names, and error messages from the code.
AI agents need precision to give precise answers.

Bad: "You can configure various timeout options."
Good: "The request timeout defaults to 30 seconds. Set the `requestTimeout` option
to any value between 1 and 300 seconds."

### 8. Consistent terminology

Use one canonical term for each concept throughout all articles. If the code calls
something a "workspace," never call it an "account," "organization," or "space"
in the articles. Inconsistent terms confuse retrieval — the AI agent can't connect
"workspace" in the query to "account" in the article.

### 9. Article length: 600-1,200 words

This is the sweet spot for RAG systems:
- Long enough to cover a topic thoroughly (2-4 natural chunks)
- Short enough to maintain and keep accurate
- Each chunk (200-500 words) is a meaningful, self-contained unit

If an article grows beyond 1,200 words, split it into separate focused articles.

### 10. No filler

Skip these entirely:
- "In this article, we'll explore..."
- "Let's take a look at..."
- "This is an important feature because..."
- Marketing language, superlatives, personality

Go straight to the substance. Every sentence should contain information the AI agent
can use to answer a user's question.

### 11. Markdown only

Use only standard markdown formatting:
- `#`, `##`, `###` for headings
- `**bold**` for emphasis
- `-` or `1.` for lists
- `` `code` `` for inline code and ` ``` ` for code blocks
- `|` tables for structured comparisons

No HTML tags. No custom components. No embedded images or media. The files must be
plain `.md` importable into any help center.

---

## What NOT to Write

- **Screenshots or UI navigation** — AI agents can't see images. Don't write "click the
  gear icon in the top right." Describe the setting or action directly.
- **Vague language** — "various options," "several ways," "and more" give the AI agent
  nothing concrete to work with.
- **Yes/no without context** — "Yes, you can" is useless. Explain how, when, and any
  constraints.
- **References to other articles** — "See [Setting Up Webhooks] for details" will break
  on import. Repeat the necessary context inline instead.
- **Speculation** — Only document what exists in the actual code. If you're not sure a
  feature exists, don't write about it.
- **Sensitive data** — Never include env var values, API keys, internal URLs, PII, or
  security implementation details. Describe what a setting does, never its actual value.
  Write as if the article will be published publicly.

---

## Data Safety Reminders

Before finalizing any article, verify:
- No actual secret values, API keys, or credentials appear anywhere
- No internal URLs, server names, or infrastructure details are mentioned
- No PII from test data, seed files, or code comments leaked through
- No security implementation details (auth internals, encryption specifics, rate limits)
- All configuration is described by purpose, not by actual production values
