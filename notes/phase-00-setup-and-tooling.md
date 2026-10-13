# Phase 00 · Setup & Tooling

Back to [[README|course hub]].

## 00.01 · Dev environment
*2026-10-13 · quiz 3/3 · `verify.py`: every route passes*

**In my words:** A dev environment is four layers: system, package managers, runtimes, libraries. The
system foundation lets our code run on this hardware, package managers get the right libraries (plus
the libraries *they* need, at versions that fit together), runtimes actually run our code, and the
libraries are built by the community so we don't reinvent the wheel. Each layer depends on the one
below, so you install bottom-up, and a broken lower layer often shows up higher up as a *silent*
problem, not an error.

**How it works:**
- Each project gets its own environment (uv makes a `.venv` in the folder), so the course's packages
  can't break other tools. `pyproject.toml` lists what's needed, `uv.lock` pins exact versions, and
  `uv run` syncs `.venv` to the lockfile before running (the "Uninstalled 1 / Installed 1" lines).
- My 5-line check: `import sys, torch` → print the Python version → print the PyTorch version → ask
  `torch.cuda.is_available()` → only if True, print the GPU name (the `if` stops a crash on a
  machine without a GPU).
- On aibox it printed Python 3.12.13, PyTorch `2.14.1+cu130`, CUDA available `True`, GPU `NVIDIA GB10`.
- PyTorch has to match the machine: aibox is ARM64 with a Blackwell GPU (GB10), so torch comes from the
  `cu130` index, not the `cu124` the lesson shows. Plain PyPI only has CPU builds for ARM.
- `verify.py --show-later` is the production version of my check: many tools, "required now" vs
  "later", and the exact fix for each failure.

**Gotchas:**
- "It installed" ≠ "it works". Without a GPU driver PyTorch still installs, then quietly runs on the
  CPU. Check `torch.cuda.is_available()`.
- `+cu130` means the build *supports* CUDA 13.0. Only `is_available() → True` proves it works.
- `python3` means whatever PATH finds first: the system `/usr/bin/python3` (no course packages) or the
  venv's if it's activated. `which python3` tells you. The GPU check goes *through* PyTorch, so system
  Python can't even check the GPU. When the course says `python3 …`, I use `uv run python …`.
- A project missing torch fails at `import torch` (`ModuleNotFoundError`). Fix: `uv add torch` in
  that project (no new venv needed), and on aibox point it at the cu130 index.
- `python` doesn't exist outside the environment on Ubuntu (only `python3`).

**Key terms:** virtual environment — a private folder of packages for one project; runtime — the
program that executes your code; package manager — downloads and installs libraries and their
dependencies; resolving — finding package versions that all fit together; lockfile — the exact version
of every package, for repeatable installs; CUDA — NVIDIA's system for running maths on the GPU's many
cores in parallel; PATH — the ordered list of folders the shell searches to find a command; standard
library — modules that ship with Python (like `sys`).

**Connects to:** everything after it. Next up: 00.02 Git & Collaboration. 00.06 Python Environments
goes deeper on venvs, and 00.03 GPU Setup on the driver/CUDA layer.
