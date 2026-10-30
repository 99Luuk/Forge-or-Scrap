# Forge or Scrap

A Claude Code skill that tells you whether a project or startup idea is worth building. It asks you a few questions, researches the idea on the web and gives one of three verdicts:

- **Forge**: build it.
- **Reforge**: the problem is real, but change one specific thing first.
- **Scrap**: don't build it.

## Install

```bash
claude plugin marketplace add 99Luuk/Forge-or-Scrap
claude plugin install forge-or-scrap@luuk-dekker
```

## Use

```
/forge-or-scrap:validate a tool that turns meeting notes into Jira tickets
```

Leave out the idea and it will ask you for one.

## What happens

1. **Questions.** Two rounds of multiple-choice questions: your goal, who it's for, what evidence you have and why you're the one to build it, then up to four questions specific to your idea.
2. **Research.** Eight agents run in parallel, one per area: problem & demand, competition, failed attempts, market & timing, willingness to pay, distribution, feasibility and risks. Every finding comes with a source link.
3. **Verdict.** Each area gets a score from 1 to 5 with a confidence level. Founder fit is scored from your answers. The scores aren't averaged: which areas count depends on your goal, and a weak problem scraps the idea no matter how strong the rest is.

A run takes several minutes and uses a lot of tokens, since eight agents search the web at once.

## The report

- Verdict and the main reason, in 2–3 sentences
- Scorecard for all nine areas
- Key findings per area, with sources
- The 1–3 assumptions that would kill the idea if wrong
- What to change (Reforge only)
- The cheapest next test to check the riskiest assumption
- All sources

## License

MIT
