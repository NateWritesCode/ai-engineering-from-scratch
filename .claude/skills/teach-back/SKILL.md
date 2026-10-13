---
name: teach-back
description: >
  Finish a lesson: Nate explains it back in his own words, Claude checks the explanation against the
  lesson and fills the gaps, then writes his lesson notes (notes/phase-NN-*.md), adds spaced-review
  cards to notes/recall.json, ticks progress in notes/README.md and offers a commit. Use for
  "/teach-back", "I finished the lesson", "lesson done", "write my notes".
---

# Teach-back: explain it, then save it

If he can explain it simply, he understands it. If he can't, now is the cheapest time to find out.
Optional argument: the lesson (`01.03`, slug or topic). Default: the newest lesson in `LEARNING.md`'s
Progress log, or the one just taught in this conversation. Confirm which lesson in one line.

## 1. Explain it back

Read the lesson's `docs/en.md` (Learning Objectives, The Concept, Key Terms) so you can judge his answers.
Ask these **one at a time**, waiting for each answer:

1. "In 2–4 sentences, as if to a friend who codes but hasn't done this course: what is <core idea>,
   and why does it matter for AI?"
2. "What does the from-scratch code actually do, step by step?" (or, for a no-code lesson, "Walk me
   through <key mechanism>.")
3. One "what if" question that probes real understanding, e.g. "What would happen if we didn't
   normalise here?", drawn from the lesson's content or exercises.

After each answer: say what was right, specifically, then name any **gap or mistake** plainly and fix
it with a short explanation or a tiny example. Don't lecture past the gap. Keep a list of the gaps.

## 2. Write the notes

File: `notes/phase-NN-<phase-slug>.md` (e.g. `notes/phase-01-math-foundations.md`). If it doesn't exist,
create it with:

```markdown
# Phase NN · <Phase title>

Back to [[README|course hub]].

```

Add a new section for the lesson **at the end** of the file (lessons stay in course order). Write it in
**his voice, from his answers** (cleaned up and corrected, not replaced with the textbook):

```markdown
## NN.MM · <Lesson title>
*<YYYY-MM-DD> · quiz <score from LEARNING.md, if any> · code: `my-work/phase-NN/<slug>/` (if he wrote any)*

**In my words:** <2–4 sentences from his answer 1, corrected>

**How it works:** <3–6 bullets from answer 2: the mechanism, maybe one formula in $LaTeX$>

**Gotchas:** <the gaps you found, each phrased as the thing to remember>

**Key terms:** term — one-line definition; term — definition

**Connects to:** <earlier lessons it builds on / later ones it sets up, as plain text>
```

- If he raised a question you couldn't settle, or he wants to come back to something, append it to
  `notes/questions.md` as `- [ ] <question> (NN.MM)`.

## 3. Make review cards

Append to `notes/recall.json` (format in the `/recall` skill; create `{"cards": []}` if missing):

- Every question in the lesson's `quiz.json` as a `quiz` card (id `NN.MM-q<i>`; skip ids that already
  exist). `box` 1, `due` tomorrow. Questions he missed in `/learn` or here are the most important.
- One `explain` card per gap from step 1 (id `NN.MM-tb<i>`), with the key points of a good answer in
  `answer`. `box` 1, `due` tomorrow.

## 4. Progress and save

- In `notes/README.md`, fill in the phase's **Started** date if empty, and tick the phase (✅ plus a
  Finished date) when this was its last lesson. When a phase finishes, suggest
  `/check-understanding <phase>`.
- Show a short summary: notes file, number of cards added, gaps found.
- Offer to commit `notes/` and `my-work/` (plus `LEARNING.md`) as `Lesson NN.MM: <title> (notes + my
  code)`, then remind him to push from VS Code.
