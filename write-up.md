# Write-Up: Design Rationale for the Daily Reflection Tree

**Candidate | DT Fellowship Assignment**

---

## Why These Questions

The hardest constraint in this assignment was the one that mattered most: **no free text**. Fixed options mean I had to pre-imagine the full spectrum of how someone might honestly answer each question — and design options that didn't collapse into "good answer" and "bad answer."

My test for every question was: *would a tired, slightly defensive employee at 7pm click through this to get it over with, or would one of the options actually make them pause?*

That test killed roughly half my first drafts.

**Axis 1 (Locus):** The opening question uses a car metaphor deliberately. "Locus of control" is jargon. "Who was driving today?" is felt. The options (driving / backseat / no wheel / found my lane eventually) capture a real gradient — including the person who started external and recovered internally, which most locus instruments miss. Option D ("I found my lane eventually") routes to the same branch as A ("I was driving") because both signal eventual agency — but the follow-up questions branch differently based on *how* they navigated.

**Axis 2 (Orientation):** The entitlement literature (Campbell et al., 2004) notes that entitled employees don't feel entitled — they feel *justified*. So I couldn't write options that sound like "I felt entitled." Instead I wrote options that sound like reasonable feelings: "I felt I deserved more recognition," "I felt frustrated others weren't pulling their weight." These are things people actually think. Naming them without shame is what makes the follow-up reflection land.

**Axis 3 (Radius):** Maslow's self-transcendence is easy to intellectualise and hard to feel. The question "who was in the frame when you think about today's biggest challenge?" gets at it obliquely — most people don't think about whose frame they're in. The options progress from A (just me) to D (the end user), and I resisted making D sound obviously better. All four are honest answers that real people have.

---

## Branching Design — Trade-offs Made

**Signal accumulation over binary branching.** The spec shows a simple binary tree. I used a signal-tallying approach instead — questions add to `axis1:internal` or `axis1:external` counters, and decision nodes route based on the dominant signal. This handles the person who gives mixed answers (internal on one question, external on the next) more honestly than a pure binary tree would.

**The nuanced reflection node (A1_R_EXT_NUANCED).** Someone who describes their day as "happening to them" but then identifies agency in their tone, priorities, or communication deserves a different reflection than someone who found nothing in their control. Adding this third branch meant one more decision node, but it treats the data the employee gave us with respect.

**Depth vs breadth.** I chose to go deeper within each axis (2 questions minimum before branching to reflection) rather than wider across more axes. The brief says "8+ question nodes" — I have 11. But more importantly, each pair of questions gives the signal accumulator more signal to work with before routing, reducing the chance of a single accidentally-clicked answer dominating the session.

**The closing intention question (A3_SUMMARY_Q).** This is the one node that doesn't route differently based on what came before — everyone gets it. The reason: the three axes converge here. The four options map to: (A) agency intention from Axis 1, (B) contribution intention from Axis 2, (C) perspective-taking from Axis 3, (D) meaning-connection that spans all three. Which option an employee chooses after going through the whole tree is the most honest signal of where they land — and what they interpolate into the summary.

---

## Psychological Sources

| Concept | Source | Application in tree |
|---|---|---|
| Internal/External Locus of Control | Rotter (1954) | Axis 1 branching; internal vs external reflection nodes |
| Growth Mindset | Dweck (2006) | A1_Q_GROWTH — "surprised yourself?" — tests whether employee sees capacity as growing |
| Psychological Entitlement | Campbell et al. (2004) | Axis 2 entitlement options written to be honest, not shameful |
| Organizational Citizenship Behaviour | Organ (1988) | A2_Q_CONTRIB_DEEP and A2_R_CONTRIB explicitly reference discretionary effort |
| Self-Transcendence | Maslow (1969) | Axis 3 radius questions; A3_R_SELF reflection references this explicitly |
| Perspective-Taking | Batson (2011) | A3_R_WIDE reflection; A3_Q_WIDE's "what did that awareness make you do?" |

---

## What I'd Improve With More Time

1. **Axis-to-axis interpolation.** The summary currently references each axis's dominant signal separately. With more nodes, the reflection could reference *cross-axis patterns* — e.g., "You saw your agency clearly (Axis 1) but kept the frame close (Axis 3). That's an interesting combination — agency for yourself, not yet for others."

2. **Time-of-week variation.** A Monday reflection should land differently than a Friday one. The tree could include a date-aware opening branch — not changing the questions, but changing the framing language. "Start of the week" vs "end of the week" shifts what "looking back" means.

3. **Longitudinal signals.** A single session tells you where someone landed today. Across 30 sessions, patterns emerge. The data structure already supports this — every session produces a signal tally. A weekly summary node could be added without changing the core tree.

4. **Validation testing.** I tested the tree by roleplaying two personas (included in `/transcripts/`). I'd want to test with 10+ real employees before trusting the routing logic — specifically the signal-accumulation thresholds for when "dominant" is declared.

---

*Total word count: ~800 words.*
