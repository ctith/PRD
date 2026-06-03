Goal
Formalise your validated idea into a structured PRD (Product Requirements Document). You arrive with a chosen project idea and a named AI use-case from Module 3, Challenge 5. This challenge is about deepening the AI strategy thinking and writing everything down in one place.
Tools & Resources

Your preferred LLM chat assistant (ChatGPT, Gemini, Le Chat…)
Notion (recommended for your PRD)
PRD Template: at the bottom of this challenge
Instructions
Notion is a great tool to write your PRD. You can combine a Notion page with AI to speed things up. Your PRD will be updated throughout the next challenges - treat it as a living document.
Step 1 - Write the problem statement
Start your PRD draft with the following:

Target user: who are they, where are they, in what context does the problem occur?
Pain: what is the specific friction or frustration they face? Goal Decide on a POC tech stack and produce an architecture diagram showing where the AI step fits. Then sketch what a production-grade version of the same system would look like. Tools
[Whimsical](https://whimsical.com/) - for your architecture diagram (use the flowchart mode, or describe your architecture to Whimsical AI to get a starting point)
Your PRD Instructions If you are unsure about your tech stack at this point, start experimenting and come back to this challenge once you have a clearer picture. The goal is not to find the perfect stack - it is to make your choices explicit and justify them. Step 1 - Draft your POC tech stack List every tool and service you plan to use to build the prototype:
Frontend (if any): frameworks, UI builders, no-code tools
Backend / logic layer: serverless functions, automation platforms, APIs
AI layer: LLM provider, model, integration method
Data layer: database, file storage, vector store (if applicable)
Hosting / deployment For each component, note:
Why you chose it for the POC (speed, familiarity, cost, integration…)
Any known constraints: rate limits, token costs, data residency, free tier caps Step 2 - Place the AI component in the architecture Produce a diagram showing the full flow of your product, with the AI step clearly identified.
Pick the diagram type that fits your product best:
[Flowchart](https://whimsical.com/simple-flowchart-Go1oMu43dcMnR1CYGefDZQ): good for products with branching logic or multiple interconnected services - use this to show components and how they connect
[Sequence diagram](https://whimsical.com/uml-sequence-diagram-Tmq3yfX8rgX65Vo9Dm5q5r): good for products with a clear request/response flow - use this to show the order of interactions between the user and the system over time
You can build it manually or describe your architecture in the Whimsical AI prompt to get a starting point you can then adjust. If you’re unsure, start with a flowchart
Your diagram must show, at minimum:
The user action that triggers the AI step
What goes into the AI step (data, format)
What the AI step produces (output, format)
What happens before and after in the user journey
Any guardrail or failsafe you are planning to implement Example Diagrams
Example 1: CV screening tool - [View flowchart](https://app.dust.tt/share/frame/bed84dbc-0adf-4cfa-88de-86ef9df091b0)
Example 2: Invoice follow-up generator - [View sequence diagram](https://app.dust.tt/share/frame/bc16985a-953a-4d15-b3b3-70e6af9fab3b)
Step 3 - Draft a production-grade target
You won’t build this - but thinking ahead helps you avoid decisions now that would be painful to undo later.
Based on your POC stack, identify what would need to change to run this product in production under real-world constraints. Pick 3 to 5 areas from the list below and for each, write one sentence on what you would change and why:
   * Scale: what breaks first if you go from 10 to 10,000 users?
   * Reliability: retries, error handling, monitoring, alerting
   * AI layer: would you switch models, add fallbacks, fine-tune?
   * Cost control: rate limiting, caching, request batching
   * Privacy & compliance: data residency, PII handling, audit logs
   * Security: auth, input sanitisation, secret management

🥇 Key Learning Points
   * Making your tech stack explicit forces you to spot hidden dependencies and constraints before they surprise you mid-build.
   * The diagram is not decoration - it is the fastest way to check whether the AI step is well-integrated or bolted on.
   * Separating POC needs from production needs keeps you moving fast now without losing sight of what scale would require.
* Current workaround: how do they deal with it today?
* Impact: what does this cost them (time, money, quality, stress…)?
* Why now: why is this a good moment to tackle this problem?
Keep it tight - 1 to 3 bullet points per item is enough at this stage.
Step 2 - Quick market & alternatives scan
Before committing to a solution direction, check what already exists.

Use an AI assistant to identify 3 to 5 existing solutions (direct competitors, partial alternatives, or common workarounds).
For each, note: what they do well, what they do poorly, and any relevant constraints (pricing, data access, integration limits).
Based on this scan, update your solution direction in the PRD if needed, or confirm it as-is and move on.
Step 3 - Write the Strategic Role Statement
You already named your AI use-case at the end of the previous module. Now formalise it.

Describe precisely what the AI component does and where it sits in the user journey.
Write a Strategic Role Statement: one to two sentences explaining why the product cannot deliver its core value at scale without this AI step. Example 1: “A freelance designer might have 5 clients - chasing invoices manually takes 10 minutes. At 50 clients, that’s over an hour a week. AI is what makes the product worthwhile as the workload grows.”
Note the user value unlocked: time saved, quality improvement, throughput, reduced cognitive load…
Add this to your PRD under a section called “AI use-case & strategic role”.
Step 4 - Capture early risks
List the risks you are already aware of. No need to go deep - you will revisit these in later challenges. For now, aim for 3 to 5 bullet points covering:

Data availability or quality issues
Model limitations relevant to your use-case
Likely failure modes
Privacy or compliance concerns
PRD template
Use this as the structure for your Notion page:

Problem statement (1-2 sentences)
Target user & context (who / where / when)
Value hypothesis (what outcome improves)
Constraints (time, budget, data)
Market scan (3-5 alternatives, gaps identified)
Solution concept (smallest slice to prove value)
AI use-case & strategic role
What the AI step does and where it sits on the golden path
Strategic Role Statement
User value unlocked
Early risks (data, model limits, failure modes, privacy)
Out of scope (explicitly not doing now)
🥇 Key Learning Points

A good PRD is not long - it is precise. Every section should help you or someone else make a faster decision.
The Strategic Role Statement forces you to verify that AI is load-bearing in your product, not just decorative.
Writing down risks early does not slow you down - it focuses your attention on what actually matters to validate first. Goal Define how you will measure success, design at least one control to keep the AI step safe, and build a prioritised backlog for the next 2 days of building. Tools
Your PRD
Notion, GitHub Projects, Trello, or Linear for the backlog Instructions If you get stuck on KPIs or guardrails, move on and come back later. The goal is to make you think about measurement and risk early - it does not need to be perfect on the first pass. Step 1 - Pick outcome-oriented KPIs Choose 2-3 leading indicators that will tell you whether your solution actually solves the problem. For each KPI:
Write what it measures and why it matters
Define how you will capture it in the prototype (log, counter, manual observation…)
Note if it is trackable now or only later, and why. Some KPIs will be important but not measurable in a prototype - that is fine. What matters is knowing what you should be tracking and what you are deferring. Examples: end-to-end task completion time (shows real time saved), error or rework rate (shows reliability), user drop-off point in the golden path (shows where friction is). Step 2 - Select 2 to 3 AI-specific KPIs These metrics tell you whether the AI step is helping or hurting. They also give you a quick way to justify viability during a demo or pitch.
Pick 2 to 3 from the list below. For each, write: what it means in your specific context, how you will capture it, and a realistic target for demo day.
AI task success rate - are outputs usable for your specific goal? (assess against a simple rubric or a small labeled sample)
Escalation or fallback rate - how often does the AI fail to handle the task and need a human or fallback?
Latency p50 and p95 - typical and worst-case response times for the AI step
Cost per AI task - rough estimate of what each call costs at current usage
For each selected metric, add one sentence on the data source (provider dashboard, your log table, manual spot-check). Step 3 - Define at least one guardrail or failsafe Pick at least one of the following guardrail and/or failsafe and write one sentence in your PRD explaining why it fits your specific risks. Guardrails prevent bad inputs or outputs from flowing through undetected:
Input validation before hitting the AI (reject empty, malformed, or oversized inputs)
Maximum input size or early truncation to control cost and latency
Confidence threshold check before auto-applying an AI output
Output format check (enforce expected schema or required keys) Failsafes help you recover when the AI struggles:
Manual review or confirmation step before a risky action is applied
Fallback to a simpler rule or a human-assigned path when confidence is low
Retry strategy on transient errors, with a clear message if retries fail Step 4 - Build your backlog Before you start building, answer two questions:
What is your golden path? Write one sentence describing the end-to-end flow you will demo on day 2.
What are your 3 to 5 must-have tasks to make that flow work? List them - these are your P1s, any other idea can be labeled as P2. Add this to your PRD. Step 5 - Finalise your PRD Make sure you added all topics related to performance & backlog in your PRD:
Your 2-3 product KPIs with a short measurement plan
Your 2-3 AI KPIs with target and data source
The guardrail or failsafe you plan to implement, plus any stretch ideas
Your backlog with P1/P2 labels and your golden path definition
🥇 Key Learning Points
   * Tying a prototype to measurable outcomes is what separates a demo from a product.
   * AI-specific KPIs give you a fast, credible way to evaluate whether the AI step is worth keeping.
   * A short backlog with clear priorities prevents scope creep and keeps the golden path in focus. Goal
Ship a working prototype that demonstrates the core user value on your golden path.
Tools
Depends on your chosen tech stack - refer to the architecture you designed in Challenge 2.
Instructions
This challenge is intentionally less guided than the previous ones - you have everything you need: a PRD, an architecture, KPIs, and a prioritised build plan. Time to put it all together and ship something real !
Step 1 - Review your PRD and golden path
Before writing a single line of code or submitting a single prompt, take 5 minutes to re-read:
      * Your golden path (the one flow you will demo)
      * Your P1 tasks (the 3 to 5 must-haves)
      * Your AI use-case and where it sits in the flow
This is your north star for the next hours. If you feel the urge to add something that is not on this list, write it down as a P2 and keep going.
Step 2 - Submit your first big prompt
Start with a single, detailed prompt that describes the full scope of what you want to build. Give the AI the full picture upfront: paste your PRD, attach screenshots, diagrams, or any design reference that helps it understand what you are after. The more context, the less back and forth.
Step 3 - Iterate and fix
Once you have a first working version:
      * Test the golden path end to end
      * Fix errors before adding anything new
      * Extend or improve only if the golden path is fully working and you have time
When you hit a blocker, pivot quickly: swap a tool, simplify a step, or drop a P2. Do not spend more than 20 minutes stuck on the same issue.
Step 4 - Publish
Deploy your prototype so it is accessible for demo day. A working link is worth more than a polished local version that cannot be shared.
️ Optional. Step 5 - Write down your learnings
If you have time, add a “Learnings” section to your PRD:
      * What unexpected challenges did you face, and how did you adapt?
      * Did you reach your P1 goals? If not, what stopped you?
      * What would you do differently in the next iteration?
🥇 Key Learning Points
      * The PRD is not just a document - it is the brief you hand to the AI. The clearer it is, the faster you build.
      * A working golden path on demo day beats a half-finished product with many features.
      * Blockers are normal. Pivoting quickly is a skill, not a failure. Goal
Keep the momentum going. Pick your next most impactful tasks and ship them - your prototype should get meaningfully better by the end of this challenge
Tools
Your chosen tech stack
Instructions
Step 1. Pick your targets
         1. Review your backlog and your v1 prototype. Ask yourself: what is the one thing that, if fixed or added, would make this demo significantly more convincing?
         2. Select 2-3 tasks to work on - either from your backlog (P1 items) or new ideas that came up while building your v1 prototype. Write them down before you start.
Rule of thumb: if a task takes more than 30 min and you’re stuck, descope it and move on.
         3. Work through your selected tasks one by one.For each task:
            * Implement it
            * Test it on the golden path
            * Note any unexpected issues or decisions you made
Step 2. Update your PRD
Add a short “Iteration Notes” section to your PRD (3-5 bullets):
         * What you built or improved
         * What you skipped and why
         * What still feels risky or incomplete
🥇 Key Learning Points
         * Focused iteration beats scattered effort - two well-executed tasks are worth more than five half-finished ones.
         * Every build session should leave the demo in a better state than it started.
         * Tracking trade-offs in your PRD is a habit that compounds over time. Goal
Wrap up your project with a short video demo that communicates the value of what you built - clearly and convincingly.
Tools
            * For recording: [Loom](https://www.loom.com/) (or equivalent)
            * Your prototype
            * Presentation support (slides, Gamma, etc.)
Instructions
Step 1. Prepare your pitch deck and script
Build a short deck (5 slides max) using AI (ChatGPT, Gamma, etc.) to speed things up. Structure both your slides and your video script around these 4 sections:
            1. Context - who is this for, and why does it matter now?
            2. Problem - what pain are you solving?
            3. Solution - what did you build, and how does it work?
            4. Proof - live demo of the golden path + 1 or 2 KPIs
Step 2. Rehearse once, then record (10 min max)
When you record:
            * Follow the golden path from start to finish, in order, without skipping steps. Showcase the product as if you were a real user - not a developer who built it.
            * Narrate what the user is trying to achieve, not what the code is doing
            * If something breaks or lags, keep going - comment it briefly and move on
            * Close with your main limitations and what you would tackle next
Tip: Treat it like a live investor demo: no “let me just quickly fix this”, no switching to the code editor, no explaining what you meant to build.

🥇 Key Learning Points
            * Lead with user value, not technical details.
            * A short, well-structured demo is more persuasive than a long one.
            * Naming your limitations shows maturity - it builds trust with your audience.
