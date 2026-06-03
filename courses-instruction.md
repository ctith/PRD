Exo 1 - Job Tracker w. Lovable
⏲️ Duration
1h - 1h15

🎯 Goal
Build a multi-user Job Tracker using Lovable, connected to a brand new Supabase project you will create for this challenge, push to GitHub, then deploy online via Lovable Publish.

🔧 Tools & Resources
Lovable.dev - Create project, with 2-way GitHub sync on main branch. ⚠️ Lovable’s free plan is limited to 5 prompts per day. Plan your prompts carefully before submitting them. If you reach the limit, move on to the next exercise.
Supabase - Create a new project (auth + Postgres)
GitHub - Canonical/source of truth after Lovable creates it
▶️ A walkthrough video is provided below the instructions, don’t hesitate to watch it to help you complete this challenge

📃 Instructions
Go to supabase.com and create a new project dedicated to this challenge.

Connect to Lovable, create a new project and in Integrations (⊕ button at the bottom left of the chat box), connect the Supabase project you just created.

Read the Lovable prompt provided below carefully before running it - make sure you understand what it’s asking Lovable to do and why.
The prompt is structured around Lovable best practices: it starts with a clear scope, asks Lovable to plan before implementing, and infers the schema from the database rather than writing SQL from scratch. Each section has a purpose - take a moment to identify them.


Copy and paste the prompt into Lovable and run it. Once the app has been created:
Verify everything is working properly
If you get errors, try solving them using Lovable’s “Fix it” feature

From Lovable, connect GitHub and let it create the repo in your account or org. Confirm the default branch is syncing.

💡 How to connect Lovable to GitHub ?


Click Publish to get a public URL. Lovable may ask you to complete additional steps before publishing, such as a security check. If so, click Run security check to let Lovable scan for potential security issues. Once the scan is complete, click Publish again.


Smoke-test the live app:
Sign up, sign in, sign out work end-to-end
Applications and Tasks CRUD (Create - Read - Update - Delete) work
Data is private per user
Check that user A cannot see user B data

Prompt to paste in Lovable
Don’t forget to link the Supabase project in the chat, before sending the prompt!

Goal: Build a Job Tracker app with a new Supabase project.

Context:

- Data and auth: create and connect a new Supabase project. Set up the database schema from scratch.

- Auth: use Supabase Auth (email/password).

Scope:

- Routes: "/", "/applications", "/applications/[id]", "/tasks".

- Features:

  - Email/password sign up, sign in, sign out

  - CRUD Applications (company, position, status, application date, notes)

  - CRUD Tasks linked to an Application (title, due date, done/not done)

  - Per-user data isolation: each user sees only their own data

- UI:

  - Navbar with links to Applications, Tasks

  - Forms with validation and toast feedback

Requirements:

1) Create the Supabase tables and enable Row-Level Security (RLS) so each user only sees their own rows.

2) Use the Supabase JS client. Ensure all queries are scoped to the signed-in user.

3) If anything is ambiguous, ask me to confirm before proceeding.

Guardrails:

- Keep the scope small and focused. Do not add unrelated pages or features.

- Keep changes small and reviewable.

Acceptance criteria:

- A1: user can sign up, sign in, and sign out.

- A2: creating an Application ties it to the signed-in user.

- A3: "/applications" shows only the current user's Applications.

- A4: the app is published via Lovable Publish and works end-to-end on the live URL.

Process:

- First, write a short plan of what you will build (tables, routes, components).

- Then implement in small, reviewable steps and preview each change before continuing.

🥇 Key Learning Points
Start in Lovable, then GitHub becomes the source of truth
Keep prompts scoped, explicit, and review changes incrementally
Deploy quickly, then iterate with more control
Lovable has an internal “git” tracking mechanism that gives you full version history

Exo 2 - CSV Import Feature
⏲️ Duration
45min - 1h

🎯 Goal
Extend your Job Tracker by adding a CSV import feature for Applications - directly inside Lovable, on top of what you built in the previous challenge.
The goal is to experience how easy it is to iterate on an existing app using AI: one short prompt, one new feature, no big rewrite needed.
Along the way, you’ll also compare two approaches to prompting - Vibe Coding vs AI-Assisted Coding - and see how the quality of your input shapes the quality of the output.

🔧 Tools & Resources
Lovable.dev - Continue from your existing project (previous challenge)
Supabase - Same project, no changes needed
GitHub - Sync is already set up

📃 Instructions
Step 1 - Generate sample CSV data with Vibe Coding
Before implementing the import feature, you need a CSV file to test it with.

Open your preferred AI assistant (Claude, ChatGPT, or the Lovable chat) and try this prompt:

Generate a CSV file with 10 sample job applications.
Run it and observe the result. Notice:

