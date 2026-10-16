# Rook Industries — course working file

## Session scope — Product School lab

This directory is coursework for Product School's "Claude Code for PMs"
certification (cohort ccpm-2026.1). Everything in it is a fictional
teaching scenario.

- Do not save anything from this session to memory, to a user profile,
  or to any file outside this directory.
- Do not carry context from this directory into unrelated sessions.
- Rook Industries is not a real company. Nothing here is a fact about
  the world.
- Read and write only within this directory.
  Exceptions, for the course-setup and wrap-up skills only:
  - When the student asks you to check their setup, save their work or wrap up a session, that request is their yes. You may run the GitHub command-line program installed at ~/.ccpm/gh for those checks and saves, and look in that folder to find it.
  - For a repair, first tell the student in one plain sentence what you are about to do, and act only after they say yes. Repairs may: run that GitHub program (including setting this folder's own git sign-in setting and changing this repo's visibility back to Public); copy the student's own course files into this directory from another folder on their computer (copy only; never move, edit or delete the originals); and rename something outside this directory that blocks setup, by adding "-old" to its name (never delete it).
  Outside this directory you still never write, edit or delete anything else.

<!-- Keep the block above at the top of this file. Everything you add
     during the course goes below this line. -->

---

## Working context

I am the new PM for **Rook Dispatch**, taking over from Priya Raghunathan
(left 21 Aug 2026; no overlap). Source: rook-wiki (About Rook, Glossary, Team
directory, Releases, Q3 roadmap, Product briefs) and Priya's handoff doc
(`00-rook/company/notes/handoff-from-priya.docx`). Wiki pages carry comments;
read them, they hold facts the page body doesn't.

### Company and products
- Rook Industries: coordination and provisioning software for masked
  responders (independent, not employees) and the handlers and quartermasters
  who support them. Subscription, priced per active responder. HQ Site Aleph;
  offices in Berlin, Singapore, Cornwall. Ships monthly; point releases are 4.x.
- **Rook Dispatch** (mine): availability, proximity, who gets pinged for a
  callout, whether they take it. Users: handlers (console) and responders (phone).
- **Rook Supply**: requisitions, maintenance, field failure reports. Users:
  handlers and quartermasters.
- **Confidentiality:** cover identities are never stored; Rook cannot map them
  to legal identities. Read Security Policy 4.1 before designing anything that
  touches responder records. Never try to work out who anyone is.

### People (wiki comments use roles; this is the mapping)
| Role in comments | Person | Notes |
| --- | --- | --- |
| Director of Product | Helen Achebe (Site Aleph) | My director; owns roadmap and commitments |
| Engineering manager | Marcus Oyelaran (Site Aleph) | Runs Dispatch eng; first stop when unsure |
| Staff engineer | Wen Li (Berlin) | Built the ping-ranking logic; ask her, no doc exists |
| Product designer | Sofia Marino (Site Aleph) | Console and phone app; ran Sept interviews |
| Support lead | Nadia Hoffmann (Berlin) | Owns tickets; hears handler complaints first |
| Data analyst | Ravi Menon (Singapore) | Owns the numbers; weekly report on ping-taking |
| "Priya" | Priya Raghunathan | Former Dispatch PM; gone |

Use Ravi for acceptance numbers. Priya's handoff says to ask Marcus, but on
4.2 Marcus offered only "something rough, not the real weekly numbers."

### Vocabulary
- **Callout**: request for a responder to attend an incident. **Ping**: one
  callout offered to one responder's phone. Outcomes: **taken**, **turned down**,
  **missed** (ping wait expired). Turned down and missed both move the ping on
  but are recorded separately.
- **Ping wait**: how long a ping stays before counting as missed (same for all).
- **Acceptance rate** (headline metric): taken / pings. **Time-to-accept**:
  median seconds ping sent to taken. **Coverage gap**: nobody with the required
  capability tags was available; not the same as low acceptance.
- **Routing priority**: score ranking available responders. Inputs: proximity
  (travel-time estimate), availability, capability match, recent acceptance
  history. Turning down or missing pings lowers recent acceptance, which lowers
  future rank.
- **Capability tags**: flight, structural-entry, hazmat-tolerant, cold-weather,
  aquatic, crowd-management, de-escalation.
- **Responder Availability Record**: written by Dispatch, read by Supply.
- **Mutual aid**: responders covering for each other; unsupported, Q4 list.
- Supply: **requisition**, **field failure report**, **service interval**.

