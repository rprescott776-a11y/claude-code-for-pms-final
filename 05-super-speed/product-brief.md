# Brief: Bringing quiet responders back into rotation

**Product:** Rook Dispatch · **Author:** Dispatch PM · **For:** Helen Achebe (Director of Product) · **Status:** Draft for discussion, not a commitment
**Source:** Helen's request (`director-request.txt`), wiki Glossary, Team directory, 4.2 release comments, Priya's handoff

## One-page summary

**The problem.** Since 4.2 (12 Aug), four responders (Vesper, The Undertow, Farlight, Meteor Mite) went from about 49 pings a week combined to 3, and their takes from about 37 to 0. They were normal before 4.2. Nobody told them or their handlers anything changed. The likely cause is in the routing code, where a missed ping costs the same as a turned-down one and a score never recovers. That is plausible but **unproven**, and the 60s ping wait may also be driving the misses.

**Who it's for.** Quiet responders, the handlers who run them, and Support. That includes handlers like Kip and Aunt Dot who never filed a ticket but are affected.

**What changes for them.**
- A quiet responder gets a path back into rotation, and their phone says in plain words why pings dropped.
- A missed ping is treated differently from a turned-down one.
- The handler console flags a responder whose pings have dropped, with the reason, so handlers find out without having to complain.
- Support can answer "my phone never goes off".

**What it deliberately does not do.** It does not revert 4.2, or change the 60s wait yet (a deferred decision that needs ping-to-tap data). It does not change a number quietly. It does not identify anyone, and it does not build the handler phone app, mutual aid or Availability Confidence.

**How we'd know it worked.** Pings to the four return to their pre-4.2 range, and "phone never goes off" tickets fall. Acceptance, missed pings and unfilled callouts must not get worse. Targets are to be set with Ravi.

**What blocks building.** Wen's answers on the routing code, ping-to-tap times from Ravi, a definition of "quiet", a way to reach a responder who isn't being pinged, and Security Policy 4.1 (not yet read).

**Next step.** A clickable prototype of the responder phone notice and the handler console flag. Nothing goes to engineering until Wen has answered.

---

# Appendix: full detail

## The problem

