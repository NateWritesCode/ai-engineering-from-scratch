---
name: recall
description: >
  Spaced review of everything learned so far in the course, a few minutes at the start of a session.
  Asks the cards that are due from notes/recall.json (made from lesson quizzes and teach-back gaps),
  moves each card further out when he gets it right and back to tomorrow when he misses it.
  Use for "/recall", "review", "warm up", "quiz me on old stuff".
---

# Recall: spaced review

Retrieval spread out over time is what makes things stick, so this runs at the start of a session,
before new material. Keep it short: **at most 8 cards, about 5 minutes.** Optional argument: a number
of cards, or a phase number to review only that phase.

## The card file

`notes/recall.json` holds a list of cards:

```json
{
  "cards": [
    {
      "id": "01.01-q3",
      "lesson": "01-math-foundations/01-linear-algebra-intuition",
      "kind": "quiz",
      "question": "…",
      "options": ["…", "…", "…", "…"],
      "correct": 1,
      "explanation": "…",
      "box": 1,
      "due": "2026-10-14",
      "history": [{ "date": "2026-10-13", "right": false }]
    },
    {
      "id": "01.01-tb1",
      "lesson": "01-math-foundations/01-linear-algebra-intuition",
      "kind": "explain",
      "question": "In your own words: why does cosine similarity ignore vector length?",
      "answer": "Key points a good answer covers…",
      "box": 1,
      "due": "2026-10-14",
      "history": []
    }
  ]
}
```

- `kind: quiz` cards come from a lesson's `quiz.json` (added by `/teach-back`). `kind: explain` cards are
  open questions about gaps found in a teach-back. He answers those in his own words.
- Boxes and gaps: box 1 → 1 day, 2 → 3 days, 3 → 7 days, 4 → 14 days, 5 → 30 days, 6 → 90 days.

## Steps

1. Read `notes/recall.json`. If it is missing or has no cards, say there is nothing to review yet (cards
   appear after the first `/teach-back`) and suggest `/learn`. Stop.
2. Pick the cards with `due` on or before today, oldest due first, lowest box first. Cap at 8. If none
   are due, say when the next one is due and offer 3 cards from the lowest boxes as a bonus round.
3. Ask one card at a time:
   - `quiz` cards: lettered options (use the AskUserQuestion tool when available). Never reveal or hint
     at the correct letter before he answers.
   - `explain` cards: he answers in his own words. Judge against `answer`: right means the key idea is
     there, even if worded differently. Partly right counts as a miss, and you say what was missing.
   - After each answer: right or wrong, and a one or two sentence explanation. When wrong, add a tiny
     concrete example if it would help.
4. Update each card: right → `box` + 1 (max 6); wrong → `box` = 1. Set `due` = today + the box's gap.
   Append to `history`. Write the file back, keeping its formatting (2-space indent).
5. Close in two lines: score (N/M), and the topic of any card that has been missed twice or more in a
   row. Suggest re-reading that lesson's section, or `/hint`-style help on it.