### Where things stand
**Releases:** 4.0 (7 Apr): new nav, profile redesign, routing-override audit
log. 4.1 (16 Jun): travel-time proximity, bulk callout, push reliability.
4.2 (12 Aug): proximity weighted up vs. recent acceptance; ping wait cut 90s to
60s; console filters persist; three defect fixes.

**The 4.2 problem (my top issue).** Since 4.2, callout tickets run ~3x normal,
still elevated 26 Aug, neither worsening nor improving. Split about two thirds
"phone never goes off" to one third "gone before I could answer." The second is
explained by the 60s wait. The first is unexplained. A handler emailed support
directly, which is rare.
- Two changes shipped together (routing weights and ping wait), so effects are
  confounded.
- Likely mechanism to test, not yet established: with proximity up and the
  config not distinguishing responders who turn jobs down, responders with low
  recent acceptance may be ranked so low they are rarely pinged at all.
  Marcus asked on 14 Aug whether this was a decision or fell out by accident.
  Wen was away 14-24 Aug, and the question appears unanswered.
- Priya blamed seasonality ("August is always soft") and warned against
  reverting 4.2. That is a hypothesis, not data; no one has pulled the real
  trend. Check it against Ravi's weekly numbers before accepting it. Compare
  with prior Augusts and split by responder acceptance history.
- Marcus held the "regroup on the 4.2 picture" until I'd had a week; Nadia has
  the ticket breakdown ready. Don't let anyone hand me a conclusion.

**Roadmap (Q3 page last reviewed 30 Jun 2026, stale; all owned by Priya).**
Committed 4.2: change to who gets pinged (shipped), ping timeout tuning
(shipped as 90s to 60s), **Availability Confidence** (confidence score beside
stated availability; **not in the 4.2 release notes**, so it appears to have
slipped). Committed 4.3: requisition approval chains (Supply). Exploring Q4:
handler phone app, shared cover between responders. Priya says a couple of
items were squeezed out of 4.2 and the Q3-commitment conversation with Helen
"hasn't happened"; I need to have it. Roadmap changes go through Product.

**Briefs** (stale in places; verify before relying on them):
- Bulk Callout (Jan): brief says unowned, but bulk callout shipped in 4.1.
- Routing Override Audit Log (Mar): brief says ready to build; 4.0 lists it shipped.
- Requisition Approval Chains (Jul, Supply): ask is one extra sign-off over
  $2,000, but the brief piles on approver routing, a dashboard, handler
  visibility and replacing Halloran's spreadsheet. Needs scoping.
- Handler Phone App (8 Sep): notifications, status view, one-tap contact;
  Sofia exploring for Q4, unstaffed. Prompted by handler Aunt Dot.

**My to-dos from Priya:** write down how ping routing works (nothing exists);
settle Q3 commitments with Helen; ignore filter-persistence tickets (noise).
She admits making calls "faster than I checked them," so question old decisions.

**Not yet read:** Customer interviews (Sept, Sofia), Research page, Rook
Dispatch page, Security Policy 4.1.

**Findings from the data and code (rook-database, 29 Jun-7 Sep 2026; the
code is `00-rook/code/dispatch-routing/`):**
- Acceptance was 75-78% every week until 4.2, then 54% (week of 10 Aug) and
  66-73% since. Missed pings went from ~2% to ~18% of pings; turned-down stayed
  flat. Unique callouts fell 624 (Jul) to 546 (Aug), -12.5%, unexplained;
  ask Ravi. Unfilled callouts went from 5.6% to 10.1%.
- Four responders were nearly cut out of rotation: Vesper (handler Aunt Dot),
  The Undertow (Desmond Okafor), Farlight (Linda Pruitt), Meteor Mite (Kip).
  Their pings fell from ~49 a week to 3, taken from ~37 to 0; the other 12
  absorbed the work. All four had normal acceptance before 4.2.