Since 4.2 (12 Aug), four responders (Vesper, The Undertow, Farlight, Meteor Mite) went from about 49 pings a week combined to 3, and from about 37 takes a week to 0. All four had normal acceptance (about 75%) before 4.2. When they are pinged now they miss about 59% of pings, against about 12% for the other 12 responders. Nobody told them or their handlers anything changed. The ping-routing code gives a missed ping the same penalty as a turned-down one (-0.12), only a take raises a score (+0.08), and nothing ever lets a score recover (Wen's 2019 TODO). 4.2 moved weight from acceptance to proximity (0.40 to 0.25 acceptance, 0.45 to 0.60 proximity). Marcus asked on 14 Aug whether this was a decision or "just fell out that way"; no answer is recorded.

The mechanism is plausible but **not proven**: the database holds no routing scores, and the 60s ping wait (cut from 90s in the same release) may also be driving the misses. A one-line code change could ship today. Helen has asked for the experience first, so this brief describes what changes for people, not which number moves.

## Who it is for

| Who | Why them |
| --- | --- |
| **Quiet responders** (Farlight, Undertow, Meteor Mite, Vesper today; anyone who could land here later) | Their phone "never goes off". They don't know why and can't see a way back. Linda (Farlight) and Desmond (Undertow) filed 20 of the 30 quiet-responder tickets. |
| **Handlers who run a quiet responder** (Kip for Meteor Mite, Aunt Dot for Vesper, and others like them) | They are who actually clicks around the console. Kip and Aunt Dot filed no tickets, but they still benefit: their responders (Meteor Mite and Vesper) were two of the four hit, so they have the same problem as the handlers who did complain. Silence in the tickets is not the same as no impact. They noticed but didn't think it was theirs to raise. Kip (interview, 4 Sep) says Meteor Mite has been texting him all week asking if something is broken. He told them to hang in there, and said "maybe that's not a console thing... I'm not the person to ask". Kip's total pings look flat only because his other responder, The Gale, absorbed the work, which hides the loss on Meteor Mite. The console flag is for them most of all, because it tells them without their having to complain first. |
| **Support (Nadia)** | Holds 83 open tickets and has no answer to give for "my phone never goes off". |

Not the target: responders who are pinged normally, and Supply users.

## What changes for them once it exists

**For a quiet responder (phone):**
- They are pinged again. A responder who has dropped out of rotation gets a path back, rather than a score that only goes up by being asked and saying yes.
- When they are being pinged much less than usual, the phone says so in plain words and says what will bring them back, instead of silence.
- A missed ping is treated differently from a ping they actively turned down. Today both cost the same.

**For a handler like Kip (console):**
- Each responder shows pings this week against their usual, with a plain flag when one has gone quiet ("Meteor Mite: 4 pings this week, usually 11").
- The flag gives the reason in plain language (recent missed and turned-down pings, proximity weighting), so the handler is not guessing.
- The handler sees when a responder is back in rotation, and doesn't have to wait for the responder to complain.
- The handler decides when a responder is available for callouts, and sets it on the responder's card. A responder the handler has set unavailable is **not** flagged as quiet, because no pings are being sent by choice. The flag only applies to responders who are available but not being pinged.
- A handler like Kip or Aunt Dot, who never filed a ticket, finds out their responder had gone quiet from the console, and sees the responder come back. They get the benefit without having had to raise it.

**For Nadia (support):** a concrete answer for the "phone never goes off" tickets, and a way to check a responder's ping activity without asking engineering.

## What it deliberately does not do

- **It does not revert 4.2.** Proximity weighting was a long-standing request from responders who cover wide areas. Priya was right about that part. We are fixing who gets locked out, not the weighting itself.
- **It does not change the 60s ping wait, yet.** This is a deferred decision, not an endorsement of 60s. The wait is probably a second, independent cause: missed pings stepped from 5% to 28% on release day (12 Aug), misses rose across all 16 responders, and 13 to 15 tickets say "gone before I could answer". We defer only because ping-to-tap times don't exist in the data (ask Ravi), nobody has been asked why 60s, and changing it alongside the routing fix would blur which change helped. Revisit as soon as Ravi's data is in. If the four mostly need longer than 60s, the recovery path alone won't bring them back.
  - **Candidate test: put the wait back to 90s for a week or two.** It is cheap, easy to undo, and has a clean baseline (misses were 1 to 3% a week before 4.2). If missed pings fall back toward that, the wait was the main cause. If they don't, routing is. Caveats: it may not help the four, who miss about 59% of their pings against about 12% for the others. A longer wait also holds each callout longer before it moves on, which could raise unfilled callouts (10.1% now). We don't know why 60s was chosen. Ravi's ping-to-tap times should come first, then Wen and Marcus's answer on why 60s. The decision is Helen's and Marcus's.
- **It does not quietly change a number and call it handled.** Any routing change ships with the visibility above, announced to handlers and responders.
- **It does not guarantee a quiet responder more takes.** They get a fair chance to be pinged. If they still miss, that is information about the wait, not a bug.
- **It does not identify anyone.** No mapping of cover identity to legal identity, and nothing that needs one. (Security Policy 4.1 has not been read yet; do this before design starts.)
- **It does not build the handler phone app, mutual aid or shared cover** (all Q4 exploration), **or Availability Confidence** (committed for 4.2, not in the release notes; separate conversation with Helen).
- **It does not add per-responder ping waits or manual overrides.** The 4.0 routing-override audit log may already help; confirm before building anything similar.
- **It does not touch console filter persistence.** That change is console-only, though its tickets are real (Priya's "noise" call was wrong) and need their own owner.

## Success metrics

Baselines come from the database (29 Jun to 7 Sep 2026). **Targets are blank on purpose:** Ravi and I set them together once the ping-to-tap data is in, so we don't invent numbers.

| Metric | Pre-4.2 | Now (latest data) | Target |
| --- | --- | --- | --- |
| **Primary:** pings a week to the four quiet responders (combined) | ~49 | 3 | _TBD with Ravi_ |
| Takes a week by the four (combined) | ~37 | 0 | _TBD_ |
| "Phone never goes off" tickets (quiet-responder), per week | 0 (first on 17 Aug) | 30 in total since 4.2 | _TBD with Nadia_ |
| Handlers who see the quiet flag before filing a ticket | n/a | n/a | _TBD_ (proves the flag reaches Kip and Aunt Dot) |
| **Guardrail:** overall acceptance rate | 75-78% | 66-73% | No lower than now |
| **Guardrail:** missed pings, share of all pings | 1-3% | 12.7% (latest week; peak week 21.5%) | No higher than now |
| **Guardrail:** unfilled callouts | 5.6% | 10.1% | No higher than now |
| Miss rate for the four, against the other 12 | n/a | ~59% against ~12% | Gap narrows. If it doesn't, the cause is the 60s wait, not routing |

Read the guardrails carefully. Bringing the four back raises their pings, and they miss more than most. Acceptance could dip even if the fix works. That is why the miss-rate row sits next to the primary metric.

## Dependencies and constraints

| Dependency | Why it matters | Owner | Status |
| --- | --- | --- | --- |
| **Wen's answer** on 4.2 weights, whether `_scores` persists, and the equal -0.12 penalty | Decides what the fix is, and whether it needs recovery over time or just separating misses from turn-downs | Wen Li (via Marcus) | Open since 14 Aug |
| **Ping-to-tap times** | Separates "ranked too low" from "wait too short" | Ravi Menon | Not requested yet |
| **Security Policy 4.1** | Anything touching responder records (the ping-activity flag, history) must comply. Cover identities are never stored and never mapped to legal identities | Me, then Security | Not yet read |
| **Routing scores and the Availability Record** | The database holds no routing scores, so the mechanism can't be tested. The Record is read by Supply, so changes may reach Supply | Wen / Marcus | Not supplied |
| **4.0 routing-override audit log** | An existing override tool may overlap with what we'd build. Any change to routing should still be auditable | Marcus | Not checked |
| **Console and phone app design** | The flag and notice need new designs | Sofia Marino | Not started |
| **Release slot** | 4.3 already carries requisition approval chains (Supply), and Availability Confidence slipped from 4.2. This work competes with both | Helen Achebe | Q3 commitments conversation hasn't happened |
| **Real code is not all here** | Availability, travel time and the phone push are empty placeholders in `dispatch-routing/`, so the engineering estimate can't come from this folder | Marcus | Open |

## Risks and trade-offs

| Risk | Why it could happen | Mitigation |
| --- | --- | --- |
| Quiet responders come back and still miss most pings, so acceptance and missed pings get worse | The four miss ~59% of the pings they get. The 60s wait may be the real cause, not their score | Get ping-to-tap times first. Roll out to the four before anyone else. Watch the miss-rate row in the success metrics. Be ready to revisit the wait |
| The other 12 responders lose pings the four gain | They absorbed about 49 pings a week of work. They miss only ~12% | Track pings per responder, not only the total. Agree beforehand how much of a shift is acceptable |
| Easing penalties on misses lets a responder who really is unavailable block the front of the queue | A missed ping can mean "away" as well as "too slow" | Recovery is limited and time-bound. A responder who keeps missing after their fresh pings goes back down |
| Flag and reason text expose something about a responder, or imply we know who they are | The flag touches responder records | Read Security Policy 4.1 before design. Show only ping counts and plain reasons. Never show identity |
| We fix the score and the wait turns out to be the cause | Two changes shipped together in 4.2 and are still confounded | Ship the routing fix and any wait change separately, with a gap between them |
| The "quiet" rule is wrong: it flags normal weeks or misses real drops | The threshold has no definition yet | Test the rule on historical data first. Check it flags the four and not Halfmoon or Ashgrove (their pings only fell from 11 to 9 and 10 to 8) |

## Rollout and communication

This section answers Helen's "not a setting we quietly flip". **Not final:** the owners are proposed and not yet agreed.

1. **Before anything ships:** re-pull the data (the analysis stops at 7 Sep). Replay the four's August pings under the proposed rule to see where they'd rank. Helen reviews the clickable prototype. Wen signs off on the routing change. (Owner: me, with Ravi, Wen and Helen.)
2. **Tell the people affected first:** the four responders and their handlers (Linda, Desmond, Aunt Dot, Kip) hear before the change: what happened since 4.2, what is changing, what to expect, and that the 60s wait is still under review. Plain words, not a release note. Nadia's team also sends a reply to the open "phone never goes off" tickets. (Owner: me and Nadia.)
3. **Phase it:** the four go first for a week or two. If the guardrails hold, it goes to all responders. (Owner: Marcus, me.)
4. **Say it publicly:** the change goes in the release notes with its reason. (4.2's notes said "long requested", and didn't mention what it cost responders who turn jobs down.)
5. **Follow up with the handlers, not just the tickets:** a few weeks after release, check in directly with Kip and Aunt Dot. They don't file tickets, so the ticket metric won't show whether this helped them. Ask whether Meteor Mite and Vesper are back to their usual pings, whether the flag wording made sense, and what they told their responders. (Owner: me, Sofia.)
6. **Watch and roll back:** check the success metrics and guardrails weekly with Ravi. If a guardrail drops below its baseline, revert the routing change and tell the same people. (Owner: Marcus, Ravi.)
7. **Tell the other 12:** they absorbed the work. Handlers whose responders will see fewer pings need a heads-up, so it doesn't generate new tickets. (Owner: Nadia.)

## Open questions that gate this

1. **Wen:** was 4.2's weight change meant to apply to responders who turn jobs down? Do scores persist (`_scores` is an in-memory dict), and why were misses and turn-downs given the same penalty? Her answer is the first thing to get.
2. **Ravi:** ping-to-tap times for the four against the other 12, and prior-August volume. This separates "ranked too low" from "wait too short".
3. **Marcus/Wen:** what does the one-line fix touch? Does it need recovery over time, or only separating misses from turn-downs?
4. **Sofia:** is a ping-activity flag a console change, a phone change, or both, and can she mock it by the time Helen wants to click through?
5. **Data after 7 Sep:** have the four recovered on their own?
6. **Ravi and Wen:** was 90s to 60s a deliberate decision, and why 60? With ping-to-tap times, what wait would let most responders answer (for the four and the other 12)? Should the wait be revisited now rather than after the routing fix? Note that the release-day jump in misses is hard to explain by routing scores alone.
7. **Marcus, Wen and Sofia: what counts as "quiet"?** We need a rule and a time window, for example pings below some share of the responder's own recent average, for a set number of weeks. It has to flag the four and not Halfmoon or Ashgrove. It must also leave out any time a responder was set unavailable by their handler, so a handler's own choice never shows up as "quiet". That means the rule needs availability history from the Availability Record, which we don't have yet. Without this the flag can't be built or designed.
8. **Sofia and Marcus: how does a responder who isn't being pinged get told?** The phone's usual channel is the ping itself, and a responder with no pings gets nothing. Options are an in-app banner, a push, or a message via the handler. Pick one.
9. **Wen: what exactly is "a path back"?** Reset to neutral, ease back over time, or a set number of fresh pings? What happens if the responder misses those too? How should a missed ping differ from a turned-down one? This is the core of the fix, and nothing is specified yet.
10. **Wen: do `_scores` persist across a deploy?** If they reset on deploy, the four should already have recovered. They haven't, so either scores are stored somewhere else or the score isn't the cause.

## Next step

A clickable prototype of the two surfaces above (responder phone notice, handler console flag), built by Sofia with me. Nothing goes to engineering until Wen has answered question 1.
