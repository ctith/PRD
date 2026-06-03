# ManipDetect 🔍
### AI-powered manipulation detection in text & e-commerce pages

![Status](https://img.shields.io/badge/status-MVP%20in%20progress-orange)
![Stack](https://img.shields.io/badge/stack-n8n%20%7C%20Airtable%20%7C%20Softr%20%7C%20LLM-blue)
![Type](https://img.shields.io/badge/type-Agentic%20AI%20product-purple)
![Author](https://img.shields.io/badge/author-Caroline%20Tith-darkred)

> *Built during Le Wagon, AI Product Builder — June 2026*

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

| Profile | Context | Pain |
|---|---|---|
| Employee in HR conflict | Receives an ambiguous email from management | Cannot name what feels wrong |
| Freelance consultant | Reviewing a client contract before signing | Unsure if pressure is legitimate |
| Online shopper | Browsing an e-commerce site with aggressive pricing | Cannot tell if urgency is real |
| Consumer protection advocate | Auditing landing pages | No scalable tool for systematic analysis |

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
- **No-code automation platforms** (n8n, Softr, Airtable) make it possible to ship an agentic MVP in 2 days without infrastructure overhead
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

| Tool | What it does well | What it misses | Relevant constraint |
|---|---|---|---|
| **Grammarly** | Style, grammar, tone detection | No rhetorical or manipulation analysis | Paid, no API for custom analysis |
| **Crystal Knows** | Personality profiling of the sender | Analyses the person, not the text | GDPR concerns, B2B focus |
| **Generic tone analyzers** | Positive / negative sentiment | Far too superficial, no taxonomy | No actionable output |
| **ChatGPT (free prompt)** | Can analyse if well-prompted | No dedicated interface, no taxonomy, inconsistent output | No structured report, UX friction |
| **No tool identified** | — | **Dedicated manipulation detection with structured taxonomy + agentic multi-source analysis + e-commerce price comparison** | Gap confirmed |

**Conclusion:** the gap is real and unoccupied. The closest competitor is a well-crafted ChatGPT prompt — our differentiation is the taxonomy, the agentic workflow, and the e-commerce price comparison layer.

---

## 6. Solution Concept

### MVP (2 days — Le Wagon)

The user pastes a text OR submits a URL. The agent:
1. Detects input type (text vs URL)
2. Scrapes the page if URL
3. Fetches the manipulation taxonomy from Airtable
4. Calls the LLM with the text and taxonomy
5. Parses the structured JSON output
6. Returns an annotated report: techniques detected, excerpts, explanations, response suggestions

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

The LLM receives the user's text + the 18-technique taxonomy fetched from Airtable, and returns a structured JSON containing:
- Number of distinct techniques detected
- Global intensity score (`low | moderate | high | systemic`)
- Per-technique breakdown: name, exact excerpt, explanation, response suggestion
- 2–3 sentence synthesis of the rhetorical profile

### Where it sits in the golden path

```
User input (text or URL)
        ↓
[n8n] Type detection + optional scraping
        ↓
[Airtable] Taxonomy fetch
        ↓
[LLM] ← CORE AI STEP — semantic analysis + JSON generation
        ↓
[n8n] JSON parsing + HTML report formatting
        ↓
[Softr] Structured report displayed to user
```

### Strategic Role Statement

> Without the AI step, ManipDetect is an empty form. The LLM holds the analytical capability that would take a trained rhetorician 30–45 minutes to produce manually — the product delivers it in under 10 seconds, at zero marginal cost per analysis. At scale, no human review layer could match this throughput. **AI is not a feature — it is the product.**

### User value unlocked

- **Time saved:** 30–45 minutes of manual analysis → under 10 seconds
- **Cognitive relief:** eliminates the need for prior expertise in rhetoric or psychology
- **Actionability:** each technique comes with a concrete response suggestion
- **Confidence:** structured report replaces vague intuition with named, explained patterns

---

## 8. Tech Stack & Architecture

### POC Stack

| Layer | Tool | Why chosen for POC | Known constraints |
|---|---|---|---|
| **Frontend** | Softr | Zero-code UI builder on Airtable, form + results page in hours | Limited custom styling, Airtable dependency |
| **Orchestration** | n8n | Visual workflow builder, native HTTP + JSON + LLM nodes, self-hostable | Rate limits on cloud free tier, 5s timeout on webhooks |
| **AI layer** | Claude 3.5 Sonnet or GPT-4o | Multimodal, strong structured JSON output, reliable instruction following | Cost ~$0.003–0.008 per analysis, token limits on very long texts |
| **Taxonomy DB** | Airtable | No-code, API-ready, editable by non-developers | 5 req/s rate limit on free tier |
| **Scraping** | n8n HTTP node | Built-in, no additional service needed | Blocked by Cloudflare / JS-heavy pages |
| **Hosting** | n8n Cloud / self-hosted | Fast setup for POC | Self-host required for production |

### Architecture Diagram — POC

```
┌─────────────────────────────────────────────────────┐
│                     USER (Softr)                    │
│         Paste text  OR  Submit URL                  │
└──────────────────────┬──────────────────────────────┘
                       │ Webhook POST
                       ▼
┌─────────────────────────────────────────────────────┐
│                    n8n WORKFLOW                      │
│                                                     │
│  [1] Receive webhook                                │
│  [2] IF node: text or URL?                          │
│       └─ URL → HTTP GET → extract body text         │
│       └─ text → pass through                        │
│  [3] HTTP GET → Airtable API → fetch taxonomy       │
│  [4] Format prompt: text + taxonomy                 │
│  [5] POST → LLM API (Claude / GPT-4o)               │
│         Input:  { system_prompt, user_text,         │
│                   taxonomy_json }                   │
│         Output: { score, intensite,                 │
│                   techniques[], synthese }          │
│  [6] Parse JSON output                              │
│  [7] Format HTML report                             │
│  [8] Webhook response → Softr                       │
└──────────────────────┬──────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────┐
│                SOFTR RESULTS PAGE                   │
│  Score | Intensity badge | Synthesis                │
│  Per-technique: excerpt + explanation + suggestion  │
└─────────────────────────────────────────────────────┘
```

### Architecture Diagram — V1 Dropshipping Extension

```
URL (e-commerce)
        │
        ▼
[n8n] Scrape page
        │
   ┌────┴────────────────────────────────────┐
   │                                         │
   ▼                                         ▼
[LLM] Manipulation analysis          [HTTP] AliExpress search
[Airtable] Taxonomy fetch            [WHOIS API] Domain age
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
              Softr report
```

---

## 9. Production-Grade Target

*Not built now — thinking ahead to avoid painful architectural decisions later.*

| Area | POC approach | Production change | Why |
|---|---|---|---|
| **Scale** | Single n8n webhook, synchronous | Queue-based (Bull, Redis) + async processing | Webhook timeouts at >10 concurrent users; queue decouples load |
| **AI layer** | Single LLM call, one model | Fallback chain (primary → secondary model) + response caching for identical inputs | Avoid single point of failure; caching cuts cost by ~60% on repeated URLs |
| **Cost control** | No limits | Rate limiting per user/IP, input truncation at 4,000 tokens, request batching | Unbounded LLM calls become expensive at scale; one long document can cost $0.10+ |
| **Privacy & compliance** | Session-only, no storage | Encrypted audit log (no PII), explicit RGPD consent flow, DPA with LLM provider | Any storage of personal communication content triggers GDPR obligations |
| **Security** | Open webhook | Auth token on webhook, input sanitisation (XSS, injection), secrets in env vars | Open webhooks are trivially abused; injected prompts in user input are a real attack vector |

---

## 10. Accessibility

ManipDetect is designed to be usable by people with visual, motor, and cognitive disabilities, in alignment with WCAG 2.1 AA and France's RGAA standards.

### Applied practices

| WCAG Principle | Implementation in ManipDetect |
|---|---|
| **Perceivable** | Intensity verdicts use colour + icon + text label (never colour alone — critical for colourblind users) |
| **Perceivable** | All technique excerpts are displayed as readable text, not images |
| **Operable** | Form submission and results navigation fully keyboard-accessible (Tab, Enter, Escape) |
| **Operable** | Touch targets (Submit button, technique cards) minimum 44×44px |
| **Understandable** | Error messages describe the problem and the fix ("Please paste text or a valid URL") |
| **Understandable** | LLM output is prompted to use plain, clear language — benefits all users, especially those with cognitive disabilities |
| **Robust** | Semantic HTML for all Softr components; ARIA labels on interactive elements |

### Colour contrast
- Intensity badges: minimum 4.5:1 contrast ratio against background (WCAG AA)
- Body text: 16px minimum, no light font weights
- Never use red/green without a complementary icon or label

### Audit plan
Run Lighthouse accessibility audit (Chrome DevTools) on the Softr output before demo day. Target score: 90+.

---

## 11. KPIs & Measurement

### Product KPIs

| KPI | What it measures | Target (demo day) | How to capture |
|---|---|---|---|
| **End-to-end completion rate** | % of users who submit text/URL AND view the full report | > 80% | n8n log: webhook received vs report rendered |
| **Analysis accuracy (spot-check)** | % of detected techniques judged correct by a human reviewer on a sample of 10 analyses | > 75% | Manual review rubric on a test set of 10 diverse inputs |
| **Time to insight** | Time from form submission to report display | < 12 seconds p50 | n8n execution log timestamps |

### AI-Specific KPIs

| KPI | Definition in this context | Target | Data source |
|---|---|---|---|
| **AI task success rate** | % of LLM calls that return valid, parseable JSON with at least one technique or a clear "no technique detected" | > 90% | n8n error log + JSON parse step |
| **Latency p50 / p95** | Median and worst-case LLM response time | p50 < 8s / p95 < 20s | n8n execution timestamps |
| **Cost per analysis** | Estimated USD cost per LLM call (input + output tokens) | < $0.01 per analysis | LLM provider dashboard (Anthropic / OpenAI usage) |

---

## 12. Guardrails & Failsafes

### Guardrail 1 — Input validation
Before hitting the LLM, n8n validates:
- Input is not empty
- Text input is between 10 and 4,000 characters (truncation above limit with user warning)
- URL input matches a basic URL pattern

*Why:* empty or malformed inputs generate expensive, useless LLM calls and confusing error states.

### Guardrail 2 — Output format check
After the LLM responds, n8n checks that the output is valid JSON and contains the required keys (`score_global`, `intensite`, `techniques`, `synthese`). If not, it retries once before returning a fallback message.

*Why:* LLMs occasionally deviate from the expected schema; a single retry resolves ~80% of format failures.

### Failsafe — Graceful degradation
If the LLM call fails after one retry (timeout, API error, invalid JSON on second attempt), the user sees:
> *"Analysis temporarily unavailable. Your text was not stored. Please try again in a moment."*

No partial or broken report is ever shown.

### Editorial guardrail — Prompt constraint
The system prompt explicitly instructs the LLM to:
- Analyse the **text**, not the **person** who wrote it
- Never use diagnostic language ("this person is manipulative")
- Be pedagogical, not alarmist

---

## 13. Backlog & Golden Path

### Golden Path

> A user pastes an email they received from their manager. In under 10 seconds, they see a report identifying 2 techniques (false urgency + culpabilisation), the exact excerpts highlighted, an explanation of each mechanism, and a suggested response strategy.

### P1 — Must-have for demo day

| # | Task | Done? |
|---|---|---|
| P1-1 | Softr form: text input + URL input + submit button + RGPD mention | ☐ |
| P1-2 | n8n webhook receives input, detects type (text vs URL) | ☐ |
| P1-3 | n8n fetches taxonomy from Airtable (18 techniques) | ☐ |
| P1-4 | n8n calls LLM with system prompt + text + taxonomy, receives JSON | ☐ |
| P1-5 | Softr results page: score, intensity badge, synthesis, per-technique cards | ☐ |

### P2 — Stretch goals

| # | Task |
|---|---|
| P2-1 | URL scraping via n8n HTTP node (JS-light pages only) |
| P2-2 | "Copy report" button on results page |
| P2-3 | Retry logic on LLM call failure |
| P2-4 | Lighthouse accessibility audit pass (score 90+) |
| P2-5 | Dropshipping score (AliExpress price comparison) |

---

## 14. Manipulation Taxonomy

*Stored in Airtable — table: `Manipulation_Taxonomy` — 6 columns: Nom / Définition / Mécanisme / Signaux_textuels / Exemple / Suggestion_réponse*

### Interpersonal techniques (12)

| # | Technique | Core signal |
|---|---|---|
| 01 | **Fausse urgence** | Artificial time pressure to prevent reflection |
| 02 | **Culpabilisation** | Attributing responsibility for a negative situation to create moral debt |
| 03 | **DARVO** | Deny, Attack, Reverse Victim and Offender |
| 04 | **Double bind** | Two options, both unfavourable — the illusion of choice |
| 05 | **Inversion de responsabilité** | Returning the burden of proof to the person who was harmed |
| 06 | **Ambiguïté délibérée** | Deliberately vague formulation to preserve an exit |
| 07 | **Appel à l'autorité implicite** | Unnamed authority invoked to inhibit contestation |
| 08 | **Minimisation** | Reducing the legitimacy of a feeling to avoid addressing it |
| 09 | **Gaslighting textuel** | Denying or rewriting past facts to create self-doubt |
| 10 | **Flatterie instrumentale** | Targeted compliment before a request to create reciprocity obligation |
| 11 | **Projection** | Attributing one's own behaviour or intentions to the other |
| 12 | **Faux consensus** | Presenting a position as shared by a silent majority to isolate |

### Marketing / copywriting techniques (3)

| # | Technique | Core signal |
|---|---|---|
| 13 | **Preuve sociale artificielle** | Unverifiable figures or testimonials to simulate crowd validation |
| 14 | **Rareté artificielle** | False scarcity (stock, time) to trigger urgency |
| 15 | **Ancrage de prix** | Crossed-out reference price to make the real price seem like a bargain |

### Dropshipping-specific techniques (3)

| # | Technique | Core signal |
|---|---|---|
| 16 | **Faux témoignages** | Generic profile photos, grouped dates, uniformly perfect reviews |
| 17 | **Badges de confiance factices** | Non-functional security logos and unverifiable certification labels |
| 18 | **Présentation produit générique** | Catalogue photos identical to AliExpress, auto-translated descriptions |

---

## 15. System Prompt

### N8N prompt
```
Tu es un analyste expert en rhétorique et psychologie de la persuasion.

Ton rôle est d'analyser un texte ou le contenu d'une page web
et d'identifier les techniques de manipulation présentes.

Voici la taxonomie de référence :
{{taxonomie_airtable}}

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

### Lovable prompt
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

- **Privacy:** no text is stored server-side — session only. Explicit RGPD mention in UI.
- **Scope of analysis:** the tool analyses text, never diagnoses the person who wrote it. No psychiatric or legal claims.
- **No paid APIs at MVP:** AliExpress and WHOIS integrations deferred to V1
- **Scraping limitations:** JS-heavy pages (Shopify, React SPAs) may not scrape cleanly via HTTP node — fallback to copy-paste instruction
- **Token budget:** inputs truncated at 4,000 characters to control LLM cost and latency
- **Language:** French-first for taxonomy and UI — English-language texts analysed but explanations returned in French at MVP

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
> - Partnership with mental health professionals and associations (3919, France Victimes)
> - GDPR Article 9 compliance (sensitive health data)
> - Systematic orientation protocol toward real support resources
> - User testing with affected populations
> - Clinical validation of the pattern detection model

*This module will not be built without these conditions. Rigour is the condition of its value.*

---

## 20. Learnings

*To be completed after Le Wagon demo day — June 5, 2026.*

- [ ] What unexpected challenges did you face, and how did you adapt?
- [ ] Did you reach your P1 goals? If not, what stopped you?
- [ ] What would you do differently in the next iteration?
- [ ] Which KPI targets were realistic? Which need adjustment?

---

## Author

**Caroline Tith** — Enterprise Architect, Data & AI Specialist

[LinkedIn](https://linkedin.com/in/caroline-tith) · [Email](mailto:caroline.tith@hotmail.com)

*ManipDetect — Le Wagon, AI Product Builder batch#1 — June 2026*

---