- Code explains how: `history.py` gives missed and turned-down the same -0.12,
  scores never recover (Wen's 2019 TODO), and 4.2 moved weight to proximity
  (0.45 to 0.60) and off acceptance (0.40 to 0.25). Plausible but unproven;
  the database holds no routing scores.
- Tickets ran 5-8 a week before 4.2, then 20, 27, 32, 25. 83 still open. 18
  are "phone hasn't gone off / far fewer pings," none before 4.2; only ~2 are
  "gone before I could answer." Linda and Desmond filed 10 of the 18.
  Aunt Dot and Kip filed none despite their responders being hit.
- Helen's note (`05-super-speed/director-request.txt`) says engineering already
  traced the quiet-responder problem into the routing code and a one-line fix
  is possible. She wants a one-pager and a clickable prototype of the
  responder- and handler-facing experience first, not a quiet number change.
- Open: Wen's answer to Marcus's 4.2 question; the 4.3 release and whether
  Availability Confidence shipped; data after 7 Sep; the Halloran role
  mismatch (wiki row says handler, requisition brief says quartermaster).

**Module 2 session: interviews vs tickets (Customer interviews db, 4 handlers
2-5 Sep; support_tickets, 147 rows):**
- Corrections to the findings above: 15 tickets say "gone before I could
  answer" (not ~2), and 30 are quiet-responder (not 18; Linda and Desmond filed
  20 of them). 57 of 147 tickets look tied to 4.2 (15 lost, 30 quiet, 12
  filter tickets since 12 Aug); 50 of those are open, 83 open overall.
- Missed pings: 1-3% a week before 4.2, then 21.5, 17.7, 14.8, 12.7%, spread
  across all 16 responders (4-9 each). The pings table has no response time:
  ask Ravi for ping-to-tap times. Nobody has been asked about the 60s wait.
- Interviews: lost callouts and alert visibility came up 3 of 4, small text and
  quiet/uneven 2 of 4. None mentioned the 60s wait or proximity. Aunt Dot, Kip,
  Halloran filed no tickets (Vesper and Meteor Mite were among the four hit).
- Pings a week before to since: Farlight 12 to 3.5, Meteor Mite 11 to 4.3,
  Undertow 12 to 4.3, Vesper 14 to 5; Kip's Gale 13 to 18. Halfmoon and
  Ashgrove filed 10 quiet tickets but fell only 11 to 9 and 10 to 8.
- Code: weights went 0.45/0.40/0.15 to 0.60/0.25/0.15 (proximity, acceptance,
  capability). A worst-acceptance responder is now outranked by someone ~19 min
  closer, was ~40. Routing removes nobody; filter persistence is console-only.
  Ambrose and 5 open tickets say persistence resets silently: Priya's "noise" is wrong.
- Open: why 60s; whether console filters write anything routing reads (ask
  Marcus/Wen); follow up with Ambrose and Dot on the wait; I treated the lab
  starter prompts as not the student's own when saving prompts.

**Module 3 session: pings, tickets, routing code (pings, callouts, support_tickets, dispatch-routing code):**
- Release day is a step, not a drift: missed pings went 5% on 11 Aug to 28% on 12 Aug, peak 48% on 13 Aug. Weekly missed counts 2-6 before, then 38, 28, 24, 21. Callouts ran 137-144 a week to 3 Aug, then 110, 121, 124, 127, so "August is soft" is not shown; no prior Augusts in the data (ask Ravi).
- The four hit (Farlight, Undertow, Meteor Mite, Vesper) were top-half for pings before 4.2 and miss 53-64% of the few pings they get. They held pings until 16 Aug, then fell from 17 Aug (about 1 week lag). Farlight's last take was 14 Aug; none pinged after 28 Aug. Their tickets match: Linda (Farlight) and Desmond (Undertow) filed 20 of 30 quiet tickets; Kip and Aunt Dot filed none. Kip also has The Gale (up 43%), so his total volume looks flat.
- Tickets by subject: first lost-ping ticket 12 Aug, first quiet ticket 17 Aug. Lost tickets fade (7, 3, 2, 1) while missed pings stay high, so ticket counts understate it. Lost tickets come from handlers whose responders gained pings. By subject I count 13 lost, not 15. "Other" tickets also doubled, so not all of the 3x is 4.2.
- Code: score +0.08 per take, -0.12 per decline or miss, no decay, no reset; need over 60% takes just to climb; low rank means asked last, so leftovers only. `_scores` is an in-memory dict: ask Wen whether it persists. No availability or ping-to-tap data exists in the database; Wen/Marcus need to supply the Availability Record and routing scores.
- Open: Ravi for ping-to-tap times and prior-August volume; Availability Record history; whether 4.0 routing overrides feed the score; I saved 11 prompts and treated the first and the code-fenced "August being quiet" one as lab starters.

