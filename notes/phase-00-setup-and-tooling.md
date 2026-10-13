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

## 00.03 · GPU Setup & Cloud
*2026-10-13 · quiz 2/3 (fp16 rule in the Review queue)*

**In my words:** A GPU has thousands of simple cores that do the same kind of maths in parallel, while
a CPU has a few (~20 on aibox) powerful ones. AI is mostly matrix multiplication, and every cell of the
result is its own dot product that doesn't depend on any other cell, so the work splits perfectly
across a GPU's cores. That's why training goes from hours to minutes.

**How it works:**
- Matrix multiply: each result cell = (row of A) · (column of B). 5000×5000 → 25 million independent
  dot products of 5000 pairs each ("embarrassingly parallel").
- Benchmark: make two random 5000×5000 matrices → time CPU `a @ b` → copy to GPU with `.to("cuda")`
  *before* the clock → **warm up** → time several runs and **average** → `torch.cuda.synchronize()`
  → speedup = CPU time ÷ GPU time.
- On aibox (fp32): CPU 0.31 s, GPU 0.015 s → **~20×**. The first, unfair run said 2×.
- Pick the device in real code: `device = torch.device("cuda" if torch.cuda.is_available() else "cpu")`.
- fp16 rule of thumb: $\text{max parameters} \approx \dfrac{\text{memory (bytes)}}{2 \text{ bytes/param}}$,
  or check a model: $\text{params} \times 2 \text{ bytes} = \text{memory needed}$.

**Gotchas:**
- fp16 sum: **divide** memory by 2 (24 GB → 12B, 16 GB → 8B). More bytes per param = fewer params,
  so the answer is always smaller than the GB number. The unit becomes parameters, not GB.
- The rule is a ceiling, not a fit: a model that needs *all* the memory won't really run (no room for
  working memory). Training needs several times more than loading.
- First run of anything is a bad benchmark: the first GPU multiply includes one-time setup (loading
  cuBLAS). Warm up first.
- GPU calls are **asynchronous**: Python queues the job and moves on. Without `synchronize()` you time
  the queueing (0.0005 s), not the work (0.015 s).
- A GPU has *thousands* of cores, not millions (25 million was the number of result cells).
- The CPU is fast too (0.3 s, not 10 s): PyTorch uses an optimised maths library on all cores.
- aibox has **unified memory**: no separate VRAM, so `nvidia-smi` says "Not Supported" for memory.
  PyTorch says 130.6 GB total (= 121.6 GiB), but only ~40 GB was **free**. Use free, and check
  `nvidia-smi` for big processes (`llama-server` ~19.7 GB) before big runs.
- GB vs GiB: GB = 10⁹ bytes, GiB = 2³⁰ bytes (130.6 GB ≈ 121.6 GiB).

**Key terms:** CPU — the main processor, few powerful cores; GPU — thousands of simple parallel cores;
CUDA — NVIDIA's platform for running code on the GPU; VRAM — the GPU's own memory, limits model size;
unified memory — CPU and GPU share one pool (aibox); dot product — multiply pairs and add them up;
asynchronous — Python doesn't wait for the GPU to finish; warm-up — an untimed first run that absorbs
setup; fp16 — 16-bit numbers, 2 bytes each; parameter — one learned number in a model; Tensor Core —
GPU hardware specialised for matrix maths (shines at fp16); compute capability — NVIDIA's chip
feature version (GB10 = 12.1, Blackwell).

**Connects to:** 00.01 (CUDA 13 driver ↔ `+cu130` PyTorch). Phase 1 (matrices, dot products). Phase 3
(training memory: gradients, optimizer state). Phase 11 (quantization: fewer bytes per parameter).

## 00.04 · APIs & Keys
*2026-10-13 · quiz 3/3*

**In my words:** An API is a formal, agreed way for my code to talk to a server. For AI models that's
an HTTP request to an endpoint, with my API key in a header and a JSON body saying what I want; a JSON
response comes back. The key identifies and authorizes me and ties usage (billing) to my account, so
it's a password and a credit card in one: anyone who has it can spend my money. Keys never go in
code; they live in environment variables or a `.env` file that git ignores.

**How it works:**
- Four parts of every call: **endpoint** (URL), **API key** (header), **request body** (JSON),
  **response body** (JSON).
- Raw HTTP: build the URL (`/v1/messages`) → headers (`Content-Type: application/json`,
  **`x-api-key`**, `anthropic-version`) → body dict (`model`, `max_tokens`, `messages`) →
  `json.dumps(...).encode()` → POST → status 200 + JSON text → `json.loads(...)` → dict →
  `result["content"][0]["text"]` (`content` is a list of blocks).
- The response also has `stop_reason` (`end_turn` = finished, `max_tokens` = cut off) and `usage`
  (input/output tokens = what you pay for).
- The SDK (`anthropic.Anthropic()`) does all of that for me: headers, reading `ANTHROPIC_API_KEY` from
  the environment, the URL, JSON both ways, a typed `Message` object (`response.content[0].text`),
  named Python errors, and automatic retries.
- Keys: `export NAME=value` reaches every program started from that shell (until it closes); `.env`
  files persist (`uv run --env-file .env ...`). This repo's `.gitignore` already ignores `.env`.
- On aibox: the local Gemma server (`http://127.0.0.1:8095`, model `gemma-26b-chat`) speaks the
  Anthropic format, so I can practise for free with `base_url=` set to that address.

**Gotchas:**
- Leaking a key: deleting it later doesn't help (it's in git history, and bots scrape public GitHub
  within minutes). **Revoke** it and make a new one.
- The key authorizes, it doesn't encrypt. Privacy on the wire comes from `https://`.
- The server sends JSON *text*, not a Python dict; my code turns it into a dict with `json.loads`.
- Checking the key is the server's job: the local server ignores it, Anthropic's checks every call.
- Status codes: **4xx = my fault** (fix the request), **5xx = the server's fault** (wait and retry).
  400 bad request; **401 bad/missing key** (retrying never helps: fix the key); 404 wrong path;
  **429 rate limit** (the 4xx where waiting + retrying *does* help, with exponential backoff);
  500/529 server error/overloaded.

**Key terms:** API — an agreed interface for programs to talk to each other; HTTP — the request/response
protocol of the web; endpoint — the URL you send requests to; API key — a secret string that
identifies and authorizes your account; JSON — text format for data (maps to a Python dict);
environment variable — a named value a shell passes to programs; `.env` — a file of `NAME=value`
secrets, never committed; SDK — the provider's library that wraps the raw HTTP; token — a chunk of text
(~¾ word), the billing unit; status code — a number saying how a request went; rate limit — max
requests per time window (429); exponential backoff — wait 1 s, 2 s, 4 s... between retries;
revoke — cancel a key so it stops working.

**Connects to:** 00.02 (git history keeps secrets forever; `.gitignore` for `.env`). Phase 11 (calling
LLM APIs for real), Phases 13–16 (agents calling APIs in loops; tool-call blocks in `content`).
