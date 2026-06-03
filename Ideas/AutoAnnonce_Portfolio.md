# AutoAnnonce 📸
### From photo to published listing in 30 seconds

![Status](https://img.shields.io/badge/status-MVP%20in%20progress-orange)
![Stack](https://img.shields.io/badge/stack-n8n%20%7C%20Airtable%20%7C%20Softr%20%7C%20GPT--4o-blue)
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
14. [Platform Style Guides](#14-platform-style-guides)
15. [System Prompt](#15-system-prompt)
16. [Constraints](#16-constraints)
17. [Out of Scope](#17-out-of-scope)
18. [Future Evolution](#18-future-evolution)
19. [Learnings](#19-learnings)

---

## 1. Problem Statement

Creating a second-hand listing on Vinted, Leboncoin, or Back Market takes 10–20 minutes per item — description, categorisation, condition rating, price estimation, then repeating for each platform. This friction is high enough that most people either never list their items or list them poorly, leaving money on the table and clutter in their homes.

**The cognitive effort of describing what you can already see should not exist.** AutoAnnonce eliminates it entirely.

---

## 2. Target User & Context

### Primary users

| Profile | Context | Pain |
|---|---|---|
| Parent sorting children's clothes | Saturday afternoon, 20 items to list on Vinted | Each listing takes 15 minutes — the task is abandoned after 3 items |
| Young adult moving flats | 2 weeks to declutter before moving, selling on Leboncoin | No time to write 30 descriptions; items end up in a skip |
| Occasional reseller | Buys bundles at vide-greniers, resells online for margin | Listing time eats into profit margin; limits scalable resale |
| Eco-conscious consumer | Wants to give items a second life rather than throw away | Good intention blocked by effort barrier |

### Secondary users
- Charity shops and social enterprises digitising donated inventory
- Estate clearance professionals managing large-volume listings
- Small e-commerce sellers managing multi-platform presence

---

## 3. Why Now

- **Vinted hit 75M+ users in Europe (2024)** — the second-hand market is mainstream, not niche. The listing friction is the primary barrier to further growth.
- **Multimodal LLMs (GPT-4o, Claude 3.5)** can now reliably identify objects, brands, conditions, and generate structured text from a photo — this capability is mature and affordable in 2026
- **EU Circular Economy Action Plan** and growing anti-fast-fashion sentiment are culturally accelerating second-hand adoption — the user base is expanding and motivated
- **No-code mobile-first tools** make a photo upload → AI generation flow achievable without native app development
- **AliExpress and Temu transparency scandals** have increased consumer awareness of dropshipping — AutoAnnonce's honest, photo-first approach aligns with the trust narrative

---

## 4. Value Hypothesis

Reducing listing creation time from 15 minutes to under 2 minutes per item removes the primary psychological barrier to selling second-hand goods, multiplying the number of items successfully listed and sold.

**Current workarounds:**
- Manual listing on each platform: dominant behaviour, 10–20 min/item, error-prone
- Vinted's own photo recognition: exists but minimal — suggests a category, generates no description
- Voice memo to self, write description later: friction just deferred, often forgotten
- Asking a friend to help: rare, non-scalable

**Impact of the problem:**
- Estimated 30–40% of items bought to resell are never listed (informal user surveys on r/Vinted)
- Average seller lists 3–5 items per session max due to time cost
- Poor descriptions reduce visibility in platform search — impacting sale price and time-to-sale

---

## 5. Market Scan

| Tool | What it does well | What it misses | Relevant constraint |
|---|---|---|---|
| **Vinted native** | Photo upload, basic category suggestion | No description generation, no price suggestion, one platform only | Closed ecosystem |
| **Leboncoin native** | Simple listing flow | Zero AI assistance, no multi-platform support | No API for posting |
| **SnapSell (US)** | Photo → basic listing on eBay | US only, not available in France, single platform | No EU presence |
| **ListingAI** | AI-assisted listing descriptions | Real estate focus, not general consumer goods | Niche tool |
| **Auchan / Fnac reprise** | Trade-in valuations | Not resale, not user-to-user, heavily discounted offers | Different model |

**Gap confirmed:** no French-language tool combines multimodal photo recognition + multi-platform description generation + price estimation for Vinted and Leboncoin.

---

## 6. Solution Concept

### MVP (2 days — Le Wagon)

1. User uploads 2–3 photos of an item on the Softr interface
2. n8n converts photos to base64 and sends to multimodal LLM
3. LLM identifies: object type, brand (if visible), condition (based on visual cues), key characteristics
4. LLM generates two listings from the Airtable platform style guide:
   - **Vinted version:** casual, French, fashion/lifestyle keywords, condition label (Neuf / Très bon état / Bon état)
   - **Leboncoin version:** factual, French, technical specs prioritised, category suggestion, price range
5. User sees both listings side by side, copies the text directly into the platform

### V1 — Multi-platform + Price Intelligence
- Add Back Market (electronics), La Redoute (furniture/decor) formats
- Price suggestion based on brand, condition, and category benchmarks
- Batch mode: upload 10 items, receive 10 listings

---

## 7. AI Use-Case & Strategic Role

### What the AI step does

The multimodal LLM receives 2–3 base64-encoded photos and the platform style guides, then:
- Identifies the object (category, type, brand, model if legible)
- Assesses visible condition (new in box / very good / good / acceptable / worn)
- Extracts relevant characteristics (colour, material, size if visible)
- Generates platform-appropriate title + description + suggested category + estimated price range

### Where it sits in the golden path

```
User uploads 2-3 photos (Softr)
          ↓
[n8n] Convert images to base64
          ↓
[Airtable] Fetch platform style guides (Vinted + Leboncoin)
          ↓
[LLM — GPT-4o or Claude] ← CORE AI STEP
  Input:  { images_base64[], platform_style_guides }
  Output: {
    object_identified: "...",
    brand: "...",
    condition: "...",
    vinted: { title, description, category, condition_label },
    leboncoin: { title, description, category, price_range }
  }
          ↓
[n8n] Parse JSON + format display
          ↓
[Softr] Side-by-side listing cards with copy buttons
```

### Strategic Role Statement

> Without the AI vision step, AutoAnnonce is a photo uploader that does nothing. The LLM is the only component capable of reading a photo and producing a structured, platform-appropriate listing in seconds. **This is a zero-human-effort product — the AI replaces the entire cognitive and editorial work of the user. Remove the AI and the product concept ceases to exist.**

### User value unlocked

- **Time saved:** 15 minutes per item → under 2 minutes. 10 items = 20 minutes vs 2.5 hours.
- **Quality improvement:** AI-generated descriptions include relevant keywords the user would have forgotten, improving platform search visibility
- **Reduced abandonment:** psychological barrier to starting the listing process eliminated
- **Multi-platform without duplication effort:** two platform formats generated in a single step

---

## 8. Tech Stack & Architecture

### POC Stack

| Layer | Tool | Why chosen for POC | Known constraints |
|---|---|---|---|
| **Frontend** | Softr | File upload widget + dual card display, zero-code | Max file size limits on free tier; image upload UX less fluid than native app |
| **Orchestration** | n8n | Image → base64 → LLM call, native HTTP node | Image processing can be slow; large images may timeout |
| **AI layer** | GPT-4o (preferred) or Claude 3.5 Sonnet | GPT-4o has strongest vision-to-text performance for product identification | ~$0.01–0.03 per session (3 images + generation) |
| **Style guide DB** | Airtable | Vinted and Leboncoin format rules, tone guides, category lists | Manual maintenance required as platform conventions evolve |
| **Image handling** | n8n base64 encode node | No additional service needed for POC | Base64 encoding of 3 images can add 1–2s; file size capped at ~4MB |

### Architecture Diagram — POC

```
┌──────────────────────────────────────────────────────┐
│                  USER (Softr)                        │
│   Upload 2-3 photos of the item to sell              │
└──────────────────────┬───────────────────────────────┘
                       │ Webhook POST (images)
                       ▼
┌──────────────────────────────────────────────────────┐
│                  n8n WORKFLOW                        │
│                                                      │
│  [1] Receive webhook with image files                │
│  [2] Convert each image to base64                    │
│  [3] Fetch from Airtable:                            │
│       → Vinted style guide (tone, keywords, format) │
│       → Leboncoin style guide (tone, format, cats)  │
│  [4] Build multimodal LLM prompt:                    │
│       { images_base64[], vinted_guide,               │
│         leboncoin_guide }                            │
│  [5] POST → GPT-4o vision API                        │
│       Input:  photos + style guides                  │
│       Output: { identified object, brand,            │
│                 condition, vinted{}, leboncoin{} }   │
│  [6] Parse JSON                                      │
│  [7] Format dual listing cards                       │
│  [8] Webhook response → Softr                        │
└──────────────────────┬───────────────────────────────┘
                       │
                       ▼
┌──────────────────────────────────────────────────────┐
│              SOFTR RESULTS PAGE                      │
│  Object identified | Brand | Condition               │
│  ┌──────────────┐  ┌──────────────────┐              │
│  │   VINTED     │  │   LEBONCOIN      │              │
│  │ Title        │  │ Title            │              │
│  │ Description  │  │ Description      │              │
│  │ Category     │  │ Category         │              │
│  │ Condition    │  │ Price range      │              │
│  │ [Copy]       │  │ [Copy]           │              │
│  └──────────────┘  └──────────────────┘              │
└──────────────────────────────────────────────────────┘
```

---

## 9. Production-Grade Target

| Area | POC | Production change | Why |
|---|---|---|---|
| **Scale** | Synchronous per upload | Async queue + progress indicator | Image processing + LLM call = 8–15s; users need feedback, not blank waiting screen |
| **AI layer** | GPT-4o single call | Fallback to Claude vision if GPT-4o fails; retry on timeout | Single provider dependency is a reliability risk for a user-facing product |
| **Cost control** | No limits | Image compression before LLM (resize to max 1024px); cap at 3 images; cache identical images via hash | Raw smartphone photos (4–8MB) are expensive to process; compression reduces cost 60–80% |
| **Privacy & compliance** | Images processed in transit | Explicit consent for image processing; no image storage after session; GDPR-compliant data processing agreement with LLM provider | Product photos may contain personal information (home interiors, faces in mirrors) |
| **Security** | Open file upload | File type validation (JPEG/PNG only), max file size enforcement, malware scan on upload | Open file upload is a standard attack vector; SSRF possible via malicious image files |

---

## 10. Accessibility

| WCAG Principle | Implementation |
|---|---|
| **Perceivable** | Upload interface includes clear text instructions, not icon-only affordances |
| **Perceivable** | Generated listings are plain text — fully readable by screen readers |
| **Perceivable** | Condition labels use text + icon, never colour alone |
| **Operable** | File upload via keyboard-accessible button (not drag-and-drop only) |
| **Operable** | "Copy to clipboard" buttons keyboard-navigable and screen-reader labelled |
| **Understandable** | Clear error messages: "File type not supported — please upload a JPEG or PNG" |
| **Understandable** | Loading state with progress text: "Analysing your photos…" not silent spinner |
| **Robust** | Semantic HTML for upload widget; copy button announces "Copied!" to screen readers via ARIA live region |

---

## 11. KPIs & Measurement

### Product KPIs

| KPI | What it measures | Target (demo day) | How to capture |
|---|---|---|---|
| **Listing generation completion rate** | % of photo uploads that result in a successfully displayed listing | > 85% | n8n log: upload received vs listing returned |
| **User-judged listing quality** | User rates "This listing is good enough to publish as-is" (yes/no) | > 70% yes | One-click rating on results page |
| **End-to-end time** | Time from photo upload click to listing displayed | < 15 seconds p50 | n8n timestamps |

### AI-Specific KPIs

| KPI | Definition in this context | Target | Data source |
|---|---|---|---|
| **Object identification rate** | % of LLM calls that correctly identify the item category (verified by spot-check on 10 test items) | > 80% | Manual review of 10 diverse items (clothing, electronics, furniture, books) |
| **Latency p50 / p95** | Median and worst-case time for vision + generation call | p50 < 10s / p95 < 25s | n8n execution log |
| **Cost per listing set** | LLM cost for one 3-image session generating 2 platform listings | < $0.03 | OpenAI usage dashboard |

---

## 12. Guardrails & Failsafes

### Guardrail 1 — File validation
Before hitting the LLM, n8n validates:
- At least 1 image submitted, max 3
- File type: JPEG or PNG only
- File size: max 5MB per image

*Why:* unvalidated uploads cause silent LLM failures or expensive processing of non-image files.

### Guardrail 2 — Identification confidence check
If the LLM cannot identify the object with reasonable confidence, it returns a `confidence: "low"` flag. The UI displays: *"We couldn't clearly identify this item — try a photo with better lighting or a clearer angle."*

*Why:* a listing for an unidentified item ("Object: unknown, brown, possibly clothing") is worse than no listing.

### Guardrail 3 — Price range disclaimer
Price estimates are always displayed as a range with the note: *"Suggested range based on item characteristics — verify against similar active listings on the platform."*

*Why:* AutoAnnonce has no access to real-time market data at MVP — a false price confidence could harm users.

### Failsafe — Graceful degradation
If the LLM call fails after one retry, display: *"Listing generation temporarily unavailable. Your photos were not stored. Please try again."*

---

## 13. Backlog & Golden Path

### Golden Path

> A parent uploads 3 photos of a children's winter coat. In under 15 seconds, they see two ready-to-publish listings: a Vinted listing in casual French with the correct condition label and size, and a Leboncoin listing with dimensions and a price range. They copy the Vinted text with one click and paste it directly into the Vinted app.

### P1 — Must-have for demo day

| # | Task | Done? |
|---|---|---|
| P1-1 | Softr: photo upload form (1–3 images) + submit button | ☐ |
| P1-2 | n8n: receive images, convert to base64 | ☐ |
| P1-3 | Airtable: Vinted style guide + Leboncoin style guide | ☐ |
| P1-4 | n8n: fetch style guides, build multimodal prompt, call GPT-4o | ☐ |
| P1-5 | n8n: parse JSON output | ☐ |
| P1-6 | Softr: dual listing display with copy-to-clipboard buttons | ☐ |

### P2 — Stretch goals

| # | Task |
|---|---|
| P2-1 | Confidence flag display when object identification is uncertain |
| P2-2 | Price range disclaimer label |
| P2-3 | Retry logic on LLM failure |
| P2-4 | User quality rating widget (yes/no on listing quality) |
| P2-5 | Back Market platform format added |

---

## 14. Platform Style Guides

*Stored in Airtable — table: `Platform_Style_Guides`*

### Vinted

| Field | Guidelines |
|---|---|
| **Tone** | Casual, friendly, first-person optional. "Super état !", "Vendu rapidement !" |
| **Title format** | `[Brand] [Item type] [Key characteristic] — [Size/Model]` max 60 chars |
| **Description** | 3–5 sentences. Start with condition, then features, then measurement if relevant. End with "N'hésitez pas à faire une offre !" |
| **Condition labels** | Neuf avec étiquette / Neuf sans étiquette / Très bon état / Bon état / Satisfaisant |
| **Keywords** | Fashion, season, colour, brand, occasion (casual, soirée, sport) |
| **Price psychology** | Round numbers ending in 9 (29€, 49€). Include "Prix ferme" or "Négociable" |

### Leboncoin

| Field | Guidelines |
|---|---|
| **Tone** | Factual, neutral, descriptive. Buyer wants specs and honesty. |
| **Title format** | `[Item type] [Brand] [Model/Reference] [Condition]` max 70 chars |
| **Description** | 4–6 sentences. Lead with specs (dimensions, capacity, year if relevant), then condition details (honest about flaws), then context (reason for selling). |
| **Condition labels** | Neuf / Très bon état / Bon état / État correct / Pour pièces |
| **Keywords** | Technical specs, brand, model number, year, compatible uses |
| **Price psychology** | Round numbers. Note if price is firm or negotiable. Include "Remise en main propre possible" if relevant. |

---

## 15. System Prompt

```
You are an expert at identifying second-hand items from photos
and writing compelling, platform-appropriate marketplace listings
in French.

The user has uploaded 2-3 photos of an item they want to sell.

Platform style guides:
VINTED: {{vinted_style_guide}}
LEBONCOIN: {{leboncoin_style_guide}}

INSTRUCTIONS:
1. Analyse all provided photos carefully.
2. Identify: item type, brand (if visible), model (if visible),
   colour, material, approximate size/dimensions (if estimable),
   condition based on visible wear.
3. Generate ONE listing for Vinted and ONE for Leboncoin,
   following each platform's style guide precisely.
4. If you cannot identify the item with confidence,
   set confidence to "low" and explain what you could and
   could not identify.
5. Price range: estimate based on brand tier, item type,
   and condition. Always return as a range, never a single price.
6. Never invent details not visible in the photos.
   If unsure, say so in the description.

RETURN ONLY valid JSON in this exact structure:
{
  "confidence": "<high|medium|low>",
  "object_identified": "<item type>",
  "brand": "<brand name or 'Non visible'>",
  "condition_assessment": "<what you observed about condition>",
  "vinted": {
    "title": "<max 60 chars>",
    "description": "<3-5 sentences, casual French>",
    "category": "<Vinted category suggestion>",
    "condition_label": "<Vinted condition label>",
    "suggested_price": "<range e.g. 15€ - 25€>"
  },
  "leboncoin": {
    "title": "<max 70 chars>",
    "description": "<4-6 sentences, factual French>",
    "category": "<Leboncoin category suggestion>",
    "condition_label": "<Leboncoin condition label>",
    "suggested_price": "<range e.g. 15€ - 25€>"
  }
}

DO NOT add text before or after the JSON.
Return raw JSON only.
```

---

## 16. Constraints

- **Platform APIs:** Vinted and Leboncoin APIs are closed — no direct publishing at MVP. Copy-paste is the intended flow.
- **Vision accuracy:** brand identification requires visible labels; model identification requires legible text in photos. Accuracy degrades with low-quality photos.
- **Price estimation:** no real-time market data access — estimates are based on LLM training data. Always presented as indicative ranges.
- **Language:** French-language listings only at MVP
- **Image quality dependency:** dark, blurry, or cluttered background photos will produce lower-confidence results — user education required
- **File size:** smartphone photos compressed to max 1024px before LLM processing

---

## 17. Out of Scope

- Direct API publishing to Vinted, Leboncoin, or any platform
- Real-time price intelligence from live platform listings
- Batch upload mode (>3 items simultaneously) at MVP
- Video listing support
- Shipping cost estimation
- Negotiation assistance or buyer communication

---

## 18. Future Evolution

### V1 — Multi-platform + Price Intelligence
- Back Market format (electronics refurbished)
- La Redoute / Vide Dressing formats (furniture, luxury)
- Price comparison: scrape similar active listings for reference
- Batch mode: upload 10 items, generate 10 listing sets

### V2 — Mobile-First
- Progressive Web App or React Native for direct camera access
- One-tap flow: camera → listing → clipboard
- Platform-specific deep link: tap "Publish on Vinted" opens Vinted with text pre-filled (clipboard API)

### V3 — Business Tools
- API for charity shops and estate clearance professionals
- Inventory management layer: track what's listed, what's sold
- Multi-language listing generation (English for eBay, Spanish for Wallapop)

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

*Built with: n8n · Airtable · Softr · GPT-4o Vision API · Le Wagon AI Product Builder Program*
