---
name: review-checklist
description: Reviews a product brief against the PM's eight-point checklist (named owner, success measure, scope consistency, problem before solution, no contradictions, claims trace to evidence, open questions have owners and dates, metric measurable today). Use when the user asks to review, check, or sanity-check a brief, or says "run the review checklist" on a brief or one-pager, before it goes any further.
---

# Review checklist for briefs

Run the same eight checks every time, on whatever brief the user points at. Read the whole brief first, including any appendix, then check each item below. Do not edit the brief unless asked.

## The eight checks

1. **Owner named.** The brief names a specific person or role who owns it (not "the team", not blank). If several owners are listed, say whether it's clear who is accountable.
2. **Success measure.** The brief says how we'll know it worked: a metric, a target or direction, and ideally where the number comes from. A metric with a blank or "TBD" target is a partial pass; say what's missing.
3. **Scope matches.** Compare the scope stated at the start (summary, goal, "what this does") with the scope at the end (appendix, rollout, requirements, "what changes"). Flag anything that appears in one and not the other: items that grew, shrank, or were quietly added or dropped. Also check that "what it doesn't do" is not contradicted elsewhere.
4. **Problem before fix.** The brief explains the problem (who is affected, what happens, evidence) before it proposes a solution. Flag a brief that opens with the solution, or whose problem section is only a restatement of the solution.

5. **No contradictions.** Read the whole brief for statements that conflict with each other, anywhere (not just start vs. end): numbers, dates, names or owners, metrics and targets, what is in or out of scope, rollout steps, and claims in the summary vs. the appendix. Quote both conflicting statements and where each appears.

6. **Claims trace to evidence.** Each number or factual claim has a source (a query, report, document, or named person) or is clearly marked as an assumption. List unsourced claims that the brief's argument depends on.
7. **Open questions have owners and dates.** Every open question or unknown names who will answer it and by when. Flag questions with no owner or no date.
8. **Metric is measurable today.** The success measure can actually be computed from data that exists now. If it needs data nobody collects yet, say what's missing and who would supply it.

## Output format

Start with one line: the brief's name and an overall verdict (Ready to move on / Fix first / Needs rework).

Then a table with one row per check:

| Check | Result | Evidence |
| --- | --- | --- |

- Result is **Pass**, **Partial**, or **Fail**.
- Evidence quotes or points to the section in the brief that supports the result. For a Fail, say what is missing.

After the table, list the specific fixes needed, most important first. Keep it short. If everything passes, say so plainly.

## Rules

- Judge only what the brief says. Don't fill gaps from memory or assume an owner or metric that isn't written down.
- Where a claim in the brief looks stale or contradicts another source the user has given you, note it but don't count it as a checklist failure.
- Do not add checks beyond these eight. If the user wants more, they will say so.
