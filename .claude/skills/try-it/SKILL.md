---
name: try-it
description: >
  Hands-on practice for the current lesson: turn its "Build It" code or one of its Exercises into a
  starter file with TODO gaps in my-work/, let Nate write the missing parts himself, then run and check
  his code on aibox. Use for "/try-it", "let me code it", "give me an exercise", "practice this".
---

# Try it: write the code yourself

Reading code feels like understanding; writing it is the real test. This turns the lesson into a small
coding task he does himself. Optional argument: a lesson (`01.03`, a slug, or a topic) and/or
`exercise N`. Default: the lesson most recently logged in `LEARNING.md`, or the one `/learn` just taught.

## Steps

1. **Find the lesson** under `phases/` and read its `docs/en.md` (focus on "Build It" and "Exercises")
   and its `code/` folder.
2. **Pick the task**, and say in one sentence what he'll build:
   - Default: the core from-scratch function(s) from "Build It", the heart of the lesson, not the
     plotting or printing.
   - With `exercise N`: that exercise from the lesson.
   - Size it to **15–40 lines of his code**. If the lesson's code is bigger, pick the central part and
     provide the rest.
3. **Make the starter file** at `my-work/phase-NN/<lesson-slug>/<name>.py` (or `.ts` / `.rs` to match the
   lesson). If the file already exists, ask before replacing it, or make `<name>-2.py`.
   - Keep imports, helper code and a `main` that exercises it. Replace the core logic with
     `# TODO:` comments that say **what** each part must do, not how, plus function signatures and
     docstrings with a tiny input → expected-output example.
   - Add a small self-check at the bottom: asserts or a comparison with the lesson's reference or a
     library (numpy, torch), printing `✓` / `✗` per check so he can see progress.
   - Python standard library + numpy/torch only, as the lesson uses.
4. Tell him the file path and how to run it: `uv run python my-work/...`. Then **stop and let him
   work**. Don't fill in TODOs.
5. **When he says he's done** (or asks you to check): run it. Then:
   - All checks pass: say what he got right, and ask one "why does this work?" question about the core
     line. Point out one idiomatic improvement if there is a clear one, but don't rewrite his code.
   - Something fails: show the failing check's output, then follow `/hint` level 1 (a question, not a
     fix). He fixes it himself.
6. When it passes, suggest `/teach-back` to finish the lesson.