Does the column structure match your app’s schema?
Are the status values ones your app actually recognizes?
Would this CSV import cleanly?
This is Vibe Coding: fast, but vague. The output may work, or it may not.



Step 2 - Generate sample CSV data with AI-Assisted Coding
Now let’s be more intentional. Instead of hoping the AI guesses your schema, give it the full context. Read the prompt below before running it - notice what makes it different from Step 1 - and make all necessary edits to match your own app (e.g. make sure the statuses listed in the prompt are the same as the ones in your app).

# Task: Generate a sample CSV file for my Job Tracker app

## Schema
The Applications table has these fields:
- company (text, required)
- position (text, required)
- status (text: "applied" | "interviewing" | "offered" | "rejected" | "withdrawn")
- application_date (date: YYYY-MM-DD)
- notes (text, optional)

## Requirements
- Generate 10 rows of realistic data
- Use a mix of companies and positions across tech, finance, and consulting
- Distribute statuses: 4 applied, 2 interviewing, 1 offered, 1 rejected, 2 withdrawn
- Dates should span the last 3 months
- Add notes for 5 of the 10 rows
- Output as a valid CSV with a header row, nothing else
Compare this output to Step 1. The result should be clean, structured, and directly importable.

This is AI-Assisted Coding: you define the schema and constraints upfront. The AI has no room to guess wrong.

Save the generated CSV file - you’ll use it to test the import feature.



Step 3 - Add CSV Import to your app with Lovable
Go back to your Lovable project and run this prompt:

Add a CSV import feature for "Applications"
That’s it. Because Lovable already has full context of your app - the schema, the existing pages, the Supabase connection - a short follow-up prompt is often enough to iterate effectively.

Test it by uploading the CSV you generated in Step 2. Expect working code, but possibly without robust validation. If something breaks, use Lovable’s “Fix it” button before writing a new prompt.



Step 4 - Publish and verify
Publish your updated app via Lovable Publish (same URL as before, it updates automatically).

Smoke-test on the live version:

CSV upload works end-to-end
Imported applications appear in your list and belong to the signed-in user
No existing functionality was broken

🥇 Key Learning Points
Iteration doesn’t require a big prompt - when the AI already has context, a short focused prompt is enough
Spec-first prompting (schema, constraints, output format) consistently outperforms open-ended “vibe” prompts for data generation
Lovable preserves your full version history - if the new feature breaks something, you can roll back in one click

Exo 3 - Add a Stat Dashboard
⏲️ Duration
1h

🎯 Goal
Add a Stats dashboard to your Job Tracker using Cursor.

On purpose, the prompt is intentionally short: instead of specifying every KPI and chart, you’ll ask Cursor to explore the codebase first and decide what metrics are meaningful - then validate its plan before it writes any code.

🔧 Tools & Resources
Cursor - your project is already open from the previous challenge
GitHub - to commit and push the final result
Lovable - to redeploy once the feature is ready

📃 Instructions
Step 1 - Let Cursor plan the dashboard
Open Cursor’s chat window (Cmd+I on Mac / Ctrl+I on Windows) and switch to Plan Mode - This lets Cursor propose changes before actually editting the code.

Paste this prompt:

 I want to add a stats dashboard to this Job Tracker app.

 Before making any changes:
 1. Explore the codebase to understand the data model and existing pages
 2. Propose a plan: which KPIs and charts would be meaningful given what the app does
 3. Wait for my approval before implementing anything
Read the generated plan carefully. Ask yourself:
Do these metrics make sense for a job tracker?
Is anything missing or irrelevant?
If you want changes, tell Cursor before approving. Once you’re happy with the plan, tell it to proceed.


Step 2 - Preview the dashboard locally
Before deploying anything, you can run the app directly on your computer and see your changes live. This is one of the key advantages of working in an IDE (Cursor) over Lovable: you can test everything locally before it goes online.

Open a terminal in Cursor (Terminal > New Terminal) and run:

 npm run dev
