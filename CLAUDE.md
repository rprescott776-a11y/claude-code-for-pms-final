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
