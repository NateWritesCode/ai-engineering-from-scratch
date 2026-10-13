# My AI Engineering Path
<!-- Managed by the ai-engineering-from-scratch learning skills.
     Repo: https://github.com/rohitg00/ai-engineering-from-scratch -->

## Mission
"Start from absolute scratch." Learn AI engineering from the ground up, with no assumed background.
Build goal: "everything", working through the whole curriculum end to end.

## Placement
- Date: 2026-10-13
- Score: self-selected
- Entry point: Phase 0: Setup & Tooling
- Pace: as fast as possible

## Path
| Phase | Name | Status | Est. hours |
|-------|------|--------|------------|
| 0 | Setup & Tooling | Do | 16 |
| 1 | Math Foundations | Do | 23 |
| 2 | ML Fundamentals | Do | 26 |
| 3 | Deep Learning Core | Do | 15 |
| 4 | Computer Vision | Do | 27 |
| 5 | NLP — Foundations to Advanced | Do | 30 |
| 6 | Speech & Audio | Do | 18 |
| 7 | Transformers Deep Dive | Do | 14 |
| 8 | Generative AI | Do | 14 |
| 9 | Reinforcement Learning | Do | 13 |
| 10 | LLMs from Scratch | Do | 26 |
| 11 | LLM Engineering | Do | 19 |
| 12 | Multimodal AI | Do | 65 |
| 13 | Tools & Protocols | Do | 43 |
| 14 | Agent Engineering | Do | 55 |
| 15 | Autonomous Systems | Do | 20 |
| 16 | Multi-Agent & Swarms | Do | 28 |
| 17 | Infrastructure & Production | Do | 34 |
| 18 | Ethics, Safety & Alignment | Do | 31 |
| 19 | Capstone Projects | Do | 620 |
| | **Total (Do)** | | **1,137** |

## Progress log
| Date | Lesson | Quiz | Note |
|------|--------|------|------|
| 2026-10-13 | 00-setup-and-tooling/01-dev-environment | 3/3 | Predicted the GPU would show up from system python3; missed that the GPU check goes through PyTorch. Learned: use `uv run python`, and `python3` may be the venv if it's activated (PATH). |
| 2026-10-13 | 00-setup-and-tooling/02-git-and-collaboration | 3/3 | Missed: push alone doesn't save staged work (needs commit first); thought staged = in history; expected a merge commit on a fast-forward; thought untracked files don't show in `git status`. |
| 2026-10-13 | 00-setup-and-tooling/03-gpu-setup-and-cloud | 2/3 | Missed the fp16 sum (multiplied by 2 instead of dividing: GB ÷ 2 bytes = params) and the GB→param unit. Also: guessed CPU 10 s (was 0.3 s), thought no-synchronize timing would be bigger, missed that the 5000×5000 result cells are all independent (25M). Learned: aibox has unified memory (nvidia-smi says Not Supported), use FREE not total, warm up before timing. |
| 2026-10-13 | 00-setup-and-tooling/04-apis-and-keys | 3/3 | Used aibox's local Gemma server (port 8095, Anthropic-compatible) for raw HTTP + SDK calls; fake key → real 401 from Anthropic. Missed: predicted 5xx for a bad key (4xx = client's fault, 5xx = server's). Warm-up miss: VRAM = Video RAM (said virtual). |

## Review queue
- 00-setup-and-tooling/03-gpu-setup-and-cloud: fp16 rule of thumb (memory ÷ 2 bytes per parameter = number of parameters; answer must be smaller than the GB figure).
