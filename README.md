# 🎯 Page Object Generator

> AI-powered Page Object Model (POM) generator for Playwright, Selenium & Cypress — runs entirely in your browser, no backend needed.

![Page Object Generator](https://img.shields.io/badge/AI--Powered-POM%20Generator-4f8ef7?style=for-the-badge&logo=robot)
![Frameworks](https://img.shields.io/badge/Frameworks-Playwright%20%7C%20Selenium%20%7C%20Cypress-22c55e?style=for-the-badge)
![Languages](https://img.shields.io/badge/Languages-TS%20%7C%20JS%20%7C%20Python%20%7C%20Java%20%7C%20C%23-f59e0b?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-blue?style=for-the-badge)

---

## ✨ What It Does

Paste a **URL** or **raw HTML** → choose your framework & language → click **Generate** → get a production-ready Page Object class + test spec file in seconds.

No servers. No installs. One `.html` file that runs anywhere.

---

## 🚀 Quick Start

1. **Download** `page-object-generator.html`
2. **Open** it in any modern browser (Chrome, Firefox, Edge)
3. **Get a free API key** from one of the supported providers (see below)
4. **Paste your key** → click Save → generate!

---

## 🤖 Supported AI Providers

| Provider | Cost | Model Used | Get Your Key |
|---|---|---|---|
| 🔵 **Google Gemini** | ✅ Free · 1500 req/day | gemini-2.0-flash | [aistudio.google.com/apikey](https://aistudio.google.com/apikey) |
| ⚡ **Groq** | ✅ Free tier | Llama 3.3 70B | [console.groq.com/keys](https://console.groq.com/keys) |
| 🔀 **OpenRouter** | ✅ Free models available | Llama 3.3 70B | [openrouter.ai/settings/keys](https://openrouter.ai/settings/keys) |
| 🟣 **Anthropic Claude** | 💳 Paid credits | claude-sonnet-4 | [console.anthropic.com](https://console.anthropic.com) |

> **Recommended for beginners:** Start with **Google Gemini** — easiest signup, most generous free quota, and excellent code generation quality.

---

## 🧩 Supported Frameworks & Languages

**Frameworks:**
- 🎭 Playwright
- 🔬 Selenium WebDriver
- 🌲 Cypress

**Languages:**
- TypeScript
- JavaScript
- Python
- Java
- C#

---

## ⚙️ Features

- **Smart locator priority** — AI picks the most stable selector strategy: `data-testid` → ARIA roles → semantic selectors → IDs → CSS classes (avoids brittle XPath)
- **Dual output** — generates both the Page Object class file AND a ready-to-run test spec
- **Configurable options:**
  - ✅ Include Action Methods (click, fill, navigate)
  - ✅ Add Assertions (expect / assert statements)
  - ✅ JSDoc / Docstring comments
  - ✅ Explicit Waits / Timeouts
- **Syntax highlighting** — color-coded output in the browser
- **Copy & Download** — one-click copy or download as `.ts / .js / .py / .java / .cs`
- **Live stats** — shows element count, method count, line count

---

## 🔄 Application Workflow

Understanding how the app works end-to-end:

```
┌─────────────────────────────────────────────────────────┐
│                    USER INPUT LAYER                      │
│  URL or HTML  →  Page Name  →  Framework  →  Language   │
│              →  Options (Methods, Assertions, etc.)      │
└────────────────────────┬────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────┐
│                  PROMPT ENGINEERING                       │
│  buildPrompt() assembles a structured instruction:       │
│  • Role: "expert test automation engineer"               │
│  • Input: URL description OR raw HTML                    │
│  • Requirements: framework, language, class name         │
│  • Locator priority rules                                │
│  • Output format: strict JSON { page, spec }             │
└────────────────────────┬────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────┐
│               PROVIDER-AWARE API CALL                    │
│  callAI(prompt) routes to selected provider:             │
│                                                          │
│  Gemini  → generativelanguage.googleapis.com             │
│  Groq    → api.groq.com/openai/v1/...                   │
│  OpenRouter → openrouter.ai/api/v1/...                  │
│  Claude  → api.anthropic.com/v1/messages                 │
│                                                          │
│  Each provider has its own auth header format            │
└────────────────────────┬────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────┐
│                  RESPONSE PARSING                        │
│  parseGeneratedCode(raw):                                │
│  1. Strip markdown code fences (```json ... ```)         │
│  2. JSON.parse() the response                            │
│  3. Extract { page, spec } fields                        │
│  4. Fallback: use raw text if JSON parsing fails         │
└────────────────────────┬────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────┐
│                OUTPUT RENDERING                          │
│  syntaxHighlight() → regex-based token coloring          │
│  showCode() → inject into <pre> code block              │
│  updateStats() → count elements, methods, lines          │
│  Tab switching → page file ↔ spec file                  │
└─────────────────────────────────────────────────────────┘
```

---

## 🧠 Key Concepts Used

### 1. Prompt Engineering
The heart of the app. `buildPrompt()` constructs a precise natural language instruction that tells the AI:
- **What role to play** ("expert test automation engineer")
- **What input to analyze** (URL context or raw HTML)
- **What rules to follow** (locator priority ladder)
- **What format to output** (strict JSON with two keys: `page` and `spec`)

Forcing JSON output is critical — it makes the response machine-parseable without relying on fragile text extraction.

### 2. Multi-Provider Abstraction
`callAI(prompt)` is a unified wrapper that adapts to 4 different REST APIs. Each provider has a different:
- **Endpoint URL** — Gemini uses query params for auth; others use Authorization headers
- **Request body shape** — Gemini uses `contents[].parts[]`; OpenAI-compatible APIs use `messages[]`
- **Response shape** — Gemini uses `candidates[0].content.parts[0].text`; OpenAI-compatible use `choices[0].message.content`

The abstraction means the rest of the app doesn't care which provider is active.

### 3. Structured JSON Output
By instructing the AI to respond **only in JSON**, the app can reliably extract two separate code files (page object + spec) from a single API call. The parser also handles edge cases where models wrap output in markdown fences.

### 4. Client-Side Only Architecture
Zero backend. The HTML file:
- Stores state in JavaScript variables (not localStorage)
- Makes API calls directly from the browser using `fetch()`
- Uses `anthropic-dangerous-direct-browser-access: true` header for Claude (required for browser-direct calls)
- API keys are held in memory only — never stored, never sent anywhere except the chosen AI provider

### 5. Regex-Based Syntax Highlighting
`syntaxHighlight()` applies token-level coloring using sequential regex replacements:
1. Escape HTML entities (`<`, `>`, `&`) first to prevent XSS
2. Match and wrap: comments → strings → keywords → class names → function calls

### 6. Locator Priority Strategy
The AI is instructed to follow test automation best practices for selector robustness:
```
data-testid  →  ARIA roles/labels  →  semantic HTML  →  IDs  →  CSS classes  →  XPath (last resort)
```
This mirrors the official Playwright and Testing Library philosophy of testing the way users interact with the page.

---

## 📁 Project Structure

```
page-object-generator.html   ← the entire application (single file)
README.md                    ← this file
```

This is intentionally a **single-file app** — no build step, no dependencies, no node_modules. Open it and it works.

---

## 🖼️ Screenshots

| Input Configuration | Generated Output |
|---|---|
| Provider selector, URL/HTML input, framework & language pickers | Syntax-highlighted Page Object with tab for spec file |

---

## 🛠️ How to Extend

Want to add a new AI provider? Add an entry to `PROVIDER_META` and a new branch in `callAI()`:

```javascript
// 1. Add metadata
const PROVIDER_META = {
  // ... existing providers
  myprovider: {
    hint: 'Get free key → myprovider.com/keys',
    placeholder: 'mp-key-...',
    label: 'MyProvider'
  }
};

// 2. Add API call branch
async function callAI(prompt) {
  // ... existing branches
  } else if (selectedProvider === 'myprovider') {
    const res = await fetch('https://api.myprovider.com/v1/chat', {
      method: 'POST',
      headers: { 'Authorization': `Bearer ${savedApiKey}` },
      body: JSON.stringify({ prompt, max_tokens: 3000 })
    });
    const data = await res.json();
    return data.output;
  }
}
```

Then add a `.provider-btn` button in the HTML with `data-provider="myprovider"`.

---

## 🔐 Privacy & Security

- **API keys are never stored** — held in a JavaScript variable for the browser session only
- **No analytics, no telemetry, no backend** — the HTML file makes calls directly to AI provider APIs
- **No data leaves your machine** except the prompt sent to your chosen AI provider
- Keys are masked in the UI after saving (displayed as `••••••••••••••••••`)

---

## 🐛 Troubleshooting

| Error | Cause | Fix |
|---|---|---|
| `Failed to fetch` | CORS or network issue | Use a provider that supports browser-direct calls; Groq and OpenRouter work well |
| `Credit balance too low` | Anthropic account has no credits | Add credits at console.anthropic.com or switch to a free provider |
| `✗ Key too short` | API key validation failed | Paste the full key (must be at least 10 characters) |
| Generated code is empty | Model returned unexpected format | Try a different provider; the parser falls back to raw text |
| Layout looks broken | Old cached version | Hard refresh with Ctrl+Shift+R |

---

## 📄 License

MIT — free to use, modify, and distribute.

---

## 🙏 Credits

Built with:
- [Google Gemini API](https://aistudio.google.com) — free AI inference
- [Groq API](https://groq.com) — ultra-fast Llama inference
- [OpenRouter](https://openrouter.ai) — multi-model API gateway
- [Anthropic Claude API](https://anthropic.com) — Claude models
- [JetBrains Mono](https://www.jetbrains.com/lp/mono/) — monospace font
- [Syne](https://fonts.google.com/specimen/Syne) — display font

Inspired by [NaveenAutomationLabs](https://naveenautomationlabs.com) Page Object Generator concept.

👤 Author
Ansuman Nayak - https://ansuman2105.github.io/
