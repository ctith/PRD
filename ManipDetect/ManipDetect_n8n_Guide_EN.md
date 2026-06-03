# ManipDetect — n8n Workflow Technical Guide
### Building the backend step by step

![Stack](https://img.shields.io/badge/stack-n8n%20%7C%20Airtable%20%7C%20Jina%20%7C%20LLM-blue)
![Duration](https://img.shields.io/badge/estimated%20time-2--3h-orange)
![Level](https://img.shields.io/badge/level-no--code%20beginner-green)

> This guide assumes Lovable has already generated the user interface.
> The goal here is to build the product's brain: the n8n workflow
> that receives text or a URL, analyses it, and returns the manipulation report.

---

## What we're building

```
[Webhook] → [IF: text or url?]
                ↓ url           ↓ text
           [Jina scrape]    [pass through]
                ↓                ↓
           [Merge both branches]
                ↓
           [Code: normalise the text]
                ↓
           [Airtable: fetch taxonomy]
                ↓
           [LLM: analyse + JSON]
                ↓
           [Respond to Webhook]
```

**8 nodes. No server-side code. No Supabase required.**

---

## Accounts to create before you start

| Tool | Role | Cost | Link |
|---|---|---|---|
| **n8n cloud** | Host the workflow | Free (starter plan) | n8n.io |
| **Airtable** | Store the 18 techniques | Free | airtable.com |
| **OpenAI** or **Anthropic** | LLM API call | ~$5 in credits is enough for testing | platform.openai.com or console.anthropic.com |
| **Jina.ai** | Scrape URLs | Free, no account needed | r.jina.ai |
| **Lovable** | User interface | Free (basic plan) | lovable.dev |

> Supabase: **not required for MVP.** Only needed if you want to store
> analysis history — a V1 feature.

---

## STEP 0 — Set up Airtable

Before touching n8n, the taxonomy needs to be ready in Airtable.
The workflow fetches it on every analysis.

### 0.1 Create the base

1. Go to **airtable.com** → sign in
2. Click **"Add a base"** → **"Start from scratch"**
3. Name the base: `ManipDetect`

### 0.2 Create the table

1. In the base, rename the default table to `Manipulation_Taxonomy`
2. Create these 6 columns:

| Column name | Type |
|---|---|
| `Name` | Single line text |
| `Definition` | Long text |
| `Mechanism` | Long text |
| `Signals` | Long text |
| `Example` | Long text |
| `Suggestion` | Long text |

### 0.3 Fill in the 18 rows

Copy and paste each entry below into Airtable.

---

#### Interpersonal techniques (12)

**#01 — False urgency**
- Definition: Creating artificial time pressure to prevent reflection and force a quick decision.
- Mechanism: Short-circuits rational analysis by triggering fear of missing out or facing a consequence.
- Signals: "you need to decide now", "this is my final offer", "you have until tonight", "we can't wait", "the deadline expires"
- Example: "I need your answer today, otherwise I'll have to offer the position to someone else."
- Suggestion: Name the pressure and slow down — "I'll take the time I need to make the right decision."

**#02 — Guilt-tripping**
- Definition: Attributing responsibility for a negative situation to the other person, placing them in a position of moral debt.
- Mechanism: Exploits the need for approval and fear of rejection — the guilt-tripped person tries to "fix" things even when they're not at fault.
- Signals: "because of you", "you've disappointed me", "I never thought you'd do this", "after everything I've done", "it's your fault"
- Example: "It's because of your refusal that we're in this situation. I hope you're proud of yourself."
- Suggestion: Separate facts from interpretation — question the causality presented as obvious.

**#03 — DARVO**
- Definition: Deny, Attack, Reverse Victim and Offender — the perpetrator presents themselves as the victim when confronted.
- Mechanism: Destabilises the other person, who ends up justifying themselves for daring to raise an issue.
- Signals: "I never said that", "you're attacking me", "you're the one hurting me", "I'm the victim here", "you're being unfair to me"
- Example: "I did nothing wrong. You're accusing me without proof — you're causing me enormous harm by saying this."
- Suggestion: Return to concrete, documented facts. Do not respond to the counter-accusation.

**#04 — Double bind**
- Definition: Placing the other person in front of two options, neither of which is favourable — the illusion of choice masks the absence of a good outcome.
- Mechanism: Generates paralysis or resignation — every answer is a losing one, reinforcing the sense of powerlessness.
- Signals: "either way", "whatever you do", two options presented as exhaustive without a third possibility
- Example: "If you accept, you show you don't hold to your principles. If you refuse, you show you're not a team player."
- Suggestion: Refuse the frame — "I don't recognise these two options as the only ones available."

**#05 — Responsibility reversal**
- Definition: Shifting the burden of proof or action onto the person who was harmed, as if they were responsible for what happened.
- Mechanism: Relieves the sender of all responsibility while keeping the other person in a position of obligatory action.
- Signals: "you should have", "if you'd been more careful", "it's up to you to fix it", "you brought this on yourself", "you should have anticipated"
- Example: "If you'd communicated better from the start, we wouldn't be here. It's up to you to find a solution."
- Suggestion: Name the reversal — "I note that responsibility is being attributed to me without examining the facts."

**#06 — Deliberate ambiguity**
- Definition: Deliberately formulating a vague message to keep a way out while creating a false impression.
- Mechanism: Allows the sender to backtrack ("that's not what I said") while having already created an expectation or pressure.
- Signals: "in a way yes", "it's possible", "we'll see", "it depends", promises without a clear commitment, date, or criterion
- Example: "We'll see about your promotion — things are moving in the right direction."
- Suggestion: Ask for an explicit, written reformulation — "Can you specify what that means concretely?"

**#07 — Implicit authority appeal**
- Definition: Invoking an unnamed authority to lend weight to a debatable position and inhibit challenge.
- Mechanism: Questioning the message becomes equivalent to opposing a vague but intimidating authority.
- Signals: "everyone knows that", "management has decided", "it's standard practice", "experts agree", "I've been told that"
- Example: "Management is very concerned about your attitude — several people have raised issues."
- Suggestion: Ask to name the authority and the specific facts — "Who exactly, and what facts?"

**#08 — Minimisation**
- Definition: Reducing the legitimacy of a feeling or problem to avoid having to address it.
- Mechanism: Invalidates the other person's experience — they end up doubting the legitimacy of their own perception.
- Signals: "you're overreacting", "it's nothing", "you're too sensitive", "everyone goes through this", "you're being dramatic", "it's not that serious"
- Example: "You're complaining for nothing — everyone has tough targets, it's not specific to your case."
- Suggestion: Maintain your perception without excessive justification — "My experience is real, whether or not it's shared."

**#09 — Textual gaslighting**
- Definition: Denying or rewriting past facts to make the other person doubt their own memory or perception.
- Mechanism: Creates cognitive instability — the person spends their energy validating reality rather than responding to the substance.
- Signals: "I never said that", "you're imagining things", "you're making it up", "you're misremembering", "that's not what happened"
- Example: "I never promised you a raise. You must have misunderstood what I said in that meeting."
- Suggestion: Rely on written evidence — emails, meeting notes. Do not argue about memory.

**#10 — Instrumental flattery**
- Definition: Giving targeted compliments before a request to create a sense of reciprocal obligation.
- Mechanism: Exploits the reciprocity rule — after receiving something (even just a compliment), people feel indebted.
- Signals: "you who are so competent", "I knew I could count on you", "you're the only person who can do this", excessive compliment immediately before a request
- Example: "You're truly the best in this domain — I need you to handle this urgent file this weekend."
- Suggestion: Separate the compliment from the request — evaluate the request on its own merits.

**#11 — Projection**
- Definition: Attributing one's own intentions, feelings, or problematic behaviours to the other person.
- Mechanism: Shifts attention from the sender's actual behaviour to a supposed behaviour of the receiver.
- Signals: "you're trying to manipulate me", "you're being aggressive", "you're lying", "you want to hurt me", mirror accusations
- Example: "I can see very well that you're trying to manipulate me with your arguments. You're the one playing games here."
- Suggestion: Do not respond to the mirror accusation — return to observable, documented facts.

**#12 — False consensus**
- Definition: Presenting one's position as shared by a silent majority to isolate the other person.
- Mechanism: Activates fear of social exclusion — opposing means being alone against everyone.
- Signals: "everyone thinks that", "nobody understood why you", "the team agrees that", without naming the individuals
- Example: "Honestly, everyone on the team finds you difficult to work with. I'm the only one still defending you."
- Suggestion: Ask to identify "everyone" — anonymous consensuses don't exist.

---

#### Marketing / copywriting techniques (3)

**#13 — Artificial social proof**
- Definition: Presenting unverifiable numbers or testimonials to simulate crowd validation.
- Mechanism: Inhibits critical thinking — if thousands of people made this choice, questioning it seems irrational.
- Signals: round numbers without sources, "over X satisfied customers", testimonials without verifiable identity, decontextualised press logos
- Example: "Over 10,000 entrepreneurs have transformed their lives with this method."
- Suggestion: Ask for the source, date, and methodology behind the figure.

**#14 — Artificial scarcity**
- Definition: Creating a false impression of limitation (stock, spots, time) to trigger a quick decision.
- Mechanism: Combines false urgency and false exclusivity — what is scarce is perceived as precious.
- Signals: "only X spots left", countdown timers, "limited edition", "offer valid until midnight", decreasing stock indicators
- Example: "Only 3 spots left for this programme. The offer expires in 47 minutes."
- Suggestion: Check if the scarcity is real by refreshing the page the next day.

**#15 — Price anchoring**
- Definition: Displaying a high crossed-out reference price to make the real price seem like a bargain.
- Mechanism: The brain evaluates the real price relative to the anchor displayed, not relative to the product's intrinsic value.
- Signals: crossed-out prices, "real value: X€", "save X%", before/after price comparisons, "instead of"
- Example: "Online course: ~~€2,497~~ Today only: €197. Save 92%."
- Suggestion: Evaluate the displayed price against real value and market alternatives, not the crossed-out price.

---

#### Dropshipping / e-commerce techniques (3)

**#16 — Fake testimonials**
- Definition: Presenting fabricated or purchased customer reviews to simulate non-existent social validation.
- Mechanism: Exploits social proof — dozens of positive reviews short-circuit personal judgement.
- Signals: generic profile photos, anglophone first names on French sites, exclusively 5-star ratings, grouped dates, unrealistically positive text
- Example: "⭐⭐⭐⭐⭐ Incredible product, fast delivery, I highly recommend! — Sarah M., 2 days ago."
- Suggestion: Look for reviews on independent sites (Trustpilot, Google). Verify profile photos via reverse image search.

**#17 — Fake trust badges**
- Definition: Displaying certification or secure payment logos that are not verifiable or functional.
- Mechanism: Transfers the legitimacy of a recognised institution to the site through simple visual association.
- Signals: decorative non-clickable Visa/Mastercard logos, "certified by" without a link, invented quality labels, generic padlock icons
- Example: A site displaying 6 security logos, none of which link to a real verification page.
- Suggestion: Click on each badge — a legitimate badge links to an external verification page. If nothing happens, it's decorative.

**#18 — Generic product presentation**
- Definition: Using visuals and descriptions copied directly from supplier catalogues without any value added.
- Mechanism: Creates an illusion of value-add when the product is identical to what is sold for a fraction of the price at the source.
- Signals: photos identical to AliExpress listings, uniform white backgrounds, auto-translated descriptions with awkward phrasing, unknown brand
- Example: "Trendy luxury watch 2024 — Express delivery from our European warehouse." (photos identical to 47 AliExpress sellers)
- Suggestion: Do a reverse image search on the main product photo.

---

### 0.4 Get your Airtable credentials

You'll need these in n8n.

1. Go to **airtable.com/create/tokens** → click **"Create token"**
2. Name it: `n8n-manipdetect`
3. Scopes → check **"data.records:read"**
4. Access → select your `ManipDetect` base
5. Click **"Create token"** → **copy the key** (you won't see it again)

To find your Base ID:
1. Go to your Airtable base
2. Look at the URL: `https://airtable.com/appXXXXXXXX/...`
3. Copy the `appXXXXXXXX` part — that's your **Base ID**

---

## STEP 1 — Create your n8n account and workflow

1. Go to **n8n.io** → **"Get started for free"**
2. Create an account (email + password)
3. Once logged in, click **"New Workflow"**
4. Click the title at the top → rename to `ManipDetect`

---

## STEP 2 — Node 1: Webhook

The webhook is the entry point. Lovable will send data here.

1. Click **"Add first step"** → search `Webhook` → select it
2. In the right panel:
   - **HTTP Method** → `POST`
   - **Path** → type `manipdetect`
3. At the top of the panel, switch to **"Test"** mode → click **"Listen for test event"**

> ✅ **Copy the webhook URL** displayed — format:
> `https://your-account.app.n8n.cloud/webhook-test/manipdetect`
> This is what you'll paste into Lovable. The version without `-test`
> will be the production URL once the workflow is activated.

---

## STEP 3 — Node 2: IF (detect text or url)

1. Click **"+"** to the right of Webhook → search `IF` → select it
2. In the panel, set up the condition:
   - **Value 1** → click the `{}` icon (expression) → type:
     `{{ $json.input_type }}`
   - **Operation** → `equals`
   - **Value 2** → type: `url`

This creates two outputs:
- **TRUE** → it's a URL, we need to scrape it
- **FALSE** → it's plain text, we pass it straight through

---

## STEP 4 — Node 3: Jina Scrape (URL branch only)

On the **TRUE** branch of the IF node:

1. Click **"+"** → search `HTTP Request` → select it
2. In the panel:
   - **Method** → `GET`
   - **URL** → click `{}` → type:
     `https://r.jina.ai/{{ $('Webhook').first().json.content }}`
   - Scroll down to **Headers** → click **"Add Header"**:
     - **Name**: `Accept`
     - **Value**: `text/plain`
3. Rename this node: click its title → type `Jina Scrape`

> 💡 Jina Reader is a free service that converts any URL into clean
> Markdown text, handling JavaScript-rendered content automatically.
> No account required.

---

## STEP 5 — Node 4: Merge

We need to reunite both branches (plain text + scraped URL)
before the rest of the workflow.

1. From the **FALSE** branch of the IF → click **"+"** → search `Merge` → select it
2. Also connect the **output of Jina Scrape** to this same Merge node
   (drag the wire from Jina Scrape's output to the Merge node)
3. In the panel:
   - **Mode** → `Append`

---

## STEP 6 — Node 5: Code (normalise the text)

The two branches don't have the same field structure. This node creates
a single `text_to_analyse` variable regardless of the source.

1. Click **"+"** after Merge → search `Code` → select it
2. **Language** → `JavaScript`
3. Paste this code:

```javascript
// Get the input type sent by Lovable
const inputType = $('Webhook').first().json.input_type;

let text;

if (inputType === 'url') {
  // Jina returns the text in .body or directly in the response
  const jinaData = $('Jina Scrape').first().json;
  text = jinaData.body || jinaData.data || JSON.stringify(jinaData);
} else {
  // Plain text sent directly by Lovable
  text = $('Webhook').first().json.content;
}

// Limit to 4000 characters to control LLM cost
const truncatedText = String(text).slice(0, 4000);

return [{ json: { text_to_analyse: truncatedText } }];
```

4. Rename this node: `Prepare Text`

---

## STEP 7 — Node 6: Airtable (fetch the taxonomy)

1. Click **"+"** after `Prepare Text` → search `Airtable` → select it
2. **Action** → `Search Records`
3. Click **"Create new credential"**:
   - **API Key** → paste your Airtable token (copied in step 0.4)
4. In the panel:
   - **Base** → type your Base ID (`appXXXXXXXX`)
   - **Table** → `Manipulation_Taxonomy`
   - Leave all filters empty (you want all 18 rows)
5. Rename: `Fetch Taxonomy`

---

## STEP 8 — Node 7: LLM (the core of the analysis)

### Option A — OpenAI (GPT-4o)

1. Click **"+"** → search `OpenAI` → select it
2. **Action** → `Message a Model`
3. Click **"Create new credential"** → paste your OpenAI API key
4. In the panel:
   - **Model** → `gpt-4o`
   - **System Message** → paste this prompt:

```
You are an expert analyst in rhetoric and the psychology of persuasion.

Your role is to analyse a text or the content of a web page
and identify the manipulation techniques present.

INSTRUCTIONS:
1. Read the submitted text carefully.
2. Identify each passage that matches a technique in the taxonomy.
3. The same technique can appear multiple times.
4. A passage can combine several techniques — flag this.
5. Do NOT diagnose the author of the text — analyse the text only.
6. Assess the overall intensity:
   low (1-2 techniques), moderate (3-5),
   high (6+) or systemic (combined and repeated techniques).
7. If no technique is detected, say so clearly.
8. Be pedagogical, not alarmist.

RETURN ONLY valid JSON in this exact structure:
{
  "score_global": <integer>,
  "intensite": "<low|moderate|high|systemic>",
  "techniques": [
    {
      "nom": "<exact technique name from taxonomy>",
      "extrait": "<exact passage from the text>",
      "explication": "<why this is this technique>",
      "suggestion": "<how to respond or position yourself>"
    }
  ],
  "synthese": "<2-3 sentences summarising the overall rhetorical profile>"
}

DO NOT add any text before or after the JSON.
DO NOT use markdown formatting.
Return raw JSON only.
```

   - **User Message** → click `{}` → type:

```
Here is the taxonomy of manipulation techniques:

{{ $('Fetch Taxonomy').all().map(r => '- ' + r.json.fields.Name + ': ' + r.json.fields.Definition + ' | Signals: ' + r.json.fields.Signals).join('\n') }}

Here is the text to analyse:

{{ $('Prepare Text').first().json.text_to_analyse }}
```

5. Rename: `LLM Analysis`

---

### Option B — Claude (Anthropic) via HTTP Request

If you prefer to use Claude:

1. Click **"+"** → search `HTTP Request` → select it
2. In the panel:
   - **Method** → `POST`
   - **URL** → `https://api.anthropic.com/v1/messages`
   - **Headers** → add these 3 headers:
     - `x-api-key` : your Anthropic API key
     - `anthropic-version` : `2023-06-01`
     - `Content-Type` : `application/json`
   - **Body** → `JSON` → paste:

```json
{
  "model": "claude-sonnet-4-5",
  "max_tokens": 2000,
  "system": "PASTE THE SAME SYSTEM PROMPT AS ABOVE HERE",
  "messages": [
    {
      "role": "user",
      "content": "Taxonomy:\n{{ $('Fetch Taxonomy').all().map(r => '- ' + r.json.fields.Name + ': ' + r.json.fields.Definition).join('\\n') }}\n\nText to analyse:\n{{ $('Prepare Text').first().json.text_to_analyse }}"
    }
  ]
}
```

3. Rename: `LLM Analysis (Claude)`

---

## STEP 9 — Node 8: Respond to Webhook

This node sends the JSON result back to Lovable.

1. Click **"+"** → search `Respond to Webhook` → select it
2. In the panel:
   - **Respond With** → `JSON`
   - **Response Body** → click `{}` → type:

**If using OpenAI:**
```
{{ JSON.parse($('LLM Analysis').first().json.choices[0].message.content) }}
```

**If using Claude:**
```
{{ JSON.parse($('LLM Analysis (Claude)').first().json.content[0].text) }}
```

3. **Response Code** → `200`
4. **Headers** → add:
   - `Access-Control-Allow-Origin` : `*`
   *(required so Lovable can call the webhook without a CORS error)*

---

## STEP 10 — Test the workflow

### Quick test with plain text

1. In n8n, make sure the **Webhook** node is in **"Test"** mode
   and click **"Listen for test event"**
2. Open **Postman** or **Hoppscotch** (free online tool)
3. Send this POST to your webhook URL:

```
URL: https://your-account.app.n8n.cloud/webhook-test/manipdetect
Method: POST
Headers: Content-Type: application/json
Body:
{
  "input_type": "text",
  "content": "Only 3 spots left. Offer valid until tonight only. After everything I have invested to support you, I hope you won't let me down. Everyone in our community has already taken the leap."
}
```

4. In n8n, click **"Execute step by step"** and watch
   each node execute in order
5. Verify that the last node returns valid JSON with
   `score_global`, `intensite`, `techniques`, `synthese`

### Test with a URL

```json
{
  "input_type": "url",
  "content": "https://coachings.richissime.net/lm/roadmap-personnalisee/"
}
```

---

## STEP 11 — Connect n8n to Lovable

Once the workflow is tested and working:

1. In n8n, click the **"Activate"** toggle at the top right
   (switches the workflow from test mode to production)
2. The webhook URL changes from:
   `https://...webhook-test/manipdetect`
   to:
   `https://...webhook/manipdetect`
3. In Lovable, send this prompt:

```
Replace the value of WEBHOOK_URL (or PLACEHOLDER_N8N_WEBHOOK_URL)
with this URL:
https://YOUR-ACCOUNT.app.n8n.cloud/webhook/manipdetect

Also make sure the fetch call uses these headers:
{
  "Content-Type": "application/json"
}

And sends this body structure:
{
  "input_type": "text" or "url",
  "content": the text or URL string
}
```

---

## Troubleshooting — common errors

| Error | Likely cause | Solution |
|---|---|---|
| `CORS error` in Lovable | Missing CORS header | Add `Access-Control-Allow-Origin: *` in Respond to Webhook |
| `Cannot read property of undefined` in Code node | Jina returned no body | Add a fallback: `text = text \|\| 'Content could not be extracted'` |
| Invalid JSON in Respond to Webhook | LLM added text around the JSON | Add a Code node before Respond that cleans: `text.replace(/```json\|```/g, '').trim()` |
| Airtable returns 0 records | Wrong Base ID or table name | Verify exact spelling of `Manipulation_Taxonomy` and Base ID `appXXX` |
| Timeout after 30 seconds | LLM too slow on large text | Reduce character limit from 4000 to 2000 in the Code node |
| `401 Unauthorized` Airtable | Expired token or wrong scope | Recreate the token with `data.records:read` scope |

---

## Summary of the 8 nodes

| # | Node | Type | Role |
|---|---|---|---|
| 1 | Webhook | Trigger | Receives the request from Lovable |
| 2 | IF | Logic | Detects text vs url |
| 3 | Jina Scrape | HTTP Request | Scrapes the page if url |
| 4 | Merge | Merge | Reunites both branches |
| 5 | Prepare Text | Code | Normalises into a single variable |
| 6 | Fetch Taxonomy | Airtable | Loads the 18 techniques |
| 7 | LLM Analysis | OpenAI / HTTP | Analyses and returns the JSON |
| 8 | Respond to Webhook | Response | Sends the report back to Lovable |

---

## Recommended test URLs

Start with these — they are known for aggressive copywriting:

| Site | URL | Expected techniques |
|---|---|---|
| Richissime | https://coachings.richissime.net/lm/roadmap-personnalisee/ | Artificial scarcity, social proof, price anchoring |
| DecisionIA | https://decisionia.com/bootcamp-consultant-ia-430449/ | False urgency, instrumental flattery |
| Mastery IA | https://www.mastery-ia-pro.fr/sommet?el=wc&utm_source=wc | Scarcity, false consensus, price anchoring |
| BryanRGT | https://www.bryanrgt-coaching.com/webinaire-fb-3 | False urgency, guilt-tripping, social proof |
| Equity Mastermind | https://www.equitymastermind.fr/ | Price anchoring, scarcity, fake badges |

---

*ManipDetect — Le Wagon AI Product Builder — June 2026*
*Caroline Tith — caroline.tith@hotmail.com*