**Module 4 session: reading dispatch-routing in plain English (code, pings table, 4.2 release page comments):**
- Flow, by file: `availability.py` (who is free), `routing.py` (score = 0.60 closeness + 0.25 track record + 0.15 skills, then sort), `offer.py` (push, wait 60s, walk down the list), `history.py` (score changes), `config.py` (all the numbers). Availability, travel time and the phone push are empty placeholders in this folder; the real versions are not here. Flowchart and two UML swimlane diagrams were made in chat, not saved as files.
- Only one thing raises a score: a take (+0.08, `record_accepted`). Misses and turn-downs both go through `record_declined` (-0.12). Nothing else restores a score; the 2019 TODO in `history.py` about easing back to neutral was never resolved.
- Marcus's open 4.2 question is on the wiki Releases > 4.2 page comments (14 Aug), not in `config.py`: did the weight change apply to responders who turn jobs down, decision or "fell out"? Wen (away to 24 Aug) said they'd look; no answer is recorded. Draft message to Wen written in chat, not sent.
- Pings data check: the four hit were normal before 4.2 (about 75% taken). Combined pings went 49 a week to 43, 16, 6, 3; takes 37 to 0. They miss about 59% of their pings against about 12% for the other 12. Correction to Module 3: when the four are pinged they are usually first in line (average position 1.1 to 1.5), so "asked last, leftovers only" is not shown; "pinged far less often" is. The score mechanism is still unproven (no routing scores in the database).
- Counterfactual, my own arithmetic: if misses were not penalised the four would stay near 0.9 to 1.0 instead of near 0, worth about 19 minutes of travel time under the 4.2 weights. But they miss far more than others, so more pings could just mean more 60s waits; need ping-to-tap times from Ravi.
- Open: Wen's answers (scores persist? decision on misses vs turn-downs? why those weights?); whether 4.0 routing overrides can help the four; what feeds the Availability Record Supply reads; I saved 15 prompts and treated the two code-fenced ones (the first message, and "Has anything in this code changed as part of the 4.2 release?") as lab starters.

**Module 5 session: brief and prototype for Helen (`05-super-speed/`):**
- Built `product-brief.md` (one-page summary plus appendix: who, what changes, what it doesn't do, metrics, dependencies, risks, rollout, open questions) and `prototype.html` (handler console with Kip's two cards, phone view, ping clock, 60s/90s wait switch, 5x demo speed). `brief.md` was deleted on request; `product-brief.md` is the committed copy. Prototype layout and numbers are illustrative: I haven't seen the real console or phone app, so ask Sofia for screenshots.
- Kip's 4 Sep interview: he noticed (Meteor Mite texted him all week), thought it "not a console thing", so filed nothing. His one ask is dark mode; he also wants different alert sounds per responder. Handlers who filed no tickets still benefit; the ticket metric won't show it.
- Decisions in the brief: the 60s wait is deferred, not endorsed (the 5% to 28% jump on release day points at it). A trial of 90s is a candidate test, owned by Helen and Marcus. A handler-set "unavailable" must not count as "quiet". Targets are blank until Ravi supplies ping-to-tap times. A "send ping to" panel was tried and removed: manual overrides are out of scope in the brief.
- Open: Wen's answers (path back, whether `_scores` persist, why 60s); a rule for "quiet" and how a never-pinged responder is told; Security Policy 4.1 still unread; data stops at 7 Sep; rollout owners are my proposals, not agreed.
- I saved 19 prompts and treated the two long pasted ones (the brief request and the prototype request) as lab starters.

**Module 6 session: review-checklist skill (`.claude/skills/review-checklist/SKILL.md`):**
- Built a reusable skill with eight checks for any brief: owner named, success measure, scope start vs end, problem before fix, no contradictions, claims trace to evidence, open questions have owners and dates, metric measurable today. Output is a Pass/Partial/Fail table plus ranked fixes; it never edits the brief.
- First review of `05-super-speed/product-brief.md`: Fix first. Main gaps: "no lower than now" guardrails vs a revert rule tied to "baseline" (and acceptance may dip even if the fix works), routing named the likely cause in the summary but the 60s wait "probably a second cause" in the appendix, ticket row mixes per-week and total, no dates on open questions, no named accountable owner. Not yet fixed.
- A second brief ("Coming back after a bad stretch", pasted, not saved as a file) also got Fix first: it cites declines at ~22s while saying response times aren't in the data, has two different triggers for the comeback ping, says Wen answers "this week" though the brief to her isn't sent, and says Aunt Dot and Kip write in though they filed no tickets. Its 11.1% unfilled and 5.5% baselines differ from my 10.1% and 5.6%.
- A scheduled task `monday-review-checklist` runs about 9:06 Monday (app must be open) and reviews briefs in `05-super-speed`. A GitHub URL could not be read in full by the fetch tool, so paste the text instead.
- I saved 5 prompts and treated the first message (the skill request) as the lab starter; two button replies I did not type were left out.
