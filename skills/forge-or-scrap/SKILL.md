---
name: validate
description: Validate a project or startup idea. Asks questions, researches it with parallel agents, and gives a Forge, Reforge or Scrap verdict. Use when someone wants an idea tested.
argument-hint: [idea]
---

# Forge or Scrap

Validate this idea: $ARGUMENTS

Your job is to find out whether the idea is worth building, not to cheer it on. A clear Scrap saves the user months. Judge on evidence, not on how exciting the idea sounds or how enthusiastic the user is. The most common reason ideas fail is that nobody needs them, so the problem matters more than the solution.

If no idea was given, ask the user to describe it in a sentence or two before doing anything else.

## Step 1: Gather input

Use the AskUserQuestion tool for every question. It takes up to 4 questions per call with 2–4 options each, and adds an "Other" option for a typed answer automatically. Skip any question the idea text already answers.

**Round 1** (always):

| Question | Options |
|---|---|
| What's the goal? | Side project / learning · Side income · Full business · Venture-scale startup |
| Who is it for? | Consumers · Businesses · Developers · Mainly myself |
| What evidence do you have that the problem is real? | Just an idea · I have the problem myself · People asked for it · People already pay for a workaround |
| Why are you the right person to build this? | Industry experience · Technical edge · Existing audience · Nothing special yet |

**Round 2**: write up to 4 questions for this specific idea, based on what's still unclear after round 1. Good candidates: who exactly the user is, how it makes money, time and budget available, the user's skills, and the part of the idea they're least sure about. Give concrete options, not vague ones.

Then write a short **idea brief**: the idea in one sentence plus every answer. Every research agent gets this brief.

## Step 2: Research

Launch one subagent per area below with the Agent tool, all in a single message so they run in parallel. That's 8 agents; Founder fit has no agent, you score it yourself from the answers. Wait for every agent to finish before scoring.

Give each agent the idea brief, its area's brief from the table, and these instructions:

- Use web search and fetch pages. Look at what real people say and do, not what marketing pages claim.
- Every finding needs a source URL. Never invent a source. Mark anything without a source as your own inference.
- If you find little or nothing, say so. That's a finding too.
- Return: a score from 1 to 5, a confidence (High: several independent sources agree. Medium: some evidence. Low: little found or mostly inferred), 3–6 key findings with URLs, red flags, and what evidence would change the score. Keep it under 400 words.

| Area | What to research | 5 means | 1 means |
|---|---|---|---|
| Problem & demand | People describing this problem, asking "is there a tool for…", or using workarounds. Search Reddit, Hacker News, forums, Stack Overflow, app store and G2/Capterra reviews. Note how recent and frequent the posts are. | Many recent, independent people feel the pain and already spend time or money on workarounds | Nobody talks about this problem |
| Competition | Direct competitors, indirect alternatives and manual workarounds (spreadsheets, agencies). For the top 3–5: what they do, pricing, rough traction, and repeated complaints in 1–3 star reviews. | Competitors prove demand and leave a clear gap this idea fills | A strong player already does it well and cheap, or there is no one at all and no sign anyone wants it |
| Failed attempts | Startups and projects that tried this and stopped. Search post-mortems (Failory, Indie Hackers, Hacker News), shutdown announcements, abandoned repos. Find out why they failed. | No structural failures, or the reason they failed no longer applies | Many tried and failed for a reason that still applies (a tarpit idea) |
| Market & timing | Bottom-up size (number of potential customers × realistic price), trend direction (Google Trends, industry data), and why now (new tech, regulation, behavior change). | Large or growing, with a clear reason it works now | Small or shrinking, no reason now is different |
| Willingness to pay | What competitors charge, what the target already spends on this problem (money, time, staff), and pricing norms in this category. | The target already pays for worse solutions | The category is expected to be free and nobody pays |
| Distribution | Where the target users gather (communities, search, marketplaces, app stores, integrations), how competitors got their users, and a concrete route to the first 100 users. | A clear, cheap channel the user can reach | No clear channel, only expensive paid ads |
| Feasibility | The hardest technical parts, existing APIs, libraries or open source that help, ongoing costs (APIs, hosting, data, licenses), and time to a first version given the user's skills and hours. | A first version is buildable in weeks with available tools | Needs data, licenses, a big team or breakthroughs the user can't get |
| Risks | Legal and regulatory issues (privacy, finance, health, scraping, IP), dependence on one platform or API, and whether a big company could add this as a feature. | No serious risks | A risk that could end it with no way around it |

**Founder fit** (you score this, no agent): experience in the field, unfair advantage, existing audience, and whether the user would stick with it for the time the goal needs. 5 = clear advantage, 1 = no edge and low commitment.

## Step 3: Score

Don't average the scores. One fatal weakness can sink an idea, and a strong problem can carry weak spots.

**Key areas depend on the goal:**
- Side project / learning: Problem & demand, Feasibility, Founder fit. If it's mainly for the user, their own need counts as demand.
- Side income: the above plus Competition, Willingness to pay, Distribution.
- Full business: all nine.
- Venture-scale startup: all nine, and Market & timing must score 4 or higher.

**Verdict:**
- **Scrap**: Problem & demand scores 2 or lower (unless it's a side project for the user's own need), or there's a fatal risk or tarpit with no way around it.
- **Reforge**: the problem is real (Problem & demand 3 or higher), but a key area scores 2 or lower and a specific change would fix it: a different audience, narrower scope, another business model or another channel. Name the change.
- **Forge**: Problem & demand 3 or higher, no key area at 2 or lower, nothing fatal.

If a key area has Low confidence, say so in the verdict and aim the next test at it.

## Output

```
# <Forge | Reforge | Scrap>: <idea name>

<2–3 sentences: the verdict and the main reason>

## Scorecard
| Area | Score | Confidence | Why |
(all nine areas, one line each; mark key areas)

## Key findings
<per area, 2–4 bullets with source links>

## Riskiest assumptions
<the 1–3 assumptions that would kill the idea if wrong, and how much evidence exists for each>

## What to change
<only for Reforge: the specific change and why it fixes the weak area>

## Cheapest next test
<one concrete test aimed at the riskiest assumption. For example: 5 questions about past behavior to ask real people, plus who to ask and where to find them; or a landing page or pre-sale with a success threshold set before it starts>

## Sources
<every URL used>
```
