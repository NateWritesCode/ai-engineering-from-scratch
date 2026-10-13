---
name: hint
description: >
  Help when stuck on a lesson, exercise or his own code, through a hint ladder, from a nudging question
  up to the fix, one step at a time, so he still does the thinking. Use for "/hint", "I'm stuck",
  "why doesn't this work", "help with this error", "I don't get this part".
---

# Hint: get unstuck without being handed the answer

Being told the answer feels good and teaches little. This gives the **smallest** push that gets him
moving again. Optional argument: what he is stuck on, an error, or a file. Otherwise work out from the
conversation and his recent files in `my-work/` what he's stuck on.

## First, understand the stuck

Read the relevant code, error output or lesson section yourself. Run the code if that helps you see the
problem. Then ask, briefly, **what he expected to happen**, unless he has already said. The gap between
expected and actual is often the whole answer.

## The ladder

Give **one level per reply**, starting at level 1, unless he asks for more (`/hint more`, "next hint",
"just show me"). Say which level you're on, e.g. "Hint 1 of 4".

1. **Question.** A question that points at the right spot: "What shape is `x` after line 12?",
   "What does the loss do on the first few steps?" Suggest one thing to print or check.
2. **Concept.** Name the idea he's missing and where the lesson explains it (file and section). One
   sentence on how it applies here.
3. **Approach.** Describe the fix in words or pseudo-code, without writing his code: "Normalise each
   row before the dot product: divide by its length."
4. **Fix.** Only when he asks for it. Show the smallest change (a few lines, as a diff or a snippet) and
   explain why it works. Then ask him to type it in himself, rather than editing his file for him,
   unless he says to apply it.

## Rules

- Concepts beat syntax. If the bug is a typo or a wrong import, just say so plainly. The ladder is for
  ideas, not trivia.
- If he's stuck on understanding (not code), level 1 is a tiny concrete example he can compute by hand
  or run in 3 lines.
- If the real problem is a missing earlier idea, say which earlier lesson covers it (`/course-guide`
  can find it).
- After he gets unstuck, if the gap was conceptual, offer to add it as an `explain` card in
  `notes/recall.json` (box 1, due tomorrow; see `/recall` for the format).
