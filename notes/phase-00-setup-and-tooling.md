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

## 00.02 · Git & Collaboration
*2026-10-13 · quiz 3/3*

**In my words:** Git tracks the changes made to a project as a history of snapshots (commits) that I
can go back to, so I can experiment without fear: try something on a branch, keep it if it works,
switch back if it doesn't. Git itself runs locally on aibox; pushing to a remote (GitHub) is what gives
me a backup and lets other people pull my code. For AI, that means every experiment is tracked, and
huge model checkpoints are kept out with `.gitignore`.

**How it works:**
- I edit a file in the **working directory** → `git add` puts it in the **staging area** (the next
  snapshot) → `git commit -m "message"` locks it into the **local repo** → `git push` sends the
  commits to the **remote** (GitHub).
- Commit protects me from my own mistakes; push protects me from the machine dying.
- `git status --short` shows only what differs from the last commit: `??` untracked, `A` added,
  `M` modified. Left column = staged, right column = working files (` M` changed, `M ` staged).
- A **branch** is a label pointing at a commit. `git checkout -b <name>` makes one and switches to it.
  Checking out a branch rewrites my files to match its commit; `HEAD` = where I am now.
- `git merge <branch>`: if `main` hadn't moved, it's a **fast-forward** (the label just slides forward,
  no new commit). Only when both branches have new commits does git create a merge commit.
- My remotes: `origin` = my fork (push here), `upstream` = the course (fetch updates from here).

**Gotchas:**
- Staged ≠ committed. `git add` doesn't write history, and `git push` only sends commits, so I need to
  commit before pushing.
- Untracked files still show up in `git status` (as `??`). That's why `git add .` can sweep up a huge
  file by accident.
- Deleting a big file in a later commit doesn't shrink the repo: the old snapshot still holds it.
  Getting it out means rewriting history (and GitHub rejects files over 100 MB anyway). Prevent it
  with `.gitignore` (this repo already ignores `*.pt`, `*.pth`, `*.safetensors`).
- A fast-forward merge makes no new commit.

**Key terms:** repository — a folder git tracks plus its full history (in `.git`); commit — a
snapshot of the whole project with a message and an ID; staging area — where I choose what goes in
the next commit; branch — a movable label on a commit; HEAD — the commit/branch I'm on now; merge —
bring one branch's changes into another; fast-forward — a merge that just moves the label; remote — a
copy of the repo somewhere else (GitHub); `.gitignore` — name patterns git ignores.

**Connects to:** 00.01 (the `.gitignore` habit pairs with keeping the environment clean). Every later
lesson: commit `my-work/` after each one. Merging course updates uses `git fetch upstream && git merge
upstream/main`, which makes a real merge commit.
