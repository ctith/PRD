# ManipDetect 🔍

### AI-powered manipulation detection in text & landing pages

![Status](https://img.shields.io/badge/status-MVP%20shipped-success)
![Stack](https://img.shields.io/badge/stack-Lovable%20%7C%20n8n%20%7C%20Claude%20%7C%20Supabase%20%7C%20Airtable%20%7C%20Jina-blue)
![Type](https://img.shields.io/badge/type-Agentic%20AI%20product-purple)
![Author](https://img.shields.io/badge/author-Caroline%20Tith-darkred)

> **Live demo:** https://manipdetect.lovable.app

---

## Table of Contents

1. [Problem Statement](#1-problem-statement)
2. [Target User & Context](#2-target-user--context)
3. [Why Now](#3-why-now)
4. [Value Hypothesis](#4-value-hypothesis)
5. [Market Scan](#5-market-scan)
6. [Solution Concept](#6-solution-concept)
7. [AI Use-Case & Strategic Role](#7-ai-use-case--strategic-role)
8. [Tech Stack & Architecture](#8-tech-stack--architecture)
9. [Production-Grade Target](#9-production-grade-target)
10. [Accessibility](#10-accessibility)
11. [KPIs & Measurement](#11-kpis--measurement)
12. [Guardrails & Failsafes](#12-guardrails--failsafes)
13. [Backlog & Golden Path](#13-backlog--golden-path)
14. [Manipulation Taxonomy](#14-manipulation-taxonomy)
15. [System Prompt](#15-system-prompt)
16. [Constraints](#16-constraints)
17. [Tests](#17-tests)
18. [Out of Scope](#18-out-of-scope)
19. [Future Evolution](#19-future-evolution)
20. [Learnings](#20-learnings)

---

## 1. Problem Statement

Most people are exposed to rhetorical manipulation in their daily communications — professional emails, contracts, personal messages, and e-commerce landing pages — without being able to identify the techniques used. This asymmetry keeps them in a position of cognitive weakness during negotiations, purchasing decisions, and personal relationships.

**Naming a manipulation technique neutralises it 80% of the time.** ManipDetect turns a vague sense of unease into an actionable, structured analysis.

---

## 2. Target User & Context

### Primary users

| Profile                      | Context                                             | Pain                                     |
| ---------------------------- | --------------------------------------------------- | ---------------------------------------- |
| Employee in HR conflict      | Receives an ambiguous email from management         | Cannot name what feels wrong             |
| Freelance consultant         | Reviewing a client contract before signing          | Unsure if pressure is legitimate         |
| Online shopper               | Browsing an e-commerce site with aggressive pricing | Cannot tell if urgency is real           |
| Consumer protection advocate | Auditing landing pages                              | No scalable tool for systematic analysis |

### Secondary users

- Journalists and fact-checkers auditing political or commercial communication
- Legal professionals reviewing contractual language
- Coaches and therapists helping clients decode difficult relationships
- Trainers building media literacy curricula

---

## 3. Why Now

- **EU AI Act (2024)** and **European Accessibility Act (June 2025)** are pushing transparency requirements across digital products — consumer protection tools are entering a regulatory tailwind
- **Explosion of AI-generated copywriting** on landing pages and e-commerce sites has industrialised manipulation at scale — the problem is growing faster than human literacy
- **LLMs are now capable** of nuanced semantic analysis with structured JSON output — this product was not technically feasible at this quality level 24 months ago
- **No-code automation platforms** (n8n, Lovable, Supabase, Airtable) make it possible to ship an agentic MVP in days without infrastructure overhead
- **Dropshipping fraud** is a documented and growing consumer harm in France and Europe — the extension to e-commerce analysis addresses a real and underserved market

---

## 4. Value Hypothesis

ManipDetect reduces the cognitive asymmetry between the sender who masters rhetorical techniques and the receiver who is subject to them.

For individual users: transform "something feels off" into "this message uses culpabilisation and false urgency combined — here is how to respond."

For e-commerce: transform "I'm not sure I trust this site" into "this product is listed at 189€ here and found at 12€ on AliExpress — here is the link."

**Current workarounds:**

- Reading books on rhetoric and persuasion (hours of investment, no real-time utility)
- Asking a trusted friend or coach (unavailable at the moment of need)
- Using generic ChatGPT prompts (no structured taxonomy, no interface, no report)
- Googling the product (manual, multi-step, no intelligence layer)

**Impact of the problem:** wasted money on overpriced products, poor decisions under artificial pressure, staying in unfavourable situations due to inability to name the dynamic.

---

## 5. Market Scan

| Tool                       | What it does well                   | What it misses                                                                                                              | Relevant constraint               |
| -------------------------- | ----------------------------------- | --------------------------------------------------------------------------------------------------------------------------- | --------------------------------- |
| **Grammarly**              | Style, grammar, tone detection      | No rhetorical or manipulation analysis                                                                                      | Paid, no API for custom analysis  |
| **Crystal Knows**          | Personality profiling of the sender | Analyses the person, not the text                                                                                           | GDPR concerns, B2B focus          |
| **Generic tone analyzers** | Positive / negative sentiment       | Far too superficial, no taxonomy                                                                                            | No actionable output              |
| **ChatGPT (free prompt)**  | Can analyse if well-prompted        | No dedicated interface, no taxonomy, inconsistent output                                                                    | No structured report, UX friction |
| **No tool identified**     | —                                   | **Dedicated manipulation detection with structured taxonomy + agentic multi-source analysis + e-commerce price comparison** | Gap confirmed                     |

**Conclusion:** the gap is real and unoccupied. The closest competitor is a well-crafted ChatGPT prompt — our differentiation is the taxonomy, the agentic RAG workflow, and the e-commerce price comparison layer.

---

## 6. Solution Concept

### MVP (shipped — Le Wagon)

The user pastes a text **or** submits a URL in the Lovable web app. The n8n agent then:

1. Detects input type automatically (regex on the content — no manual "type" flag needed)
2. Scrapes the page via **Jina AI Reader** if it is a URL
3. Guards against blocked pages (Cloudflare / captcha / empty scrape) and, if blocked, asks the user to paste the visible text instead of analysing a verification page
4. Runs an **AI Agent (Claude)** that retrieves the relevant manipulation taxonomy from a **Supabase vector store** (RAG) and analyses the text
5. Parses and validates the structured JSON output
6. Returns an annotated report to Lovable: techniques detected, excerpts, explanations, response suggestions

### V1 — Dropshipping extension (post-MVP)

For e-commerce URLs, the agent additionally:

1. Extracts the product name and keywords
2. Searches AliExpress / Temu for equivalent products
3. Checks domain age via WHOIS API
4. Analyses review patterns for fake testimonials
5. Returns a dropshipping probability score (0–100) with verdict and direct link to source product

---

## 7. AI Use-Case & Strategic Role

### What the AI step does

The analysis runs inside an **n8n LangChain Agent** powered by **Claude (Anthropic)**. Rather than receiving the full taxonomy stuffed into the prompt, the agent **retrieves** the relevant techniques on demand from a **Supabase vector store** (taxonomy embedded with OpenAI embeddings) — a retrieval-augmented generation (RAG) pattern. It returns a structured JSON containing:

- Number of distinct techniques detected
- Global intensity score (`faible | moderee | elevee | systemique`)
- Per-technique breakdown: name, exact excerpt, explanation, response suggestion
- 2–3 sentence synthesis of the rhetorical profile

A short-term conversational memory (buffer window) lets the user follow up on a previous analysis in the same session.

### Where it sits in the golden path

```
User input (text or URL) — Lovable front
        ↓
[n8n] Regex type detection → Jina Reader scraping (for URLs)
        ↓
[n8n] Blocked-page failsafe (else: ask user to paste text)
        ↓
[AI AGENT] Claude  ← CORE AI STEP
   + RAG retrieval of the taxonomy from Supabase vector store
   + session memory
        ↓
[n8n] Robust JSON parse + schema validation
        ↓
[Lovable] Structured report displayed to user
```

### Strategic Role Statement

> Without the AI step, ManipDetect is an empty form. The agent holds the analytical capability that would take a trained rhetorician 30–45 minutes to produce manually — the product delivers it in seconds, at near-zero marginal cost per analysis. At scale, no human review layer could match this throughput. **AI is not a feature — it is the product.**

### User value unlocked

- **Time saved:** 30–45 minutes of manual analysis → seconds
- **Cognitive relief:** eliminates the need for prior expertise in rhetoric or psychology
- **Actionability:** each technique comes with a concrete response suggestion
- **Confidence:** structured report replaces vague intuition with named, explained patterns

---

## 8. Tech Stack & Architecture

### POC Stack

| Layer                 | Tool                                   | Why chosen for POC                                                                                  | Known constraints                                                  |
| --------------------- | -------------------------------------- | --------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------ |
| **Frontend**          | Lovable (React)                        | AI-generated React UI, instant public deploy; replaced Softr for full control of the report UI and the webhook call | Free-tier prompt limits; styling driven by prompt                  |
| **Orchestration**     | n8n                                    | Visual agentic workflow, native LangChain + HTTP + Code nodes, self-hostable                        | Webhook timeout, rate limits on cloud free tier                    |
| **AI agent**          | n8n LangChain Agent + Claude (Anthropic) | Tool-using agent: retrieves taxonomy, then analyses; strong structured JSON output                  | Cost per call; added latency from retrieval                        |
| **Retrieval / vectors** | Supabase (Postgres + pgvector)        | Stores the embedded taxonomy; queried by the agent as a RAG tool                                    | Requires an embedding/indexing pipeline; pgvector setup            |
| **Embeddings**        | OpenAI embeddings                      | Index the taxonomy and embed agent queries                                                          | Second AI-provider dependency; cost (mostly one-time at indexing)  |
| **Knowledge source**  | Airtable                               | Editable source of truth for the 18-technique taxonomy; synced into the vector store                | 5 req/s rate limit on free tier                                    |
| **Scraping**          | Jina AI Reader (`r.jina.ai`)           | Turns a URL into clean text with a single GET; handles many JS-heavy pages the raw HTTP node could not | Cloudflare *challenge* pages still blocked → paste fallback; rate limits without an API key |
| **Hosting**           | Lovable (front) + n8n Cloud / self-host (workflow) | Fast public deploy for demo day                                                                     | Self-host required for production                                  |

### Architecture Diagram — POC (runtime)

```
┌──────────────────────────────────────────────────────┐
│              FRONTEND — Lovable (React)                │
│               manipdetect.lovable.app                  │
│             Paste text   OR   submit URL               │
└─────────────────────────┬──────────────────────────────┘
                          │ POST { input_type, content, timestamp }
                          ▼
┌──────────────────────────────────────────────────────┐
│                     n8n WORKFLOW                       │
│  [1] Webhook (responds via Respond node)               │
│  [2] IF "Text or URL?" — regex ^https?:// on content   │
│        ├─ URL  → Jina Reader (r.jina.ai) → clean text  │
│        └─ text → passthrough                           │
│  [3] Merge → Prepare Text (normalise, truncate 4 000)  │
│  [4] IF "Scrape OK?" — blocked-page failsafe           │
│        ├─ blocked → Respond: "please paste the text"   │
│        └─ ok → AI Agent                                │
│                                                        │
│   ┌──────────── AI AGENT (LangChain) ──────────────┐   │
│   │  LLM:    Claude (Anthropic)                     │   │
│   │  Memory: buffer window (session follow-ups)     │   │
│   │  Tool:   Supabase Vector Store (RAG)  ◄─────────┼───┼─ taxonomy
│   │          embeddings: OpenAI                     │   │  retrieval
│   └─────────────────────┬───────────────────────────┘   │
│  [5] Parse JSON (robust: reads direct input, strips md, │
│      validates score_global/intensite/techniques/...)   │
│  [6] Respond to Webhook (CORS headers, JSON)            │
└─────────────────────────┬──────────────────────────────┘
                          │ { score_global, intensite, techniques[], synthese }
                          ▼
┌──────────────────────────────────────────────────────┐
│             Lovable results view (2 states)            │
│   Score | Intensity badge | Synthesis | technique cards │
└──────────────────────────────────────────────────────┘
```

### Architecture Diagram — Async job flow (client ↔ n8n)

*Because the workflow can take ~90 seconds, the call is decoupled from the request/response cycle through a job row in Supabase.*

```
┌───────────────┐   1. enqueue (server fn)    ┌──────────────────────────────┐
│    Lovable    │ ──────────────────────────► │  analysis_jobs (Supabase)    │
│    client     │                             │  status: processing          │
│               │                             │  RLS: deny-all to client/anon│
└──────┬────────┘                             └───────────────┬──────────────┘
       │                                                      │ 2. trigger webhook
       │ 4. poll job (≤ 1 min 30)                             ▼
       │    processing → done                       ┌───────────────────┐
       │                                            │    n8n workflow   │
       │                                            │  Jina + AI Agent  │
       │                                            └─────────┬─────────┘
       │                                                      │ 3. callback
       │                                                      │   header: N8N_WEBHOOK_SECRET
       │                                                      │   updates only if "processing"
       ▼                                                      ▼
   render report  ◄───────── job row updated: status = done + result JSON
```

Server (service role) owns the job row; the client never writes it. The callback authenticates with `N8N_WEBHOOK_SECRET` and refuses to touch a job that is already finished. Timeouts are aligned to ~1 min 30 on both server and client.

### Architecture Diagram — Taxonomy indexing pipeline (setup)

```
Airtable  (Manipulation_Taxonomy = source of truth)
   │  Search records  (batched: Loop Over Items + Wait)
   ▼
Default Data Loader  ──►  Character Text Splitter
   │
   ▼
OpenAI Embeddings  ──►  Supabase Vector Store (pgvector)
                              ▲
                              └── queried at runtime by the AI Agent (RAG tool)
```

### Architecture Diagram — V1 Dropshipping Extension

```
URL (e-commerce)
        │
        ▼
[n8n] Jina Reader scrape
        │
   ┌────┴────────────────────────────────────┐
   │                                         │
   ▼                                         ▼
[AI Agent] Manipulation analysis     [HTTP] AliExpress search
[Supabase] RAG taxonomy retrieval    [WHOIS API] Domain age
[JSON] Technique report              [HTTP] Temu search
   │                                         │
   └────────────────┬────────────────────────┘
                    │
                    ▼
          [n8n] Merge results
                    │
                    ▼
          Manipulation score (0–18)
          + Dropshipping score (0–100)
          + Verdict + AliExpress link
                    │
                    ▼
              Lovable report
```

---

## 9. Production-Grade Target

*Not built now — thinking ahead to avoid painful architectural decisions later.*

| Area                     | POC approach                              | Production change                                                                  | Why                                                                                         |
| ------------------------ | ----------------------------------------- | ---------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| **Scale**                | Async job pattern (`analysis_jobs` + n8n callback + client polling, ~90s) | Managed queue (Bull/Redis) + horizontal workers + dead-letter for failed jobs      | The job row already decouples long AI work from the request; a real queue adds retries and back-pressure at volume |
| **AI layer**             | Single agent, one model (Claude) + OpenAI embeddings | Model fallback chain (primary → secondary) + response caching for identical inputs | Avoid single point of failure; caching cuts cost by ~60% on repeated URLs                   |
| **Retrieval**            | Supabase vector store, manual re-index    | Scheduled re-embedding on taxonomy change + retrieval-quality eval set             | Keep the taxonomy in the vector store in sync with the Airtable source of truth             |
| **Cost control**         | No limits                                 | Rate limiting per user/IP, input truncation at 4,000 tokens, request batching      | Unbounded LLM calls become expensive at scale; one long document can cost $0.10+            |
| **Privacy & compliance** | Session-only, no storage of user input    | Encrypted audit log (no PII), explicit RGPD consent flow, DPA with AI providers    | Any storage of personal communication content triggers GDPR obligations                     |
| **Security**             | Deny-all RLS on `analysis_jobs`; callback gated by `N8N_WEBHOOK_SECRET` | + webhook rate-limiting, input sanitisation (XSS, prompt injection), secret rotation, pen-test | Locking the table and authenticating the callback closes the obvious holes; the rest hardens against abuse at scale |

### Security model (implemented)

The async job flow is the main attack surface, so it is locked down by default:

- **Deny-all RLS on `analysis_jobs`.** Client and anonymous roles cannot read or write any job row. Only the server's **service role** creates and reads jobs — the browser never touches the table directly.
- **Authenticated callback.** The n8n → app callback must present the shared secret `N8N_WEBHOOK_SECRET`; requests without it are rejected. The callback also **only updates jobs still in `processing`**, so a finished result cannot be replayed or overwritten.
- **Secrets server-side only.** The webhook URL, the callback secret, and all AI provider keys live in environment variables / Supabase secrets — never in client code.

*Lesson carried from the build: lock every new table deny-first, and treat any public callback as untrusted until it proves the shared secret.*

---

## 10. Accessibility

ManipDetect is designed to be usable by people with visual, motor, and cognitive disabilities, in alignment with WCAG 2.1 AA and France's RGAA standards.

### Applied practices

| WCAG Principle     | Implementation in ManipDetect                                                                                          |
| ------------------ | ---------------------------------------------------------------------------------------------------------------------- |
| **Perceivable**    | Intensity verdicts use colour + icon + text label (never colour alone — critical for colourblind users)                |
| **Perceivable**    | All technique excerpts are displayed as readable text, not images                                                      |
| **Operable**       | Form submission and results navigation fully keyboard-accessible (Tab, Enter, Escape)                                  |
| **Operable**       | Touch targets (Submit button, technique cards) minimum 44×44px                                                         |
| **Understandable** | Error messages describe the problem and the fix ("Please paste text or a valid URL")                                   |
| **Understandable** | The agent is prompted to use plain, clear language — benefits all users, especially those with cognitive disabilities  |
| **Robust**         | Semantic HTML for all Lovable/React components; ARIA labels on interactive elements                                    |

### Colour contrast

- Intensity badges: minimum 4.5:1 contrast ratio against background (WCAG AA)
- Body text: 16px minimum, no light font weights
- Never use red/green without a complementary icon or label

### Audit plan

Run a Lighthouse accessibility audit (Chrome DevTools) on the Lovable app before demo day. Target score: 90+.

---

## 11. KPIs & Measurement

### Product KPIs

| KPI                                | What it measures                                                                       | Target (demo day) | How to capture                                          |
| ---------------------------------- | -------------------------------------------------------------------------------------- | ----------------- | ------------------------------------------------------- |
| **End-to-end completion rate**     | % of users who submit text/URL AND view the full report                                | > 80%             | n8n log: webhook received vs report rendered            |
| **Analysis accuracy (spot-check)** | % of detected techniques judged correct by a human reviewer on a sample of 10 analyses | > 75%             | Manual review rubric on a test set of 10 diverse inputs |
| **Time to insight**                | Time from form submission to report display                                            | < 15 seconds p50  | n8n execution log timestamps                            |

### AI-Specific KPIs

| KPI                      | Definition in this context                                                                                      | Target               | Data source                                       |
| ------------------------ | --------------------------------------------------------------------------------------------------------------- | -------------------- | ------------------------------------------------- |
| **AI task success rate** | % of agent runs that return valid, parseable JSON with the required keys                                        | > 90%                | n8n error log + JSON parse step                   |
| **Scrape success rate**  | % of URL submissions where Jina returns usable text (not a blocked/verification page)                           | > 70%                | "Scrape OK?" branch counter in n8n                |
| **Latency p50 / p95**    | Median and worst-case end-to-end time (scrape + retrieval + agent)                                              | p50 < 10s / p95 < 25s | n8n execution timestamps                          |
| **Cost per analysis**    | Estimated USD cost per run (Claude tokens + OpenAI embedding queries)                                           | < $0.02 per analysis | Anthropic + OpenAI usage dashboards               |

---

## 12. Guardrails & Failsafes

### Guardrail 1 — Input validation

Before hitting the agent, n8n validates:

- Input is not empty
- Text input is truncated above 4,000 characters (control cost and latency)
- URL input is detected by regex (`^https?://`) on the content — the workflow auto-detects type instead of trusting a client-supplied flag

*Why:* empty or malformed inputs generate expensive, useless calls and confusing error states.

### Guardrail 2 — Output format check (robust parser)

The "Parse JSON from AI Agent" node reads the agent's **direct input** (not a hard-coded node name), strips any markdown code fences, parses the JSON, and verifies the required keys (`score_global`, `intensite`, `techniques`, `synthese`). On failure it returns a clean fallback instead of a broken report.

*Why:* LLMs occasionally wrap JSON in ```` ```json ```` fences or deviate from the schema; and referencing the agent by a hard-coded name silently breaks the moment the node is renamed.

### Failsafe 1 — Blocked-page detection (implemented)

If a scraped URL returns a verification/challenge page (Cloudflare, captcha, "verify you are human", or a body shorter than ~250 characters), the workflow **short-circuits before the agent** and responds with a message asking the user to paste the visible text of the page.

*Why:* otherwise the agent analyses the verification page itself and produces a confident but meaningless "standard" analysis — the worst possible failure for a trust tool. This also avoids wasting an LLM call.

### Failsafe 2 — Error / 404 page detection (implemented)

A scraped URL can also return a *broken* page rather than a *blocked* one: the target site's own SSR error or 404 fallback (e.g. "This page didn't load", "Page not found"). The scrape succeeds, but the extracted "content" is an error page with no rhetorical material. The workflow detects this case (very short body + tell-tale strings such as `didn't load`, `404`, `not found`, `try again`) and returns a clear message — *"This page could not be read — it returned an error. Try another URL or paste the visible text."* — instead of analysing noise.

*Why:* the upstream site can be broken independently of ManipDetect; analysing its error page produced confident-but-empty reports and surfaced 500s on the server function. This complements Failsafe 1: *blocked* (bot wall) and *broken* (error page) are different failures, both caught before the agent.

### Failsafe 3 — Graceful degradation

If the agent call fails (timeout, API error, invalid JSON after parse), the user sees:

> *"Analysis temporarily unavailable. Your text was not stored. Please try again in a moment."*

No partial or broken report is ever shown.

### Editorial guardrail — Prompt constraint

The system prompt explicitly instructs the agent to:

- Analyse the **text**, not the **person** who wrote it
- Never use diagnostic language ("this person is manipulative")
- Be pedagogical, not alarmist

---

## 13. Backlog & Golden Path

### Golden Path

> A user pastes an email they received from their manager. In seconds, they see a report identifying 2 techniques (false urgency + culpabilisation), the exact excerpts highlighted, an explanation of each mechanism, and a suggested response strategy.

### P1 — Must-have for demo day

| #    | Task                                                                              | Done? |
| ---- | --------------------------------------------------------------------------------- | ----- |
| P1-1 | Lovable form: text input + URL input + submit + RGPD mention                      | ✅     |
| P1-2 | n8n webhook receives input, auto-detects type (regex) and scrapes URLs via Jina   | ✅     |
| P1-3 | Taxonomy indexed into Supabase vector store; agent retrieves it via RAG           | ✅     |
| P1-4 | n8n AI Agent (Claude) returns validated structured JSON                           | ✅     |
| P1-5 | Lovable results page: score, intensity badge, synthesis, per-technique cards      | ✅     |

### P2 — Stretch goals

| #    | Task                                                            | Done? |
| ---- | --------------------------------------------------------------- | ----- |
| P2-1 | Jina scraping on JS-heavy pages                                 | ✅     |
| P2-2 | Blocked-page failsafe (paste fallback)                          | ✅     |
| P2-3 | Robust JSON parser (markdown fences + schema validation)        | ✅     |
| P2-4 | "Copy report" button on results page                            | ☐     |
| P2-5 | Lighthouse accessibility audit pass (score 90+)                 | ☐     |
| P2-6 | Dropshipping score (AliExpress price comparison)                | ☐     |

---

## 14. Manipulation Taxonomy

*Source of truth in Airtable — table `Manipulation_Taxonomy`, 6 columns: Nom / Définition / Mécanisme / Signaux_textuels / Exemple / Suggestion_réponse. The table is embedded (OpenAI) and indexed into a Supabase vector store, which the agent queries via RAG at analysis time.*

### Interpersonal techniques (12)

| #  | Technique                        | Core signal                                                              |
| --- | -------------------------------- | ------------------------------------------------------------------------ |
| 01 | **Fausse urgence**               | Artificial time pressure to prevent reflection                           |
| 02 | **Culpabilisation**              | Attributing responsibility for a negative situation to create moral debt |
| 03 | **DARVO**                        | Deny, Attack, Reverse Victim and Offender                                |
| 04 | **Double bind**                  | Two options, both unfavourable — the illusion of choice                  |
| 05 | **Inversion de responsabilité**  | Returning the burden of proof to the person who was harmed               |
| 06 | **Ambiguïté délibérée**          | Deliberately vague formulation to preserve an exit                       |
| 07 | **Appel à l'autorité implicite** | Unnamed authority invoked to inhibit contestation                        |
| 08 | **Minimisation**                 | Reducing the legitimacy of a feeling to avoid addressing it              |
| 09 | **Gaslighting textuel**          | Denying or rewriting past facts to create self-doubt                     |
| 10 | **Flatterie instrumentale**      | Targeted compliment before a request to create reciprocity obligation    |
| 11 | **Projection**                   | Attributing one's own behaviour or intentions to the other               |
| 12 | **Faux consensus**               | Presenting a position as shared by a silent majority to isolate          |

### Marketing / copywriting techniques (3)

| #  | Technique                       | Core signal                                                            |
| --- | ------------------------------- | ---------------------------------------------------------------------- |
| 13 | **Preuve sociale artificielle** | Unverifiable figures or testimonials to simulate crowd validation      |
| 14 | **Rareté artificielle**         | False scarcity (stock, time) to trigger urgency                        |
| 15 | **Ancrage de prix**             | Crossed-out reference price to make the real price seem like a bargain |

### Dropshipping-specific techniques (3)

| #  | Technique                          | Core signal                                                            |
| --- | ---------------------------------- | ---------------------------------------------------------------------- |
| 16 | **Faux témoignages**               | Generic profile photos, grouped dates, uniformly perfect reviews       |
| 17 | **Badges de confiance factices**   | Non-functional security logos and unverifiable certification labels    |
| 18 | **Présentation produit générique** | Catalogue photos identical to AliExpress, auto-translated descriptions |

---

## 15. System Prompt

### Agent prompt (n8n)

*Runs inside the LangChain Agent. The taxonomy is no longer pasted in full — the agent retrieves the relevant entries from the Supabase vector store tool, then analyses the text.*

```
Tu es un analyste expert en rhétorique et psychologie de la persuasion.

Ton rôle est d'analyser un texte ou le contenu d'une page web
et d'identifier les techniques de manipulation présentes.

Utilise l'outil de recherche (vector store Supabase) pour récupérer
les techniques pertinentes de la taxonomie de référence avant d'analyser.

INSTRUCTIONS :
1. Lis attentivement le texte soumis.
2. Identifie chaque passage qui correspond à une technique de la taxonomie.
3. Une même technique peut apparaître plusieurs fois.
4. Un passage peut combiner plusieurs techniques — signale-le.
5. Ne diagnostique PAS l'auteur du texte — analyse le texte uniquement.
6. Évalue l'intensité globale :
   faible (1-2 techniques), modérée (3-5),
   élevée (6+) ou systémique (techniques combinées et répétées).
7. Si aucune technique n'est détectée, dis-le clairement.
8. Sois pédagogique, pas alarmiste.

RETOURNE UNIQUEMENT un JSON valide avec cette structure exacte :
{
  "score_global": <entier>,
  "intensite": "<faible|moderee|elevee|systemique>",
  "techniques": [
    {
      "nom": "<nom exact de la technique>",
      "extrait": "<passage exact du texte>",
      "explication": "<pourquoi c'est cette technique>",
      "suggestion": "<comment répondre ou se positionner>"
    }
  ],
  "synthese": "<2-3 phrases résumant le profil rhétorique global>"
}

NE PAS ajouter de texte avant ou après le JSON.
NE PAS utiliser de balises markdown.
Retourner uniquement le JSON brut.
```

### Lovable prompt (frontend)

```
Build a web app called ManipDetect — an AI-powered manipulation
detection tool that analyses text or landing page URLs and identifies
rhetorical manipulation techniques used by the author.

---

## BRANDING & DESIGN

App name: ManipDetect
Tagline: "Ce qu'on vous dit vraiment."
Color palette:
  - Background: #0F0F1A (very dark navy)
  - Primary accent: #8B1A1A (deep red)
  - Secondary accent: #2C3E7A (dark blue)
  - Card background: #1A1A2E
  - Text: #E8E8E8
  - Muted text: #888888
  - Success/low: #2A5E2A
  - Warning/moderate: #7A5E1A
  - Danger/high: #8B1A1A
  - Critical/systemic: #4A0A0A

Typography: Inter or system-ui, clean and readable.
Overall aesthetic: dark, serious, analytical — like a security
audit tool, not a cheerful SaaS. Think intelligence report,
not marketing tool.

---

## APP STRUCTURE

Single-page application with two states:
1. INPUT STATE — the analysis form
2. RESULTS STATE — the manipulation report

---

## STATE 1 — INPUT FORM

### Layout
Full-screen centered card on dark background.

### Header (always visible)
- Logo: a magnifying glass icon + "ManipDetect" in bold
- Tagline below: "Détectez ce qu'on vous dit vraiment."
  in muted text

### Input card
Title: "Analysez un texte ou une page web"

Two input options with a visual toggle or tabs:
  TAB 1 — "Texte libre"
    Large textarea (min 6 rows), placeholder:
    "Collez ici un email, un contrat, un message,
     une description de formation..."
    Character counter showing X/4000 characters
  
  TAB 2 — "URL à analyser"
    Single text input, placeholder:
    "https://exemple.com/landing-page"
    Small note below: "Fonctionne sur la plupart des
    pages web publiques."

Below both inputs:
  Primary CTA button: "Analyser maintenant →"
  Full width, deep red background, white text,
  rounded corners, hover effect

Below the button, small muted text:
  "🔒 Aucun texte n'est conservé — analyse en session
  uniquement. Conforme RGPD."

### Example test URLs section (collapsible)
A small "Tester avec des exemples →" toggle that reveals
a grid of clickable URL chips. When clicked, the URL
auto-fills the URL field. Organise them by category:

Category "IA & Automatisation":
- DecisionIA: https://decisionia.com/bootcamp-consultant-ia-430449/
- Revolia: https://formation.revolia.pro/inscription-masterclass-ia-consultant
- Mastery IA: https://www.mastery-ia-pro.fr/sommet?el=wc&utm_source=wc
- Catalyst Academy: https://catalystacademy.ai
- ProActive Academy: https://proactiveacademy.fr/lp-ia-generative/
- Success IA: https://www.success-ia-academy.com/
- Mastery DDA: https://www.mastery-dda-ias.com/
- Mister IA: https://www.mister-ia.com/formations-particuliers
- NAIO Agency: https://inscription.naiomagency.com/aut
- SMS AI System: https://book.flexxable.com/optin?utm_source=SCALING

Category "Business & Coaching":
- Envergure: https://go.envergure-media.com/recrutement
- Alegria: https://www.alegria.group/
- BryanRGT: https://www.bryanrgt-coaching.com/webinaire-fb-3
- Kevin Hanot: https://www.kevinhanot.com/
- Jodie Cavalie: https://academy.jodycavalie.com/
- Richissime: https://coachings.richissime.net/lm/roadmap-personnalisee/
- Amal Castel: https://www.academiepsr.com/
- Boring Business: https://www.boringbusiness.fr/

Category "Finance & Trading":
- Paradox Mastermind: https://www.paradox-mastermind.com/live
- Equity Mastermind: https://www.equitymastermind.fr/
- Tom Crosshill: https://www.indexmasterclass.com/webinar-registration

Category "Immobilier":
- KretzClub: https://www.kretzclub.com/landing-page

Category "Outils IA":
- MyKreator: https://mykreator.ai/acces
- Hyperentrepreneur: https://hyperentrepreneur.ai/70-ai-specialists-for-claude

---

## LOADING STATE

When the user clicks "Analyser maintenant":

1. Button becomes disabled + shows spinner
2. Full-screen animated loading overlay appears with:
   - ManipDetect logo centered
   - Rotating progress messages (cycle every 2 seconds):
     "Lecture de la page en cours..."
     "Extraction du contenu textuel..."
     "Consultation de la taxonomie..."
     "Analyse rhétorique en cours..."
     "Identification des techniques..."
     "Rédaction du rapport..."
   - Subtle pulsing animation on the logo

---

## API CALL LOGIC

On form submit, make a POST request to the n8n webhook:

const WEBHOOK_URL = "PLACEHOLDER_N8N_WEBHOOK_URL";

const payload = {
  input_type: "text" | "url",    // based on active tab
  content: textareaValue | urlValue,
  timestamp: new Date().toISOString()
};

const response = await fetch(WEBHOOK_URL, {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify(payload)
});

const data = await response.json();

Expected response shape from n8n:
{
  "score_global": 7,
  "intensite": "elevee",
  "techniques": [
    {
      "nom": "Fausse urgence",
      "extrait": "Il ne reste que 3 places disponibles",
      "explication": "La limitation artificielle...",
      "suggestion": "Vérifier si la rareté est réelle..."
    }
  ],
  "synthese": "Cette page combine plusieurs techniques..."
}

Note: the workflow auto-detects whether `content` is a URL (server-side
regex), so `input_type` is informational only. When a page cannot be
scraped (bot protection), the response may instead contain
`needs_manual_input: true` with a message asking the user to paste the
visible text — handle that case by showing the message, not an empty report.

Handle errors gracefully:
- Network error: show error card with retry button
- Timeout (>30s): show timeout message
- Invalid JSON: show parsing error with retry

---

## STATE 2 — RESULTS PAGE

### Back button
Top left: "← Nouvelle analyse" — returns to input state
and clears everything

### Results header card
Dark card with:
  Left side:
    - Large number: score_global (e.g. "7")
    - Label below: "techniques détectées sur 18"
  Right side:
    - Intensity badge (large pill):
      faible → green background, "⚡ Faible"
      moderee → yellow/amber background, "⚠️ Modérée"  
      elevee → red background, "🔴 Élevée"
      systemique → dark red + pulsing border,
                   "🚨 Systémique"
    - If URL was analysed: show the URL in muted small text

### Synthesis card
Full-width card below header:
  Label: "Synthèse de l'analyse"
  Content: synthese field from API response
  Italic, slightly larger text, muted border left accent

### Techniques section
Title: "Techniques identifiées" + count badge

One card per technique in techniques array.
Each card contains:
  TOP ROW:
    - Technique name in bold red: nom
    - Small numbered badge (1, 2, 3...)
  
  EXCERPT BOX:
    - Label: "Extrait détecté"
    - Light background box with italic text: extrait
    - Small quote icon on the left
  
  EXPLANATION:
    - Label: "Pourquoi c'est cette technique"
    - Body text: explication
  
  SUGGESTION BOX:
    - Different background (dark blue tint)
    - Label with icon: "💡 Comment répondre"
    - Text: suggestion

Cards animate in one by one with a subtle fade-up.

### Empty state (if techniques array is empty)
Green card:
  ✅ "Aucune technique de manipulation détectée"
  "Ce contenu ne présente pas de signaux rhétoriques
  problématiques identifiables."

### Footer of results
Two elements:
  Left: "⚠️ Cette analyse est générée par IA.
         Elle peut contenir des erreurs."
  Right: "Analyser un autre texte →" button

---

## MOBILE RESPONSIVENESS

- Full mobile support
- Touch-friendly tap targets (min 44px)
- URL chips wrap gracefully on small screens
- Technique cards stack vertically
- Header score/badge stacks vertically on mobile

---

## ACCESSIBILITY

- All form inputs have associated labels
- Color is never the only indicator (always text label too)
- Focus states visible on all interactive elements
- Loading messages announced via aria-live region
- Error messages descriptive and associated with inputs
- Minimum contrast ratio 4.5:1 for all text

---

## ADDITIONAL NOTES

- Use React with hooks (useState, useEffect)
- No external routing needed — single page, two states
- Store the webhook URL as a const at the top of the file
  so it's easy to replace
- The "Tester avec des exemples" section is a nice-to-have
  but important for demo day — keep it
- Add a small "?" tooltip on the intensity badge explaining
  what each level means
- Do not use any mock data — always call the real webhook.
  If the webhook is not configured, show a clear
  "Configuration requise" message instead of fake results.
```

---

## 16. Constraints

- **Privacy:** no user input is stored server-side — session only. Explicit RGPD mention in UI.
- **Scope of analysis:** the tool analyses text, never diagnoses the person who wrote it. No psychiatric or legal claims.
- **Dual AI provider:** Claude (analysis) + OpenAI (embeddings) — two providers to monitor for cost, rate limits and DPAs.
- **No paid e-commerce APIs at MVP:** AliExpress and WHOIS integrations deferred to V1.
- **Scraping limitations:** Jina handles most public/JS pages, but Cloudflare *challenge* pages and hard captchas remain unreadable → the blocked-page failsafe asks the user to paste the visible text.
- **Token budget:** inputs truncated at 4,000 characters to control LLM cost and latency.
- **Language:** French-first for taxonomy and UI — English-language texts analysed but explanations returned in French at MVP.

---

## 17. Tests

[Infopreneur website :](https://lacour-avocats.com/expertises/infoprenariat/)

### IA / Consultant IA / Automatisation

- [DecisionIA](https://decisionia.com/bootcamp-consultant-ia-430449/)
- [Revolia](https://formation.revolia.pro/inscription-masterclass-ia-consultant)
- [Mastery IA pro](https://www.mastery-ia-pro.fr/sommet?el=wc&utm_source=wc)
- [Catalystacademy](https://catalystacademy.ai)
- [ProActive academy](https://proactiveacademy.fr/lp-ia-generative/)
- [Success IA academy](https://www.success-ia-academy.com/)
- [Mastery DDA IAS](https://www.mastery-dda-ias.com/)
- [Mister IA](https://www.mister-ia.com/formations-particuliers)
- [NAIO agency](https://inscription.naiomagency.com/aut)
- [SMS AI System](https://book.flexxable.com/optin?utm_source=SCALING)

### Business / Marketing / Coaching

- [Envergure](https://go.envergure-media.com/recrutement)
- [Alegria](https://www.alegria.group/)
- [BryanRGT](https://www.bryanrgt-coaching.com/webinaire-fb-3)
- [Kevin Hanot](https://www.kevinhanot.com/)
- [Jodie Cavalie](https://academy.jodycavalie.com/)
- [Richissime](https://coachings.richissime.net/lm/roadmap-personnalisee/)
- [Amal Castel](https://www.academiepsr.com/)
- [Boring business](https://www.boringbusiness.fr/)

### Finance / Trading / Reprise d'entreprise

- [ParadoxMastermind](https://www.paradox-mastermind.com/live)
- [Equity mastermind](https://www.equitymastermind.fr/)
- [Tom Crosshill](https://www.indexmasterclass.com/webinar-registration)

### Immobilier

- [KretzClub](https://www.kretzclub.com/landing-page)

### Outils / Produits IA

- [MyKreator](https://mykreator.ai/acces)
- [Hyperentrepreneur](https://hyperentrepreneur.ai/70-ai-specialists-for-claude)

---

## 18. Out of Scope

- Audio or voice transcription analysis
- Direct integration with email clients or messaging apps
- Psychological diagnosis of the text's author
- **Module emprise / violence psychologique** — see Future Evolution
- "Write a manipulative message" mode — intentionally excluded
- Multi-language UI at MVP
- User accounts and analysis history

---

## 19. Future Evolution

### V1 — Dropshipping Detector (next sprint)

- 3 additional taxonomy entries (#16 #17 #18)
- AliExpress / Temu price comparison agent
- Domain age check via WHOIS API
- Dropshipping probability score 0–100
- Verdict with direct link to source product

### V2 — Distribution

- Browser extension: real-time analysis on any page
- Public API for third-party integration (consumer protection media, legal firms)
- PDF report export
- Analysis history with user account

### V3 — Module Emprise & Violence Psychologique

> ⚠️ Out of scope until the following conditions are met:
>
> - Partnership with mental health professionals and associations (3919, France Victimes)
> - GDPR Article 9 compliance (sensitive health data)
> - Systematic orientation protocol toward real support resources
> - User testing with affected populations
> - Clinical validation of the pattern detection model

*This module will not be built without these conditions. Rigour is the condition of its value.*

### V4 — Trust & Value Score (*Indice de Valeur Potentielle*)

ManipDetect today answers a single question: *is this text manipulative?* But manipulation intensity alone is not a verdict on the **worth of the offer**. An aggressive sales page can sit on top of a genuinely serious product, and a calm, "clean" page can hide an empty one. This evolution adds a **second, orthogonal axis** — a Trust / Potential-Value Score running in parallel to the manipulation score — built from three indices.

#### 1. Structural Transparency Index

The baseline — the more transparent the offer, the more likely it is to be serious.

| Criterion                  | Weight     | What to look for                                                      |
| -------------------------- | ---------- | --------------------------------------------------------------------- |
| Trainer identity           | High       | Full name, verifiable background, track record (LinkedIn, past projects) |
| Publicly displayed price   | Very High  | Price visible without a form or a sales call                          |
| Detailed curriculum        | High       | Module-by-module plan with precise learning objectives                |
| Complete legal notices     | Medium     | Company name, address, SIRET, terms of sale (CGV)                     |
| Refund conditions          | Medium     | Clearly stated, no booby-trapped asterisk                             |

#### 2. Verifiable Social Proof Index

Separate *surface* social proof (media logos, vague numbers) from proof that can actually be checked.

| Criterion                | Weight     | What to look for                                                                          |
| ------------------------ | ---------- | ----------------------------------------------------------------------------------------- |
| Third-party reviews      | Very High  | Trustpilot, Google Reviews, Reddit — the tool could scrape these for an average rating    |
| Named testimonials       | High       | Identifiable people with an active LinkedIn profile and a real photo                      |
| Traceable alumni         | High       | The tool could search LinkedIn posts mentioning the training to see what alumni say after |
| Visible community        | Medium     | Is there a public group, forum, or active Slack/Discord?                                   |

#### 3. Substance vs. Promise Index

The core of the evolution: quantify the **gap between what is promised and what is demonstrable**.

| Criterion                  | Weight     | What to look for                                                                                         |
| -------------------------- | ---------- | -------------------------------------------------------------------------------------------------------- |
| Promise / Content ratio    | High       | Compare the count of *promises* ("you will become", "you will earn") to the count of concrete curriculum items |
| Precise taught skill       | High       | An evaluable skill ("run a scoping workshop", "use Make") vs. a vague outcome ("success", "freedom")    |
| Seller's business model    | Very High  | Does the seller earn from the activity they teach, or only from the training? Analyse site, domain history, legal notices |
| Existence of free content  | Medium     | Quality free content (blog, YouTube) is a good predictor of paid-content value                          |

#### Interpretation — the two-axis matrix

Crossing the manipulation score with the value score turns a single number into an actual verdict:

| Manipulation \ Value | **Low value**                       | **High value**                                  |
| -------------------- | ----------------------------------- | ----------------------------------------------- |
| **High manipulation**| 🚨 Red alert — predatory offer      | Aggressive marketing on a genuinely solid product |
| **Low manipulation** | Bland / empty offer                 | ✅ Trustworthy                                  |

*High manipulation + low value* is the case the tool exists to catch; *high manipulation + high value* protects the user from dismissing a good product just because the copy is loud.

#### Technical implementation

A second branch alongside the manipulation agent step (same pattern as the V1 dropshipping branch), merged into a combined report:

- **Targeted scraping** — fetch third-party reviews (Trustpilot), legal notices, and the curriculum page.
- **Semantic analysis** — extend the agent step to tag each sentence as *promise* vs. *content*, then compute the ratio.
- **Data cross-referencing** — verify the trainer on LinkedIn, cross the domain/company name against public registries (INSEE / SIRENE), reuse the WHOIS domain-age check planned for V1.

**Schema impact:** add an optional `valeur` block to the webhook response (`transparence`, `preuve_sociale`, `substance`, `score_confiance`, `verdict`) next to the existing `score_global` / `techniques` / `synthese`, so the front renders the two-axis verdict without breaking the current contract.

#### Worked example (illustrative) — DecisionIA

Applying the rubric to one of the test URLs, to show how the score reads. *Figures are illustrative and would be confirmed by the scraping/cross-referencing layer above.*

- **Structural Transparency** — trainer identified, price publicly displayed, module-level curriculum, accessible legal notices → **High**
- **Verifiable Social Proof** — visible community (YouTube, public Slack), findable reviews; on-page testimonials less nominative → **Medium–High**
- **Substance vs. Promise** — precise skills taught (posture, sales, legal), an entrepreneurial track record and active free content, revenue not derived solely from the training → **High**

**Verdict:** even if the page scores high on marketing intensity, the value axis is strong → it lands in the *"aggressive marketing, solid product"* quadrant rather than the red-alert quadrant.

---

## 20. Learnings

### Build learnings (technical)

Concrete issues surfaced while wiring the agentic workflow — each one is now a documented guardrail:

- **Field contract drift.** The type-detection IF node referenced a non-existent field (`$json.Text`) and compared it to the literal string `"url"` instead of detecting a URL pattern — so the branch was always false and Jina never ran. Fix: regex `^https?://` on `body.content`. *Lesson: align the field names and the detection logic across every node; do not test a value against a label.*
- **Jina 400 (malformed request).** The scrape node sent an empty body on a GET request. Removing the body (and trimming the URL against stray whitespace) fixed it. *Lesson: a "Bad Request" is usually the request shape, not the target.*
- **Hard-coded node name broke the parser.** "Parse JSON from AI Agent" referenced the agent by name (`'AI Agent'`); renaming the node silently sent every run to the fallback. Fix: read the parser's **direct input** (`$input`) instead. *Lesson: avoid name-coupling between nodes.*
- **Garbage-in, confident-out.** Scraped Cloudflare/verification pages were analysed as if they were real content, producing a plausible but meaningless report. Fix: a blocked-page failsafe that detects challenge signatures / too-short output and asks the user to paste the text. *Lesson: for a trust tool, a wrong-but-confident answer is the worst outcome — fail loud, not silent.*
- **Architecture shift.** Moved from "stuff the taxonomy into the prompt" to **RAG retrieval** over a Supabase vector store, and from **Softr to Lovable** for the frontend — more control over the report UI and the API call.

#### Local dev & front ↔ n8n integration (Cursor session)

Running the Lovable-exported project locally and wiring it to n8n surfaced a second cluster of issues:

- **npm blocked on Windows (PowerShell).** `npm run dev` failed because PowerShell's execution policy blocks `npm.ps1`. Fix: run `npm.cmd` (or, once, `Set-ExecutionPolicy -Scope CurrentUser RemoteSigned`) and `npm install` first — `node_modules` was missing. *Lesson: a dev command that fails on Windows is often the shell, not the project.*
- **CORS on the webhook call.** The React app called the n8n webhook directly from the browser; n8n sends no CORS headers for `localhost`, so the browser blocked the request behind a generic "can't reach the server" error. Fix: route the call through a **TanStack Start server function** (server-side proxy) instead of `fetch` from the client. *Lesson: third-party webhooks belong server-side, not in the browser.*
- **Test vs production webhook URL.** `/webhook-test/...` only accepts one call after clicking "Listen for test event"; normal use needs the production URL (`/webhook/...`) with the workflow toggled **active**. Stored as `N8N_WEBHOOK_URL` in `.env` (gitignored). *Lesson: ship against the production webhook; keep the test URL for manual debugging only.*
- **Empty 200 response.** The webhook returned HTTP 200 with an empty body → JSON parse failed ("invalid response"). Fix: set the Webhook to respond **When Last Node Finishes** with a *Respond to Webhook* node, and harden the client to accept the common n8n shapes (array, `json`, `body`). *Lesson: 200 ≠ a usable body — validate the payload, not just the status code.*
- **Stale timeout / unrestarted dev server.** A "timeout > 30s" message kept appearing after the limit had already been raised — the Vite dev server was still running old code. Fix: move the timeout to an env var (`ANALYSIS_TIMEOUT_MS`), restart Vite, hard-refresh. *Lesson: after editing server config, restart and hard-refresh before trusting an error message.*
- **OpenAI Responses API output shape.** The JSON parser kept hitting its fallback because it read `choices[0].message.content`, but the OpenAI **Responses** API returns the JSON inside `content[].text` (`type: "output_text"`), and the node emitted several assistant messages. Fix: read every `output_text` block, take the last non-empty one, and use `$('node').all()` rather than `.first()`. *Lesson: match the parser to the exact provider/API shape — "OpenAI output" is not a single format.*

#### Production hardening & sync (Lovable session)

Wiring the front to a slow workflow and publishing it surfaced a third cluster, this time about latency, security and tooling:

- **Async job pattern for slow analyses.** n8n can take ~90 seconds (scrape + several AI calls) — longer than a comfortable synchronous request. The client now reads n8n's response as soon as it arrives and otherwise polls an `analysis_jobs` row in Supabase (status `processing` → done), with timeouts raised to **1 min 30** on both server and client to match real latency. *Lesson: size the timeout to the actual workflow latency, and decouple long AI work from the request/response cycle.*
- **Deny-all RLS on the jobs table.** Added an explicit **deny-all** RLS policy on `analysis_jobs` so client/anon roles cannot read or write jobs; only the server's service role manages them. *Lesson: lock down every new table by default — deny first, then grant narrowly to the server.*
- **Authenticated callback.** The n8n → app callback now **requires `N8N_WEBHOOK_SECRET`** and only updates jobs still in `processing` status. *Lesson: a public callback must authenticate its caller and refuse to mutate already-finished work (no replay or overwrite).*
- **Error page mistaken for content.** A test URL (a broken TanStack/Lovable site) served its own SSR fallback ("This page didn't load"); n8n scraped that error HTML, so there was nothing rhetorical to analyse — and the server function surfaced 500s. *Lesson: the upstream site can be broken too — detect when extracted "content" is actually an error/404 page (very little text, tell-tale strings) and tell the user, instead of analysing noise.* This extends the blocked-page failsafe to upstream SSR failures.
- **Lovable ↔ GitHub are not auto-synced.** Edits committed directly on GitHub (removing a test URL) didn't appear in the app: Lovable reads its own file state and won't pull external commits unless two-way sync is enabled or a manual pull is triggered. *Lesson: pick a single source of truth for edits, or enable bidirectional sync — don't edit both ends and expect convergence.*

### Demo-day reflection

- [x] What unexpected challenges did you face, and how did you adapt?
      
*n8n workflow using Airtable database as memory took 1:53 to 2:35 as execution time, too much for a user.
I had to create RAG with supabase from airtable data in order to speed up the execution time : it took 10 to 35 sec now. *
      
- [x] Did you reach your P1 goals? If not, what stopped you?
      
*I reach P1 goal by focusing on the MVP, which is a text to test. In n8n, I had to remove the webhook node to execute the workflow manually. It speeds up the tests.
As P2, I focused on URL scraping, and had to insert a scenario when the website is asking a human verification, or has a pop-up blocking the scraping.*
      
- [x] What would you do differently in the next iteration?
      
*Due to the time constraint (1.5d to develop and to publish for the demo), I did not have the time to have a solid understanding of the product context.
Nevertheless, using Lovable, Claude, Deepseek, Cursor, N8N chatbot helped a lot. I believe with only my brain, this project would have taken at least 1 week instead of 1 day to develop.*

- [x] Which KPI targets were realistic? Which need adjustment?
      
*Unconsciouly, having an execution time > 1 min is not acceptable, for me (tokens are precious), nor the users (time is precious). I believe it's one of the KPI project.*

---

## Author

**Caroline Tith** — Enterprise Architect, Data & AI Specialist

[LinkedIn](https://linkedin.com/in/caroline-tith) · [Email](mailto:caroline.tith@hotmail.com)

---
