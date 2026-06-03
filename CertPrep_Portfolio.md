# CertPrep Adaptatif 🎓
### AI-powered certification prep adapted to your cognitive profile

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
14. [Cognitive Profile Framework](#14-cognitive-profile-framework)
15. [System Prompt](#15-system-prompt)
16. [Constraints](#16-constraints)
17. [Out of Scope](#17-out-of-scope)
18. [Future Evolution](#18-future-evolution)
19. [Learnings](#19-learnings)

---

## 1. Problem Statement

All IT certification prep platforms (TOGAF, DAMA CDMP, ITIL, PMP, BABOK, COBIT, Scrum) deliver the same linear, one-size-fits-all sequence — regardless of how different learners actually process information. A systemic thinker who needs the global architecture before the details is forced into the same step-by-step path as a sequential learner, generating confusion, drop-off, and wasted study time.

**The same content, delivered in the wrong order, feels incomprehensible. Delivered in the right order, it clicks in hours instead of weeks.**

---

## 2. Target User & Context

### Primary users

| Profile | Context | Pain |
|---|---|---|
| IT professional preparing TOGAF | Self-studying 3-5h/week alongside a full-time job | Linear ADM sequence loses meaning without architectural context first |
| Data analyst preparing DAMA CDMP | First certification attempt, no study group | DMBOK domains feel disconnected without a governance systems view |
| Project manager preparing PMP | Re-sitting after a first failure | Memorised content without understanding the underlying logic |
| Enterprise architecture student | Formal programme but non-linear thinker | Course structure conflicts with cognitive style, causes anxiety |

### Secondary users
- Training managers building internal certification paths
- Coaches supporting career transitioners into IT governance roles
- HR teams measuring learning efficiency across certification tracks

---

## 3. Why Now

- **TOGAF 10 (2022) and DAMA DMBOK v2** are now standard requirements for Enterprise Architect and Data Governance roles — demand for prep tools is at an all-time high
- **AI-generated personalisation** is now technically feasible at the question level with modern LLMs — this was not possible at quality and cost before 2024
- **Remote learning fatigue** has increased drop-off rates on traditional e-learning platforms — personalisation is the single most cited lever for re-engagement in post-pandemic L&D research
- **No-code stack** (n8n, Airtable, Softr) makes a functional adaptive learning MVP achievable in 2 days — the technical barrier has collapsed
- **Felder-Silverman learning styles** research has been validated in professional adult learning contexts — applying it to certification prep is an obvious and under-exploited extension

---

## 4. Value Hypothesis

Aligning the order and format of certification prep content to the learner's cognitive profile reduces study time by 30–40% and increases first-attempt pass rates by reducing the gap between memorisation and genuine understanding.

**Current workarounds:**
- Whizlabs / ExamTopics: QCM banks, linear, no adaptation — learners use them but report rote memorisation without real understanding
- YouTube crash courses: useful for global thinkers but non-interactive and non-adaptive
- Study groups: effective but expensive in time, dependent on finding people at the same level
- Generic ChatGPT prompts: no structure, no profile, inconsistent quality

**Impact of the problem:**
- Failed first attempts cost €500–1,500 in re-examination fees
- Study time wasted on content in the wrong order: estimated 30–50% inefficiency for non-sequential learners
- Increased anxiety and abandonment for learners whose cognitive style is incompatible with the standard format

---

## 5. Market Scan

| Tool | What it does well | What it misses | Relevant constraint |
|---|---|---|---|
| **Whizlabs** | Large question banks, timed practice exams | Zero personalisation, pure rote, all sequential | Paid subscription, no API |
| **MeasureUp** | Official partner content, high quality questions | Linear progression, no adaptation | Expensive, B2B focus |
| **Udemy / Coursera** | Video courses with good instructors | Passive consumption, no interaction, no adaptation | No adaptive path, no Q&A generation |
| **ExamTopics** | Free, community-sourced questions | No explanation quality, no learning path | Content accuracy varies |
| **ChatGPT** | Can generate questions if well-prompted | No profile, no persistence, no structured path | Requires prompt expertise from user |

**Gap confirmed:** no platform combines cognitive profile assessment + personalised question ordering + adaptive explanation style for professional IT certifications.

---

## 6. Solution Concept

### MVP (2 days — Le Wagon)

1. User completes a 12-question cognitive profile form (Felder-Silverman + VARK + Kolb combined)
2. Profile is classified into one of 4 dominant types: **Systemic-Theoretical**, **Sequential-Practical**, **Visual-Experiential**, **Analytical-Reflective**
3. User selects target certification (MVP: TOGAF ADM only)
4. LLM generates a personalised set of 20 practice questions:
   - Ordered according to the profile (global overview first for systemic learners; step-by-step for sequential)
   - Formatted in the profile's preferred style (analogy/metaphor for intuitive; concrete example for sensory)
   - With explanations adapted to the dominant learning style

---

## 7. AI Use-Case & Strategic Role

### What the AI step does

The LLM receives the user's cognitive profile classification + the certification domain (TOGAF ADM at MVP) and:
- Selects the appropriate concept sequencing from the Airtable question bank
- Generates explanations in the learner's style (systemic: "here's how ADM fits the whole architecture," sequential: "step 1 of ADM is...")
- Adapts the question format (schema + annotation for visual learners; definition-first for verbal learners)
- Identifies weak spots after answers and suggests reinforcement

### Where it sits in the golden path

```
Cognitive profile form (Softr)
          ↓
[n8n] Receive form data
          ↓
[n8n] Classify cognitive profile (IF nodes)
          ↓
[Airtable] Fetch TOGAF concept bank + profile style guide
          ↓
[LLM] ← CORE AI STEP
  Input:  { profile_type, certification, domains_requested }
  Output: { ordered_questions[], explanations[], style_notes }
          ↓
[n8n] Format question sequence
          ↓
[Softr] Display personalised learning session
```

### Strategic Role Statement

> Without the AI step, CertPrep Adaptatif is a static form with a fixed question list — identical to every existing platform. The LLM is the only component capable of dynamically reordering concepts, reformulating explanations, and adapting format to a cognitive profile in real time. **AI is the personalisation engine. Remove it and the product's entire differentiation disappears.**

### User value unlocked

- **Time saved:** estimated 30–40% reduction in study time by eliminating content consumed in the wrong order
- **Comprehension vs memorisation:** explanations adapted to cognitive style produce genuine understanding, not surface recall
- **Reduced anxiety:** learners whose cognitive style is incompatible with standard formats finally have a path that works for them
- **Exam ROI:** higher first-attempt pass rates reduce re-examination costs (€500–1,500 per attempt)

---

## 8. Tech Stack & Architecture

### POC Stack

| Layer | Tool | Why chosen for POC | Known constraints |
|---|---|---|---|
| **Frontend** | Softr | Form + multi-step display, Airtable-native, zero-code | Limited conditional logic in forms |
| **Orchestration** | n8n | IF nodes for profile classification, Airtable fetch, LLM call in one workflow | 5s webhook timeout — may need async for long LLM calls |
| **AI layer** | Claude 3.5 Sonnet | Strong instruction following, good at structured JSON + pedagogical text | ~$0.005–0.012 per session at 20 questions |
| **Content DB** | Airtable | TOGAF concept bank + 4 profile style guides, editable without code | 5 req/s rate limit; question bank curation is the main time cost |
| **Profile logic** | n8n IF nodes | Simple scoring rules (dominant axis per model) | Complex edge cases need a proper scoring algorithm in V1 |

### Architecture Diagram — POC

```
┌────────────────────────────────────────────────────────┐
│                  USER (Softr)                          │
│   Step 1: 12-question cognitive profile form           │
│   Step 2: Select certification (TOGAF at MVP)          │
└──────────────────────┬─────────────────────────────────┘
                       │ Webhook POST (profile answers)
                       ▼
┌────────────────────────────────────────────────────────┐
│                  n8n WORKFLOW                          │
│                                                        │
│  [1] Receive profile answers                           │
│  [2] Score Felder axes (sequential↔global,             │
│      sensing↔intuitive)                                │
│  [3] Score VARK (visual/auditory/reading/kinesthetic)  │
│  [4] Score Kolb (assimilator/convergent/etc.)          │
│  [5] IF node → classify dominant profile type          │
│  [6] Fetch from Airtable:                              │
│       → TOGAF concept bank (ordered by domain)        │
│       → Profile style guide for dominant type         │
│  [7] Build LLM prompt with profile + concepts          │
│  [8] POST → Claude API                                 │
│       Input:  profile_type + TOGAF concepts + style   │
│       Output: 20 ordered questions with explanations  │
│  [9] Parse JSON                                        │
│  [10] Webhook response → Softr                         │
└──────────────────────┬─────────────────────────────────┘
                       │
                       ▼
┌────────────────────────────────────────────────────────┐
│               SOFTR RESULTS PAGE                       │
│  Profile summary | Learning style displayed            │
│  Question 1 → Answer → Adaptive explanation            │
│  Progress indicator                                    │
└────────────────────────────────────────────────────────┘
```

### Architecture Diagram — V1 (Adaptive Loop)

```
User profile
     │
     ▼
Initial question set (20 Qs)
     │
     ▼
User answers each question
     │
     ▼
[LLM] Analyse answers
  → Identify weak concepts
  → Adjust next questions
  → Generate targeted explanation
     │
     ▼
Reinforcement set (5-10 Qs on weak spots)
     │
     ▼
Session summary + recommended next domain
```

---

## 9. Production-Grade Target

| Area | POC | Production change | Why |
|---|---|---|---|
| **Scale** | Synchronous webhook per user | Async queue (Bull + Redis) | >20 concurrent users will hit n8n timeout; question generation takes 8–15s |
| **AI layer** | Single LLM call per session | Streaming responses + session caching per profile type | Streaming removes perceived latency; caching avoids regenerating identical profile+domain combos |
| **Cost control** | No limits | Rate limit: 10 sessions/user/day; cache common profile×domain combos | Popular combos (Systemic + TOGAF ADM) can be pre-generated and cached at near-zero marginal cost |
| **Privacy & compliance** | No storage | Encrypted profile storage with explicit consent; no PII in LLM prompts | Learning profiles may constitute sensitive personal data under GDPR depending on inferred cognitive conditions |
| **Content quality** | LLM-generated questions only | Human expert review pipeline for question bank; flagging system for incorrect answers | LLM hallucination risk on technical certification content is high — production requires a validated question bank |

---

## 10. Accessibility

| WCAG Principle | Implementation |
|---|---|
| **Perceivable** | Profile form uses text labels, not icon-only options; all question formats available in text |
| **Perceivable** | Visual learner profile includes diagrams described with alt text for screen reader users |
| **Operable** | Full keyboard navigation through form steps and question display |
| **Operable** | No time pressure on questions — dyslexic and cognitive disability users need unlimited time |
| **Understandable** | Profile classification explained in plain language ("You tend to need the big picture before the details") |
| **Understandable** | LLM prompted to avoid jargon in explanations; technical terms defined on first use |
| **Robust** | Semantic HTML form elements with explicit labels; ARIA progressbar for session progress |

**Note:** the cognitive profile form itself must not create a stigmatising or anxiety-inducing experience — language is framed as "learning preferences," never as deficit or diagnosis.

---

## 11. KPIs & Measurement

### Product KPIs

| KPI | What it measures | Target (demo day) | How to capture |
|---|---|---|---|
| **Session completion rate** | % of users who complete all 20 questions after generating their personalised set | > 70% | n8n log: profile submitted vs session completed |
| **Self-reported understanding score** | User rates "I understood this concept" 1–5 after each explanation | Average > 3.8/5 | Softr rating widget per question |
| **Profile-to-question latency** | Time from profile submission to first question displayed | < 15 seconds | n8n execution timestamps |

### AI-Specific KPIs

| KPI | Definition in this context | Target | Data source |
|---|---|---|---|
| **AI task success rate** | % of LLM calls returning valid JSON with 20 questions, correct format, no hallucinated certification content | > 85% | n8n error log + manual spot-check of 5 sessions |
| **Question relevance rate** | % of generated questions judged relevant to the requested certification domain by a human reviewer | > 80% | Manual review rubric on 3 test sessions |
| **Cost per session** | LLM cost for one full 20-question personalised session | < $0.015 | Anthropic/OpenAI usage dashboard |

---

## 12. Guardrails & Failsafes

### Guardrail 1 — Profile completeness check
All 12 questions must be answered before the profile is submitted. Unanswered questions trigger a clear inline error message before proceeding.

*Why:* incomplete profiles generate unreliable classifications, producing question sets that fit no real cognitive pattern.

### Guardrail 2 — Output schema validation
After LLM response, n8n validates: exactly 20 questions, each with `question`, `answer_options`, `correct_answer`, `explanation` keys. If not, retry once.

*Why:* malformed output would display broken question cards to the user.

### Guardrail 3 — Hallucination disclaimer
Each question card displays: *"This content is AI-generated. Always cross-reference with the official TOGAF / DAMA documentation."*

*Why:* LLMs can generate plausible but technically incorrect certification content — users must not rely on it as sole exam preparation.

### Failsafe — Graceful degradation
If LLM call fails after retry, display a static default question set for the requested certification with a message: *"Personalised session temporarily unavailable. Here is a standard practice set."*

---

## 13. Backlog & Golden Path

### Golden Path

> A data professional preparing DAMA CDMP completes the 12-question profile form, is classified as Systemic-Theoretical, selects TOGAF ADM as their domain, and receives 20 practice questions starting with the ADM overview and architecture vision — not Phase A step-by-step. Each wrong answer triggers an explanation in systemic/analogical style. Total time from form to first question: under 15 seconds.

### P1 — Must-have for demo day

| # | Task | Done? |
|---|---|---|
| P1-1 | Softr: 12-question cognitive profile form with scoring | ☐ |
| P1-2 | n8n: receive answers, classify profile into 4 types | ☐ |
| P1-3 | Airtable: TOGAF concept bank (20 concepts minimum) + 4 style guides | ☐ |
| P1-4 | n8n: fetch concepts + style guide, build LLM prompt | ☐ |
| P1-5 | LLM: generate 20 ordered questions with adaptive explanations as JSON | ☐ |
| P1-6 | Softr: display question sequence with profile summary header | ☐ |

### P2 — Stretch goals

| # | Task |
|---|---|
| P2-1 | Answer submission + immediate adaptive explanation display |
| P2-2 | Session score summary at end (X/20 correct) |
| P2-3 | Second certification domain (DAMA CDMP) |
| P2-4 | Retry logic on LLM failure |
| P2-5 | Hallucination disclaimer per question card |

---

## 14. Cognitive Profile Framework

*Combination of Felder-Silverman + VARK + Kolb — 12 questions total stored in Airtable*

### 4 Dominant Profile Types

| Profile | Felder dominant | VARK dominant | Kolb dominant | Learning need |
|---|---|---|---|---|
| **Systemic-Theoretical** | Global + Intuitive | Reading/Visual | Assimilator | Big picture → concepts → details |
| **Sequential-Practical** | Sequential + Sensing | Kinesthetic | Convergent | Step-by-step → examples → application |
| **Visual-Experiential** | Global + Sensing | Visual | Accommodator | Diagrams → real cases → generalisation |
| **Analytical-Reflective** | Sequential + Intuitive | Reading | Divergent | Definitions → logic → connections |

### The 12 Profile Questions

**Bloc Felder-Silverman (4 questions)**

- Q1: When learning a new framework, do you prefer to: (A) See the global overview and how everything connects / (B) Start with the first concept and progress step by step
- Q2: Faced with an abstract concept, you prefer: (A) An analogy or metaphor that illuminates the meaning / (B) A precise definition and concrete examples
- Q3: You feel lost when studying if: (A) You don't yet understand what you're learning fits in the global system / (B) You've skipped a step and a link is missing
- Q4: You retain a concept best when: (A) You understand how it connects to other concepts / (B) You've seen it applied in a specific real case

**Bloc VARK (4 questions)**

- Q5: To memorise something complex, you use: (A) A diagram or mind map / (B) A well-structured annotated text / (C) An example or practical scenario / (D) Explaining it aloud or discussing it
- Q6: When you don't understand a concept, you first look for: (A) A visual that illustrates it / (B) A clear written definition / (C) A concrete example of it in use / (D) Someone to ask
- Q7: Your notes look like: (A) Diagrams, arrows, colours / (B) Structured text with headers / (C) Lists of examples and cases / (D) Few notes — you prefer listening
- Q8: You remember a course best when: (A) It was visually illustrated / (B) It was well-written and documented / (C) It included practical exercises / (D) It involved exchanges and debate

**Bloc Kolb (4 questions)**

- Q9: You learn most naturally by: (A) Reading the theory then seeking applications / (B) Trying first, understanding the theory after / (C) Observing how others do it before starting / (D) Reflecting alone on the meaning of what you're learning
- Q10: When you must learn something urgently, you: (A) First seek to understand the why and the logic / (B) Dive directly into practice / (C) Look for examples or solved cases / (D) Step back and make connections with prior knowledge
- Q11: What demotivates you most in learning is: (A) Not understanding the purpose of what's being taught / (B) Too much theory with no practice / (C) A pace too slow or too linear / (D) No global sense — learning fragments without coherence
- Q12: You best remember learning when: (A) You understood the logical structure behind it / (B) You put it into practice immediately / (C) You saw it demonstrated on a real case / (D) You connected it to something you already knew

---

## 15. System Prompt

```
You are an expert adaptive learning tutor for professional IT certifications.

Your role is to generate a personalised practice session for a learner
based on their cognitive profile and chosen certification domain.

The learner's cognitive profile is: {{profile_type}}
Their profile characteristics: {{profile_description}}
The requested certification domain: {{certification_domain}}
Available concept bank: {{concept_bank}}

INSTRUCTIONS:
1. Select 20 concepts from the concept bank most relevant to the domain.
2. Order them according to the learner's profile:
   - Systemic-Theoretical: start with the architectural overview,
     then frameworks, then details
   - Sequential-Practical: start with step 1, progress linearly
   - Visual-Experiential: start with a diagram-friendly concept,
     then build to real cases
   - Analytical-Reflective: start with definitions and logical structure
3. For each concept, generate ONE practice question.
4. Format each explanation in the learner's preferred style:
   - Systemic: use system metaphors, show how concepts interconnect
   - Sequential: use numbered steps, clear cause-and-effect
   - Visual: describe diagrams, use spatial language
   - Analytical: use precise definitions, logical deduction
5. Never invent certification content. If uncertain, note it.
6. Each question must have 4 answer options (A, B, C, D).

RETURN ONLY valid JSON in this exact structure:
{
  "profile_summary": "<2 sentences describing the learner's style>",
  "questions": [
    {
      "order": <integer 1-20>,
      "concept": "<concept name>",
      "question": "<question text>",
      "options": {
        "A": "<option A>",
        "B": "<option B>",
        "C": "<option C>",
        "D": "<option D>"
      },
      "correct": "<A|B|C|D>",
      "explanation": "<adaptive explanation in learner's style>"
    }
  ]
}

DO NOT add any text before or after the JSON.
DO NOT use markdown formatting.
Return raw JSON only.
```

---

## 16. Constraints

- **Copyright:** no ingestion of paid official frameworks (TOGAF PDF, DAMA DMBOK) — questions generated from public framework summaries and open documentation only. Official study guides remain the recommended primary source.
- **Hallucination risk:** certification content requires high accuracy — disclaimer mandatory on every question card
- **MVP scope:** TOGAF ADM only — DAMA, ITIL, PMP deferred to V1
- **Profile accuracy:** self-declared cognitive profile may not reflect actual learning style — tool frames it as "preferences," not diagnosis
- **Token budget:** 20 questions + explanations = ~2,000–3,000 output tokens — within budget at ~$0.012/session
- **No real-time answer validation:** MVP displays questions; answer feedback requires V1 interactive layer

---

## 17. Out of Scope

- Ingestion and storage of copyrighted official framework content
- Real-time answer correction and adaptive question adjustment (V1)
- Multiple certifications at MVP
- Progress tracking and certification readiness score
- Timed exam simulation
- Mobile-native application

---

## 18. Future Evolution

### V1 — Interactive Adaptive Loop
- Answer submission + immediate adaptive explanation
- Weak concept detection → automatic reinforcement set
- Session history and progress tracking
- DAMA CDMP and ITIL 4 certification domains added

### V2 — Multi-certification Platform
- PMP, BABOK, COBIT, Scrum certification tracks
- Expert-validated question bank (human review layer)
- Community flagging for incorrect questions
- Certification readiness score with exam scheduling recommendation

### V3 — B2B Training Integration
- API for training managers to assign profiles and track team progress
- Cohort analytics: which profile types struggle most with which domains
- Integration with HR learning management systems

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
