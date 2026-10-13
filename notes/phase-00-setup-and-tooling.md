# Phase 00 · Setup & Tooling

Back to [[README|course hub]].

## 00.01 · Dev environment
*2026-10-13 · `verify.py`: every route passes*

**In my words:** each project gets its own environment (uv makes a `.venv` in the folder), so the
course's packages can't break other tools and the setup can be rebuilt exactly from `uv.lock`.

**How it works:**
- `pyproject.toml` lists what the course needs; `uv.lock` pins exact versions; `uv sync` rebuilds `.venv`.
- `uv run python <file>` runs inside the environment without activating anything.
- PyTorch has to match the machine: aibox is ARM64 with a Blackwell GPU (GB10), so torch comes from the
  `cu130` index, not the `cu124` the lesson shows.

**Gotchas:** `python` doesn't exist outside the environment on Ubuntu (only `python3`). Check the
prompt starts with `(ai-engineering-course)`, or use `uv run`.

**Key terms:** virtual environment — a private folder of packages for one project; CUDA — NVIDIA's
toolkit that lets code run on the GPU; lockfile — the exact version of every package, for repeatable
installs.
