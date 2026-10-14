# 04 · X-Ray Vision — prompts

**Context:** You joined Rook two weeks ago as PM on Dispatch.
Release 4.2 shipped on 12 August, just before you arrived, and
landed badly. Last session the numbers showed you what happened: a
ping used to wait ninety seconds and now it waits sixty, people
missed pings they used to catch, and missing one counts the same as
turning one down — so four responders stopped hearing from us
altogether.

At the end of the session, type wrap up, and Claude Code saves the
prompts you wrote yourself below — not the starter prompt — updates
CLAUDE.md, and saves your work to GitHub. By Module 6 this file is a
prompt library built from your own questions.

---

### 1.
Create a workflow chart with the steps taken within the code

### 2.
please turn the diagram into a UML process flow with more details

### 3.
How does the code determine who is available?

### 4.
Are there any engineering comments in the codebase worth evaluating further

### 5.
what code changes were introduced in 4.2 that may be contributing towards the issues we've been seeing with more misses and less pings being sent

### 6.
How is the supply connection tied into the availability record?

### 7.
what is the specific comment from Marcus on 14 Aug in the config.py

### 8.
thank you that is what I was looking for. the pings table on rook-database has module 3's numbers. Read that alongside the code. Does it back up what was found initially in this chat? And does that answer the engineering manager's question?

### 9.
What would happen if missed and turned down were not treated the same? Would it allow the quiet responders to receive more pings?

### 10.
You indicated 'The weights apply to everyone the same way, with no special handling for responders who turn jobs down, so they are ranked low automatically.' Can you confirm that?

### 11.
going back to Marcus' question, why would he have asked that question in the first place?

### 12.
draft a message to Wen asking about the 4.2 weights

### 13.
Find me the part of this code that takes points off somebody when they miss a ping or turn one down. Show it to me and explain it in plain English. Then find me every single thing in this code that puts points back on.

### 14.
besides taking  well over half responders are offered for their ranking to improve, what else can they do?

### 15.
If I'd only asked what the code does, not what it doesn't do, what would I have missed?