Cursor will give you a local URL (usually http://localhost:5173) - open it in your browser. Navigate to the stats page and check that the charts and KPI cards render correctly with your data.

Any change you make in the code will automatically refresh in the browser - no need to restart.

If something is broken, paste the error into Cursor’s chat (Cmd+L / Ctrl+L) and ask it to fix it.

🖱️ Want to skip the terminal for now?


Step 3 - Commit and push
Once you’re satisfied with the result:

git add .
git commit -m "Add stats dashboard"
git push
🖱️ Want to skip the terminal for now?


Step 4 - Redeploy via Lovable
Because of the 2-way sync between GitHub and Lovable, your changes are already in Lovable. Go to your Lovable project and hit Publish to update the live URL.

Check that the stats page works on the deployed version.

🥇 Key Learning Points
Asking the AI to plan before implementing consistently produces better results than jumping straight to code
Local preview is a superpower: test everything on your machine before it goes live, with instant feedback on every change
A short, well-framed prompt can outperform a long, detailed one - when you give the AI room to explore the codebase, it often finds the right approach on its own
Cursor’s Composer is the right tool for features that touch multiple files at once
Reviewing AI-generated changes before running them is a habit worth building early

Exo 4 - From Vibe Coding to TDD
⏲️ Duration
1h - 1h15

🎯 Goal
Rebuild the CSV import feature from scratch using Warp and a TDD approach: write the tests first, watch them fail, then let Warp implement the code to make them pass.

In Challenge 2, you added CSV import with a single short prompt in Lovable. It probably works - but you have no idea if it correctly handles bad data, missing fields, or duplicates. This challenge answers that question by defining exactly what the feature must do before writing a single line of code.

💡 What is Warp?
Warp is a terminal with a built-in AI agent. Unlike Cursor where you write code in an editor and ask AI for help, Warp works entirely from the command line: you describe what you want in plain English, and it reads your codebase, proposes a plan, and executes changes autonomously - running commands, editing files, and fixing errors on its own.

The key difference from tools you’ve used so far: Lovable controls its own hosted environment, Cursor helps you edit files in your project, and Warp controls both files and terminal commands. It is more autonomous than Cursor - you can ask it to run tests, see the failures, fix the code, and re-run, all in one go without switching between windows. That makes it particularly well suited for TDD.

🔧 Tools
Warp - terminal-based AI agent, download it here
GitHub - canonical repo
Lovable - to redeploy once done
⚠️ Warp resets its request limit every 5 hours. If you hit the cap mid-challenge, switch to Cursor or Claude Code to keep going.

📃 Instructions
Step 0 - Install Warp
🛠️ First time setup


Step 1 - Remove the existing CSV feature
Before rebuilding properly, you need to remove what Lovable generated in Challenge 2. Don’t do it manually - ask Warp to do it:

Remove the CSV import feature from this app entirely.
This includes any UI components, helper functions, and related code.
Do not remove anything else. Show me what you plan to delete before making any changes.
Review what Warp plans to remove
Confirm, then let it proceed
Run the app locally to verify everything else still works:
npm run dev


Step 2 - Write the tests first
This is the core of TDD: you write the tests before the implementation exists. This forces you to define exactly what the code must do before writing a single line.

Think of it like writing the marking criteria for an exam before the student has studied. The student (Warp) then has to produce exactly what’s needed to pass.

Ask Warp to write the tests:

I want to rebuild the CSV import feature using TDD.

First, set up Vitest if it's not already installed.

Then write unit tests for a CSV import utility with these behaviours:

- parseCSVRow maps a raw CSV row to an application object (company, position, status, application_date, notes)
- validateRow returns an error if company or position is missing
- validateRow returns an error if status is not one of: applied, interviewing, offered, rejected, withdrawn
- validateRow returns an error if application_date is not a valid date
- validateRow returns no errors for a valid row
- isDuplicate returns true when company + position + application_date match an existing record
- isDuplicate returns false when no match exists

Write only the tests. Do not implement anything yet.
Read through the tests Warp generated
Do they describe the behaviours you actually want?
Ask Warp to adjust anything that doesn’t look right before moving on


Step 3 - Run the tests (they will fail)
npm run test
Every test will fail. This is normal and expected - it’s called the red phase.

This is the key moment of TDD: you’ve defined what the code must do before writing a single line of implementation. The failing tests are your spec.



Step 4 - Let Warp make them pass
Now ask Warp to implement the functions:

Now implement the CSV import utility to make all the tests pass.
Create the utility file with the three functions: parseCSVRow, validateRow, isDuplicate.
Do not modify the test file.
Run the tests again:

npm run test
All tests should now pass - this is the green phase. If some still fail, paste the error output into Warp and ask it to fix the implementation.



Step 5 - Add the UI
The logic is tested and working. Now ask Warp to connect it to the app:

Add a CSV import button to the Applications page that uses the utility we just built.
On upload, parse and validate the CSV, show a preview of valid rows, then import them into Supabase on confirm.
Show a summary after import: how many rows were imported and how many were skipped.
Test it in the browser using the sample CSV file from Challenge 2:

npm run dev


Step 6 - Commit, push and redeploy
Once everything works:

git add .
git commit -m "Rebuild CSV import with TDD"
git push
Go to Lovable and hit Publish to update the live URL. Verify the import works on the deployed version.

🥇 Key Learning Points
TDD is red-green-refactor: write a failing test, make it pass, clean up - in that order
Writing tests first forces precision: you can’t write a test for something vague
AI is exceptionally good at TDD: it generates exhaustive edge cases faster than any human and can run the full cycle autonomously
The one thing AI can’t do: decide what to test and why - that judgment still comes from you
Using different AI tools on the same codebase (Lovable, Cursor, Warp) is a real professional skill - each has limits, and knowing when to switch is valuable

Prompt Template example
<spec>
  Feature: <one-sentence goal>
  Context:
    - Next.js (App Router) + TypeScript + Tailwind + shadcn/ui
    - Supabase (Postgres + Auth), Vercel hosting
  Requirements:
    - <bullet 1>
    - <bullet 2>
  Tests:
    - <T1: expected behavior>
    - <T2: access control / edge case>
  Guardrails:
    - Do not change DB schema or Supabase RLS
    - Small plan first, then small diffs
</spec>

Example AGENTS.md (short example)
# AGENTS.md — AI Guidelines

Tech: Next.js (App Router) + TypeScript + Tailwind + shadcn/ui  
DB/Auth: Supabase (enforce RLS) • Hosting: Vercel

Rules:
- Plan first, implement in small diffs
- Tests before features when possible
- Do not change DB schema or RLS without approval
- Scope queries to current user; prefer server actions for writes
- Keep components small; responsive by default
Example Requesting Tests in Prompt
<tests>
  T1: Create Application → belongs to current user
  T2: Other users cannot read it (RLS)
  T3: Build succeeds locally and on Vercel
</tests>

Example: XML-Tag Parser Spec
<spec>
  Feature: Parse job description text to fields
  Requirements:
    - Output JSON with {company, role, location, seniority}
    - Null for unknowns; ensure data constraint from /path/to/company.ts types
  Tests:
    - Missing fields → null
    - Multiple roles → pick most likely
</spec>

Example: “Plan First” Prompt Add-On
<instructions>
Before coding, output:
1) Detected schema summary
2) File plan (add/modify)
3) First small diff
Stop for review.
</instructions>

