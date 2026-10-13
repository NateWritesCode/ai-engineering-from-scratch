---
tags: [course, ai, learning]
started: 2026-10-13
course: https://aiengineeringfromscratch.com/
repo: https://github.com/NateWritesCode/ai-engineering-from-scratch
---

# AI Engineering From Scratch

My notes for the course: 20 phases, 528 lessons. Python for phases 1–12, then TypeScript and Rust for 13–17.
These notes live in my fork and will move into my Obsidian vault later. The links are Obsidian-style.

- One file per phase: `phase-NN-<slug>.md`, written by `/teach-back`.
- [[questions]]: things to come back to.
- `recall.json`: my spaced-review cards (used by `/recall`).

## The workflow

**One session ≈ one lesson ≈ 45–90 minutes.** Open with `course` on the laptop, then open the Claude
Code panel in VS Code.

| Step | Do | Why |
|---|---|---|
| 1. Warm up (5 min) | `/recall` | Pulling old ideas back out of memory, spaced out over days, is what makes them stick. Cheapest 5 minutes of the session. |
| 2. Learn | `/learn` | The course tutor teaches the next lesson section by section and quizzes me. **Predict before every run**, and answer its questions before reading on. |
| 3. Build it myself | `/try-it` | Type the core code from a TODO starter instead of reading it. Then run the library version and compare. |
| 4. When stuck | `/hint` | Hints one level at a time. Struggle 10 minutes first, ask for the fix last. |
| 5. Explain it back | `/teach-back` | Explain it in my own words, get the gaps corrected, save notes and review cards. |
| 6. Save | commit + push | `/teach-back` offers the commit. Push with VS Code's Sync button. |

## Commands

Type these in the Claude Code panel (in VS Code on aibox).

| Command | Use it when | What happens |
|---|---|---|
| `/recall` | Start of every session | Up to 8 due review cards. Right → the card comes back later; wrong → tomorrow. |
| `/learn` | Learning the next lesson | The course tutor teaches it step by step, quizzes me and logs it in `LEARNING.md`. |
| `/try-it` | After `/learn`, or `/try-it exercise 2` | A TODO starter file in `my-work/` that I complete. Claude runs and checks it. |
| `/hint` | Stuck on code, an error or an idea | One hint per reply: question → idea → approach → fix. `/hint more` for the next one. |
| `/teach-back` | Finishing a lesson | I explain it back. Writes my phase notes, adds review cards, offers a commit. |
| `/check-understanding <N>` | End of phase N | 8-question phase quiz. Below 70% → revisit before moving on. |
| `/course-guide <topic>` | "Where is X taught?" or "I'm stuck on X" | Points at the exact lessons and the next command. |
| `/start-learning` | Once at the start, or to re-plan | Placement quiz and a personal plan in `LEARNING.md`. |
| `/build-project` | Capstone projects (phase 19) | Stage-by-stage project tutor: hints, never solutions. |

On the laptop: `course` opens the course in VS Code (with this page), and `course -t` opens a terminal on aibox.

**Each week:** at least one session that's only `/recall` and the lesson **Exercises** I skipped.
Re-read [[questions]] and clear what I can.

**End of each phase:** `/check-understanding <phase>`. Below 70% → redo the lessons it flags before
moving on. Delete that phase's big datasets and models (aibox's disk is nearly full).

**Rules I keep:**
- Type code, don't paste it. Run everything; change one number and predict what happens.
- If a lesson feels easy, still do the quiz and teach-back, and go faster through the reading.
  `/learn` speeds up if I say "I know this".
- Behind on the plan? Skip new lessons, never `/recall`.
- A whole phase I already know: `/check-understanding <phase>`. Score ≥ 85% → skim it and move on.
- Pace: ~5 lessons a week finishes in about two years. Capstones (phase 19, 85 lessons) are projects,
  so use `/build-project` there.

### Where things are
- Course on **aibox** (NVIDIA GB10): `~/Projects/ai-engineering-from-scratch`. `course` opens it in
  VS Code; `course -t` opens a terminal.
- Run Python with `uv run python <file>`; add packages with `uv add`. Rust, Node 22, bun and Julia are
  installed. Use bun where the course says pnpm.
- My own code goes in `my-work/phase-NN/<lesson>/`.
- Course updates: `git fetch upstream && git merge upstream/main`.

## Progress

| ✓ | Phase | Lessons | Started | Finished | Phase quiz |
|---|---|---|---|---|---|
| ⬜ | 0 · Setup & Tooling | 13 | 2026-10-13 | | |
| ⬜ | 1 · Math Foundations | 22 | | | |
| ⬜ | 2 · ML Fundamentals | 21 | | | |
| ⬜ | 3 · Deep Learning Core | 13 | | | |
| ⬜ | 4 · Computer Vision | 28 | | | |
| ⬜ | 5 · NLP, Foundations to Advanced | 29 | | | |
| ⬜ | 6 · Speech & Audio | 17 | | | |
| ⬜ | 7 · Transformers Deep Dive | 16 | | | |
| ⬜ | 8 · Generative AI | 15 | | | |
| ⬜ | 9 · Reinforcement Learning | 12 | | | |
| ⬜ | 10 · LLMs from Scratch | 24 | | | |
| ⬜ | 11 · LLM Engineering | 17 | | | |
| ⬜ | 12 · Multimodal AI | 25 | | | |
| ⬜ | 13 · Tools & Protocols | 31 | | | |
| ⬜ | 14 · Agent Engineering | 54 | | | |
| ⬜ | 15 · Autonomous Systems | 22 | | | |
| ⬜ | 16 · Multi-Agent & Swarms | 25 | | | |
| ⬜ | 17 · Infrastructure & Production | 29 | | | |
| ⬜ | 18 · Ethics, Safety & Alignment | 30 | | | |
| ⬜ | 19 · Capstone Projects | 85 | | | |

Lesson-by-lesson progress and quiz scores are in `LEARNING.md` (kept by `/learn`).
