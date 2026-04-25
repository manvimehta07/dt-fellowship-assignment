# Daily Reflection Tree — DT Fellowship Assignment

A deterministic end-of-day reflection tool. No LLM at runtime. Same answers → same path → same reflections. Every time.

---

## Repository Structure

```
/tree/
  reflection-tree.json     ← Part A: the full tree as structured data (34 nodes)
  tree-diagram.md          ← Part A: visual Mermaid diagram of all branches

/agent/
  agent.py                 ← Part B: Python CLI agent (loads tree, walks employee through it)

/transcripts/
  persona-1-transcript.md  ← Part B: "Victim / Entitled / Self-centric" persona session
  persona-2-transcript.md  ← Part B: "Victor / Contributing / Altrocentric" persona session

write-up.md                ← Part A: Design rationale (why these questions, branching logic, sources)
README.md                  ← This file
```

---

## Part A — Reading the Tree

Open `tree/reflection-tree.json`. Every node has:

| Field | Purpose |
|---|---|
| `id` | Unique node identifier |
| `parentId` | Parent node (for hierarchy context) |
| `type` | `start / question / decision / reflection / bridge / summary / end` |
| `text` | What the employee sees. `{placeholder}` tokens are interpolated at runtime. |
| `options` | For questions: the fixed choices. For decisions: routing rules. Empty for auto-advance nodes. |
| `target` | Where to go after this node (for bridges, reflections, etc). |
| `signal` | What this node adds to accumulated state. e.g. `"axis1:internal"` |

**Decision node routing rules:**
```
answer=A|D:TARGET_ID              → if last answer started with A or D, go to TARGET_ID
signal_dominant=axis1:internal:TARGET  → if axis1's internal signal count dominates, go to TARGET
```

**To trace a path manually:** Start at `START`, follow `target`. At `question` nodes, pick an option and note its letter. At `decision` nodes, apply the routing rules to find the next id. At `reflection` and `bridge` nodes, follow `target` directly.

**Node counts (all requirements met):**

| Type | Count | Requirement |
|---|---|---|
| question | 15 | ≥ 8 ✓ |
| decision | 6 | ≥ 4 ✓ |
| reflection | 8 | ≥ 4 ✓ |
| bridge | 2 | ≥ 2 ✓ |
| summary | 1 | ≥ 1 ✓ |
| **Total** | **34** | **≥ 25 ✓** |

---

## Part B — Running the Agent

**Requirements:** Python 3.7+. No external dependencies.

```bash
# From the repo root:
python3 agent/agent.py

# With a custom tree file:
python3 agent/agent.py --tree path/to/reflection-tree.json

# Save a transcript of your session:
python3 agent/agent.py --transcript

# Save to a specific path:
python3 agent/agent.py --transcript --transcript-path my-session.md
```

**Keyboard navigation:**
- At question nodes: type the letter of your choice (A/B/C/D) and press Enter
- At reflection/summary nodes: press Enter to continue
- `Ctrl+C` to exit at any time

**What the agent does:**
1. Loads `reflection-tree.json` (not hardcoded — pass any valid tree file)
2. Renders each node in the terminal with colour formatting
3. Waits for input at question nodes; auto-advances at all other types
4. Accumulates signals per axis as the employee answers
5. Routes decision nodes deterministically based on answers and signal tallies
6. Interpolates `{placeholder}` tokens in reflection/summary text with actual answers
7. Optionally saves a Markdown transcript of the full session

---

## The Three Axes

| Axis | Spectrum | Psychology |
|---|---|---|
| **Axis 1 — Locus** | Victim ↔ Victor | Rotter (1954) Locus of Control · Dweck (2006) Growth Mindset |
| **Axis 2 — Orientation** | Entitlement ↔ Contribution | Campbell et al. (2004) · Organ (1988) OCB |
| **Axis 3 — Radius** | Self-Centric ↔ Altrocentric | Maslow (1969) Self-Transcendence · Batson (2011) Perspective-Taking |

---

## Design Principles

1. **No moralizing.** Someone on the "external locus" path gets a reflection that honours that feeling before gently surfacing the question of agency. They don't get told they're wrong.
2. **Signal accumulation over binary branching.** Multiple questions feed into each axis's signal tally, so one misclick doesn't dominate the session.
3. **Interpolation, not generation.** All reflection text is pre-written. `{A1_OPEN.answer}` pulls the employee's own words back into the reflection — not LLM output.
4. **The three axes are a sequence, not three quizzes.** Each bridge acknowledges what just happened and frames what comes next. The closing summary synthesises all three.

---

## Anti-Hallucination Notes (for voice note)

- The tree is **static data** — no model makes any decision at runtime.
- All branching is **lookup-based** — string matching on answer letters and signal counters.
- All reflection text is **pre-written by a human** (me) — the tree cannot generate novel text.
- `{placeholder}` interpolation uses only the employee's own verbatim answers — no inference, no paraphrasing.
- The summary node produces a **template-filled string**, not generated prose.
- I used Claude/LLMs to **research psychology, critique questions, and roleplay personas** — but all text in the tree was evaluated and rewritten by me before inclusion.