Takeways
Key learnings from the session
TL;DR
AI-assisted coding beats vibe coding when you front-load clear specs and context. Vibe coding costs can climb quickly, tools differ a lot in context handling, and you must keep database constraints, migrations, and secrets management under control to avoid long, costly detours.

Strongly Recommended habits
Start each new chat with a mini-PRD: goals, inputs, outputs, constraints, schema snippets, and acceptance tests.
Keep GitHub as the single source of truth and run all tools against that repo.
Use migrations for every schema change and test DB constraints early.
Track spending and prefer tools that show per-action cost.
Avoid vendor lock-in. Retain ability to switch tools without losing context.
Takeaways
Vibe coding vs AI-assisted coding
Vague prompts produced generic or wrong outputs and wasted time and money.
Precise specs or PRDs first, then implementation, yielded results aligned with the repo and DB schema.
Treat the AI like a junior collaborator. If you would not brief a human that vaguely, do not brief the AI that way.
Context is king
Tools that could read more of the repo and prior chats performed better at inferring intent and matching existing schemas.
Cross-tool handoffs worked when the same Git repo acted as the single source of truth.
Tooling comparison
Claude Code: transparent per-action cost, but weaker repo context handling, limited UX for attaching screenshots, inability to automatically follow-up on issues, needed lots of explicit explanation and more time to get the desired outcome.
Cursor and Warp: stronger context use and repo awareness, easier to paste images and iterate, faster to converge on real issues.
Mixing tools is useful, but subscriptions add up, so you may eventually pick one primary tool, per “role” (one for initial scafolding of web app and another for iterating on existing codebase).
Cost awareness (Claude Code only)
Small runs already incurred non-trivial spend.
About 3 cents per simple request in one example.
Roughly 18 cents to turn a big prompt into CSV (0.11 to 0.29).
Roughly 40 cents to scaffold features (0.29 to ~0.70).
Cumulative observed total around 1.22 USD during the session.
Seeing explicit costs helps decide when to do tasks manually.
Other tools (Warp, v0, Lovable, etc.) don’t have this level of cost transparency
Database truth vs UI truth
One real blocker came from a Supabase constraint on application_status that did not match app types or CSV values. Spent approx. an hour to identify/solve.
UI and types listed 4 statuses while DB logic expected more. AI changed UI and CSV but missed the DB constraint until guided by human.
Lesson: align schema, constraints, types, and seed data. Keep authoritative migrations checked in and consistent.
Migrations and environment setup
Reusing V0 or prior migrations could have prevented status drift.
Version every schema change. Verify connection strings and tool access.
AI has access to all tools as admin, security is a real issue that isn’t solved yet. If you get hacked, AI will help offender as much as it tries to help you.
Secrets and safety
Do not commit raw connection strings or passwords.
Avoid pasting secrets into AI contexts when possible. Prefer scoped tokens.
Developer ergonomics
Rapid multi-tool iteration is powerful, but you must stay in control.
Keep commits small, review diffs, and back up with Git.
When asking for fixes, include screenshots and exact error text, plus where and when it occurred.
