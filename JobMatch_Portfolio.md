# JobMatch CV 💼
### Your CV in the exact language of the company that's hiring

![Status](https://img.shields.io/badge/status-MVP%20in%20progress-orange)
![Stack](https://img.shields.io/badge/stack-n8n%20%7C%20Airtable%20%7C%20Softr%20%7C%20LLM-blue)
![Type](https://img.shields.io/badge/type-Agentic%20AI%20product-purple)
![Author](https://img.shields.io/badge/author-Caroline%20Tith-darkred)

> *Built during Le Wagon AI Product Builder — June 2026*

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
14. [Analysis Framework](#14-analysis-framework)
15. [System Prompt](#15-system-prompt)
16. [Constraints](#16-constraints)
17. [Out of Scope](#17-out-of-scope)
18. [Future Evolution](#18-future-evolution)
19. [Learnings](#19-learnings)

---

## 1. Problem Statement

Most candidates send a generic CV to dozens of job offers — the same words, the same framing — even when their skills genuinely match the role. Every company has its own vocabulary, its ATS keywords, and its implicit language conventions. A CV that speaks the wrong dialect is screened out before a human ever reads it, regardless of the candidate's actual fit.

**The gap between a rejected and a shortlisted application is often not a skills gap — it's a translation gap.** JobMatch CV closes it in under 60 seconds.

---

## 2. Target User & Context

### Primary users

| Profile | Context | Pain |
|---|---|---|
| IT professional in career transition | Moving from BI to Enterprise Architecture | CV still speaks BI language; EA recruiters screen it out at keyword level |
| Experienced candidate returning to market | After 5 years in one company | Generic CV doesn't map to current market vocabulary |
| Consultant applying to a specific firm | Targeting McKinsey, Onepoint, Capgemini Invent | Each firm has its own jargon and framing expectations |
| Junior candidate with strong experience | Undersells transferable skills without realising it | Doesn't know which of their experiences maps to what the job is asking for |

### Secondary users
- Career coaches and outplacement consultants working with multiple clients
- Recruiters who want to help shortlisted candidates improve their materials
- Universities supporting graduates in their first job search

---

## 3. Why Now

- **ATS systems filter 75% of CVs before human review** (LinkedIn Talent Insights, 2024) — keyword alignment is now a survival requirement, not a nice-to-have
- **AI-assisted CV writing is mainstream** (Kickresume, Teal, Resume.io) but none of them do offer-specific vocabulary alignment — the gap is precise and addressable
- **Career transitions are accelerating** post-AI wave: data analysts moving to AI product, developers moving to platform roles, BI managers moving to architecture — all need vocabulary translation help
- **LLMs are now capable** of semantic gap analysis + targeted rewriting with explicit constraints (preserve facts, don't invent, match register) — this quality was not reliably achievable before 2024
- **France's job market context:** ROME codes and APEC terminology create additional vocabulary layers that French candidates must navigate — a French-language specialised tool fills a specific gap

---

## 4. Value Hypothesis

Aligning the vocabulary of a CV to the exact wording of a job offer increases the probability of passing ATS screening and resonating with human reviewers — without inventing skills or compromising the authenticity of the candidate's profile.

**Current workarounds:**
- Manual rewrite per application: dominant behaviour, 30–60 min/application, mentally draining and inconsistent
- Copy-pasting job keywords directly: detectable, feels unnatural in context
- Asking a friend or coach to review: unavailable on demand, expensive
- Generic ChatGPT prompt "improve my CV": rewrites without offer-specific alignment, often overwrites the candidate's voice

**Impact of the problem:**
- Average time per manual CV adaptation: 30–60 minutes
- Average job search duration in France: 4–6 months, 50–100 applications
- Cognitive cost: "application fatigue" is cited as primary reason for reduced application quality over time
- Financial cost: missed shortlisting = missed interviews = extended job search

---

## 5. Market Scan

| Tool | What it does well | What it misses | Relevant constraint |
|---|---|---|---|
| **Kickresume / Resume.io** | Beautiful CV templates, basic AI suggestions | No job offer input, no vocabulary gap analysis | Generic rewrites, not offer-specific |
| **Teal HQ** | Application tracking + some AI matching | US-centric, limited in France, no deep rewrite | No offer-specific vocabulary alignment |
| **LinkedIn Easy Apply** | Frictionless application | Zero personalisation, no CV adaptation | Black box ATS |
| **Rezi.ai** | ATS score optimisation | Keyword stuffing approach — detectable by human reviewers | Not a nuanced rewrite |
| **ChatGPT free** | Can help if well-prompted | Requires prompt expertise, no structured gap analysis output, no interface | Not accessible to non-technical users |

**Gap confirmed:** no French-language tool combines job offer vocabulary analysis + targeted CV rewriting + explicit gap flagging (truly missing vs poorly expressed) in one structured interface.

---

## 6. Solution Concept

### MVP (2 days — Le Wagon)

1. User pastes their CV text into field 1
2. User pastes the job offer text into field 2
3. LLM performs a three-layer analysis:
   - **Vocabulary gap:** keywords in the offer missing from the CV
   - **Latent skills mapping:** experiences in the CV that match offer requirements but use different terminology
   - **Real gaps:** requirements in the offer that are genuinely absent from the CV
4. LLM rewrites the most relevant CV experience sections using the offer's vocabulary
5. Output: annotated gap report + rewritten sections + genuine gap list

---

## 7. AI Use-Case & Strategic Role

### What the AI step does

The LLM receives the full CV text and the full job offer text, then:
1. Extracts the offer's key vocabulary: job title variants, required skills, tools, methodologies, domain language
2. Maps each offer requirement to the CV: found (same words), latent (equivalent experience, different words), or missing
3. Rewrites targeted CV sections: preserving factual accuracy while adopting the offer's vocabulary
4. Flags genuine gaps with a neutral message: "The offer requires X — this does not appear in your profile"

### Where it sits in the golden path

```
User inputs CV text + Job offer text (Softr)
            ↓
[n8n] Receive both texts via webhook
            ↓
[Airtable] Fetch analysis framework + domain vocabulary guide
            ↓
[LLM] ← CORE AI STEP
  Input:  { cv_text, job_offer_text, analysis_framework }
  Output: {
    vocabulary_gaps: [],
    latent_mappings: [],
    real_gaps: [],
    rewritten_sections: [],
    summary: "..."
  }
            ↓
[n8n] Parse JSON + format annotated report
            ↓
[Softr] Three-panel results: Gap analysis | Rewritten sections | Genuine gaps
```

### Strategic Role Statement

> Without the AI step, JobMatch CV is two text boxes side by side. The LLM performs the semantic analysis that a career coach would take 45–60 minutes to do manually — identifying not just missing keywords but the deeper vocabulary translation between how the candidate describes their experience and how the recruiter's world names the same concepts. **AI makes this instantaneous, scalable, and available at the moment of need — 11pm before a deadline, not 2 weeks later after a coaching appointment.**

### User value unlocked

- **Time saved:** 30–60 minutes per manual adaptation → under 60 seconds
- **Quality improvement:** catches vocabulary gaps the candidate is blind to because they can't see their own language assumptions
- **Confidence:** explicit gap analysis removes the anxiety of "did I miss something?"
- **Authenticity preserved:** the tool rewrites what exists, flags what's missing — it never invents
- **ATS pass rate:** keyword alignment directly improves automated screening scores

---

## 8. Tech Stack & Architecture

### POC Stack

| Layer | Tool | Why chosen for POC | Known constraints |
|---|---|---|---|
| **Frontend** | Softr | Two large text fields + structured results display, zero-code | Multi-panel display requires careful Softr layout configuration |
| **Orchestration** | n8n | Receive dual text inputs, call LLM, parse complex JSON, format output | Long CV + long job offer = large token input; monitor cost |
| **AI layer** | Claude 3.5 Sonnet | Best-in-class at instruction-following + preserving factual constraints while rewriting | ~$0.005–0.015 per analysis depending on CV length |
| **Framework DB** | Airtable | Analysis rubric + domain vocabulary guides (IT, consulting, finance) | Manual curation required; vocabulary guides add quality but are not required for MVP |
| **Privacy** | Session-only processing | CV contains personal data — no storage at MVP | GDPR compliance requires explicit consent in V1 if any storage is added |

### Architecture Diagram — POC

```
┌──────────────────────────────────────────────────────────┐
│                    USER (Softr)                          │
│   Field 1: Paste CV text                                 │
│   Field 2: Paste job offer text                          │
│   [Analyse my application]                               │
└──────────────────────┬───────────────────────────────────┘
                       │ Webhook POST {cv_text, job_offer}
                       ▼
┌──────────────────────────────────────────────────────────┐
│                  n8n WORKFLOW                            │
│                                                          │
│  [1] Receive webhook (cv_text + job_offer_text)          │
│  [2] Input validation (non-empty, min length)            │
│  [3] Fetch from Airtable:                                │
│       → Analysis framework (3-layer rubric)             │
│       → Domain vocabulary guide (optional at MVP)       │
│  [4] Build LLM prompt:                                   │
│       { cv_text, job_offer_text, framework }             │
│  [5] POST → Claude API                                   │
│       Input:  CV + offer + analysis framework           │
│       Output: { vocabulary_gaps[], latent_mappings[],   │
│                 real_gaps[], rewritten_sections[],       │
│                 summary }                                │
│  [6] Parse JSON                                          │
│  [7] Format three-panel report                           │
│  [8] Webhook response → Softr                            │
└──────────────────────┬───────────────────────────────────┘
                       │
                       ▼
┌──────────────────────────────────────────────────────────┐
│              SOFTR RESULTS PAGE                          │
│                                                          │
│  Summary: "X vocabulary gaps, Y latent matches,         │
│            Z genuine gaps identified"                    │
│                                                          │
│  ┌─────────────────┐ ┌──────────────┐ ┌───────────────┐ │
│  │  GAP ANALYSIS   │ │  REWRITTEN   │ │ GENUINE GAPS  │ │
│  │                 │ │  SECTIONS    │ │               │ │
│  │ • Missing KWs   │ │              │ │ • X not found │ │
│  │ • Latent maps   │ │ [Copy]       │ │ • Y not found │ │
│  └─────────────────┘ └──────────────┘ └───────────────┘ │
└──────────────────────────────────────────────────────────┘
```

---

## 9. Production-Grade Target

| Area | POC | Production change | Why |
|---|---|---|---|
| **Scale** | Single synchronous LLM call | Async processing + queue | Long CVs (3+ pages) + long offers = 10,000+ token inputs; response time 15–30s needs async with progress indicator |
| **Privacy & compliance** | No storage, session only | Explicit GDPR consent + DPA with LLM provider if storage added; CV data classified as personal data | Name, contact details, employment history = personal data under GDPR; LLM provider must sign DPA |
| **AI layer** | Single model, no fallback | Fallback to GPT-4o if Claude unavailable; confidence scoring per rewritten section | Career-critical output must be reliable; model downtime cannot block a job application |
| **Cost control** | No limits | Input truncation at 6,000 tokens; rate limit 20 analyses/user/day | Long CVs and offers are expensive; unlimited free use is not sustainable |
| **Security** | Open webhook | Auth token on webhook; input sanitisation; rate limiting by IP | Open endpoint is trivially abused; prompt injection via malicious job offer text is a real attack vector |

---

## 10. Accessibility

| WCAG Principle | Implementation |
|---|---|
| **Perceivable** | Results divided into clearly labelled panels — not colour-coded sections without text labels |
| **Perceivable** | "Genuine gap" items never shown in red alone — always with text label "Not found in your profile" |
| **Operable** | Two text fields and submit button fully keyboard-navigable |
| **Operable** | "Copy rewritten section" button keyboard-accessible and screen-reader labelled |
| **Understandable** | Gap analysis uses plain language: "This skill appears in the offer but not in your CV" not "Vocabulary discrepancy detected" |
| **Understandable** | Rewritten sections clearly marked "AI suggestion — review before using" to avoid confusion with original text |
| **Robust** | No auto-populated form fields — user must actively paste content (prevents accidental data submission) |

**Privacy accessibility note:** CVs contain sensitive personal and professional data. The UI must clearly communicate data handling before submission: *"Your CV and the job offer are processed to generate this analysis. Nothing is stored."*

---

## 11. KPIs & Measurement

### Product KPIs

| KPI | What it measures | Target (demo day) | How to capture |
|---|---|---|---|
| **Analysis completion rate** | % of CV + offer submissions that return a complete three-panel report | > 85% | n8n log: input received vs report returned |
| **User-judged rewrite quality** | User rates "At least one rewritten section is usable as-is" (yes/no) | > 65% yes | One-click rating on results page |
| **Genuine gap usefulness** | User rates "The genuine gaps section told me something I didn't know" (yes/no) | > 50% yes | Second one-click rating |

### AI-Specific KPIs

| KPI | Definition in this context | Target | Data source |
|---|---|---|---|
| **AI task success rate** | % of LLM calls returning valid JSON with all four required sections (vocabulary_gaps, latent_mappings, real_gaps, rewritten_sections) | > 90% | n8n error log + JSON parse step |
| **Hallucination rate** | % of rewritten sections that introduce skills or experiences not present in the original CV (spot-check on 10 test pairs) | < 5% | Manual review: compare rewritten sections to original CV line by line |
| **Cost per analysis** | LLM cost for one CV + offer pair analysis | < $0.015 | Anthropic usage dashboard |

---

## 12. Guardrails & Failsafes

### Guardrail 1 — Input validation
Before hitting the LLM, n8n checks:
- CV field: minimum 200 characters, maximum 8,000 characters
- Job offer field: minimum 100 characters, maximum 4,000 characters

*Why:* empty fields, one-line inputs, or extremely long CVs cause either useless or cost-prohibitive LLM calls.

### Guardrail 2 — Hallucination constraint in prompt
The system prompt explicitly and repeatedly instructs the LLM:
- "Never add skills, experiences, or qualifications not present in the original CV"
- "If a required skill is absent, list it in real_gaps — do not include it in rewritten_sections"
- "Rewrite means change the words, not the facts"

*Why:* inventing credentials in a job application CV is legally and professionally harmful to the user.

### Guardrail 3 — Output schema validation
n8n validates JSON contains all four required keys before display. If not, retry once.

### Editorial disclaimer
Every rewritten section is displayed with: *"AI suggestion — verify accuracy against your original experience before using."*

*Why:* even well-constrained LLMs can occasionally shift emphasis in ways that misrepresent the user's actual experience level.

### Failsafe — Graceful degradation
If both LLM call and retry fail: *"Analysis temporarily unavailable. Your data was not stored. Please try again."*

---

## 13. Backlog & Golden Path

### Golden Path

> A data professional targeting an Enterprise Architecture role at a consulting firm pastes their CV and the job offer. In under 60 seconds, they see: 6 vocabulary gaps (terms the offer uses that their CV doesn't), 4 latent skill mappings ("your 'BI Roadmap Manager' experience maps to 'SI Urbanisation' in the offer's language"), 2 genuine gaps, and 3 rewritten experience bullet points ready to copy into their CV.

### P1 — Must-have for demo day

| # | Task | Done? |
|---|---|---|
| P1-1 | Softr: two text input fields + submit button + GDPR notice | ☐ |
| P1-2 | n8n: receive dual text webhook, input validation | ☐ |
| P1-3 | Airtable: analysis framework (3-layer rubric) | ☐ |
| P1-4 | n8n: fetch framework, build LLM prompt, call Claude | ☐ |
| P1-5 | n8n: parse JSON output | ☐ |
| P1-6 | Softr: three-panel results display with copy buttons | ☐ |

### P2 — Stretch goals

| # | Task |
|---|---|
| P2-1 | User quality rating widgets (2 yes/no buttons) |
| P2-2 | "AI suggestion" disclaimer on each rewritten section |
| P2-3 | Retry logic on LLM failure |
| P2-4 | Domain vocabulary guide in Airtable (IT, consulting) |
| P2-5 | Summary header: "X gaps, Y matches, Z genuine gaps" |

---

## 14. Analysis Framework

*Stored in Airtable — table: `Analysis_Framework`*

### The 3-Layer Analysis Rubric

**Layer 1 — Vocabulary Gap**
> Terms, tools, methodologies, or role titles that appear in the job offer but are absent from the CV in any form.

Output format:
```
{ "type": "vocabulary_gap", "offer_term": "...", "cv_equivalent": null }
```

**Layer 2 — Latent Skill Mapping**
> Experiences or skills in the CV that directly correspond to offer requirements but use different terminology.

Output format:
```
{
  "type": "latent_mapping",
  "offer_term": "...",
  "cv_equivalent": "...",
  "rewrite_suggestion": "..."
}
```

**Layer 3 — Genuine Gap**
> Requirements explicitly stated in the offer for which no equivalent experience exists anywhere in the CV.

Output format:
```
{
  "type": "real_gap",
  "offer_requirement": "...",
  "message": "This requirement does not appear in your profile."
}
```

### Rewritten Section Rules

| Rule | Constraint |
|---|---|
| **Fact preservation** | No experience, skill, or qualification may be added that is not present in the original CV |
| **Vocabulary alignment** | Replace CV terminology with offer vocabulary where a direct equivalence exists |
| **Register matching** | Match the tone of the offer (formal consulting → formal French; startup → more dynamic) |
| **Scope signal** | Preserve quantified impacts (€2M revenue, 12 projects, 60 incidents) — these are credibility signals |
| **Authenticity** | If a rewrite would feel false to the candidate, flag it as a suggestion not a replacement |

---

## 15. System Prompt

```
You are an expert career strategist and CV consultant specialising
in vocabulary alignment between candidate profiles and job offers.

You will receive:
- A candidate's CV text
- A job offer text
- An analysis framework with 3 layers

CV:
{{cv_text}}

Job Offer:
{{job_offer_text}}

Analysis Framework:
{{analysis_framework}}

CRITICAL CONSTRAINT: You must NEVER add skills, experiences,
or qualifications that are not present in the original CV.
Rewriting means changing vocabulary and framing — never facts.
If a skill is absent, list it in real_gaps. Do not include it
in rewritten_sections.

INSTRUCTIONS:
1. Extract all key terms, skills, tools, and role concepts
   from the job offer.
2. For each offer term, check: is it present in the CV?
   - Same or similar words → match (do not flag)
   - Different words, same meaning → latent_mapping
   - Absent entirely → vocabulary_gap OR real_gap
3. Distinguish vocabulary_gap (the concept exists in the CV
   but uses different words) from real_gap (the concept is
   genuinely absent from the CV).
4. Rewrite only the 2-4 most relevant CV experience sections.
   Use the offer's vocabulary where a direct equivalence exists.
   Preserve all factual details, numbers, and timeframes.
5. Write all output in French.

RETURN ONLY valid JSON in this exact structure:
{
  "summary": "<1-2 sentences: X vocabulary gaps, Y latent mappings, Z genuine gaps>",
  "vocabulary_gaps": [
    { "offer_term": "...", "suggested_addition": "..." }
  ],
  "latent_mappings": [
    {
      "offer_term": "...",
      "cv_equivalent": "...",
      "rewrite_suggestion": "<rewritten phrase in offer vocabulary>"
    }
  ],
  "real_gaps": [
    {
      "offer_requirement": "...",
      "message": "Cette compétence ne figure pas dans votre profil."
    }
  ],
  "rewritten_sections": [
    {
      "original": "<original CV bullet or section>",
      "rewritten": "<rewritten version using offer vocabulary>",
      "note": "<brief explanation of what changed and why>"
    }
  ]
}

DO NOT add text before or after the JSON.
Return raw JSON only.
```

---

## 16. Constraints

- **Privacy:** CV contains personal data — no storage at MVP. Explicit GDPR notice before submission.
- **Hallucination risk:** the LLM must be explicitly and repeatedly instructed not to invent credentials — this is the primary ethical risk of the product
- **Language:** French-first at MVP — offers and CVs in English are handled but output in French
- **Token budget:** long CVs (3 pages) + long offers = 6,000–10,000 input tokens — truncation strategy required
- **ATS caveat:** keyword alignment improves ATS scoring but does not guarantee it — ATS systems vary widely in their matching algorithms
- **Legal disclaimer:** "This tool suggests vocabulary alignments. The candidate is solely responsible for the accuracy of their CV."

---

## 17. Out of Scope

- Full CV generation from scratch
- Career coaching or professional advice
- Automated application submission
- Application pipeline tracking
- Interview preparation
- Salary benchmarking
- LinkedIn profile optimisation at MVP

---

## 18. Future Evolution

### V1 — Enhanced Analysis
- Domain vocabulary libraries in Airtable (IT/data, consulting, finance, marketing)
- ATS score simulation: estimate keyword match percentage before and after rewrite
- Cover letter generator: same vocabulary alignment applied to the motivation letter
- Multi-language support: English offers + French CVs, or vice versa

### V2 — Application Intelligence
- Application history: track which CV version was used for which offer
- A/B comparison: compare two rewritten versions side by side
- "Weak signal" detector: identify consistently missing skills across 10+ applications (suggests a structural gap to address)

### V3 — Career Navigation
- Role gap map: given current profile and target role, generate a 12-month competency development roadmap
- Market vocabulary tracker: monitor evolving terminology in job offers across domains
- Coach API: career coaches use the tool with multiple clients, see aggregate insights

---

## 19. Learnings

*To be completed after Le Wagon demo day — June 5, 2026.*

- [ ] What unexpected challenges did you face, and how did you adapt?
- [ ] Did you reach your P1 goals? If not, what stopped you?
- [ ] What would you do differently in the next iteration?
- [ ] Which KPI targets were realistic? Which need adjustment?

---

## Author

**Caroline Tith** — Enterprise Architect & AI Product Builder
TOGAF Foundation & Practitioner (2026) | 8 years in data governance & BI transformation
Île-de-France, France

[LinkedIn](https://linkedin.com/in/carolinetith) · [Email](mailto:caroline.tith@hotmail.com)

---

*Built with: n8n · Airtable · Softr · Claude API · Le Wagon AI Product Builder Program*
