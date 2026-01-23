# ide-system-prompt
# AI Prompt Infrastructure

This repository is the **single source of truth** for all AI system prompts used across IDE platforms (e.g. SIMKSPSTK, Pengelolaan Kinerja, Ruang Guru core).

It exists to ensure:

* consistency of language, tone, and terminology
* regulatory and institutional correctness
* controlled evolution of prompts via review

> **Important**: Prompts are **authored and governed here**, but **copied and used from Notion**. Do not copy prompts directly from GitHub unless you are a maintainer.

---

## Repository Structure

```
ai-prompts/
├─ README.md                  # You are here
├─ base/
│  └─ base-content-design.md  # Global rules that apply to ALL systems
├─ design-system/
│  └─ ui-design-system.md     # UI + component + interaction constraints
├─ overlays/
│  ├─ simkspstk.md            # SIMKSPSTK-specific rules & terminology
│  ├─ pengelolaan-kinerja.md  # Pengelolaan Kinerja overlay
│  └─ ruang-guru-core.md      # Shared Ruang Guru platform rules
└─ archive/                   # Deprecated or historical prompts
```

### Folder Responsibilities

#### `/base`

Contains **non-negotiable, system-agnostic rules**, such as:

* language standards
* tone principles
* accessibility rules
* copywriting constraints

Every AI task **must** include the Base Prompt.

---

#### `/design-system`

Defines **UI and interaction constraints**, including:

* allowed components
* layout and styling rules
* accessibility requirements
* technical guardrails (e.g. no raw HTML elements)

Used whenever AI is asked to generate UI or front-end code.

---

#### `/overlays`

System-specific overlays that **extend (not override)** the Base Prompt.

Each overlay may define:

* approved terminology
* institutional names
* system context
* regulatory references

Only **one overlay** should be used per task.

---

#### `/archive`

Old or deprecated prompts kept for traceability.

Prompts in this folder **must not** be used for new work.

---

## Prompt Composition Model

Every AI request follows this order:

1. **Design System Prompt** (if UI or code is involved)
2. **Base Prompt** (always required)
3. **System Overlay** (exactly one)
4. **Task Prompt** (what to build)

Example:

```
[DESIGN SYSTEM]
[BASE]
[SIMKSPSTK OVERLAY]
[TASK]
```

---

## Governance Rules

### Ownership

* This repository is maintained by the **Content Design team**.
* Each folder has designated reviewers.

### Change Process

* All changes require a Pull Request.
* No direct commits to `main`.
* PRs must clearly state:

  * what changed
  * why the change is needed
  * affected systems

### Versioning

* Versions are tracked via Git history and tags.
* Version labels shown in Notion must match the latest approved commit.

---

## What This Repo Is NOT

* ❌ Not a playground for experimentation
* ❌ Not a place for system-specific UI tasks
* ❌ Not a substitute for Notion prompt blocks

If you want to *use* a prompt, go to **Notion**.
If you want to *change* a prompt, contact **Content Design team**.

---

## Relationship with Notion

Notion is the **distribution layer**:

* prompts are copied from approved GitHub commits
* pasted into locked, read-only code blocks
* used by designers, PMs, and engineers

GitHub remains the **source of truth**.

---

## FAQ

### Can I copy prompts directly from GitHub?

Technically yes, but **not recommended** unless you are a maintainer. Always prefer the official Notion page to avoid partial copies or outdated versions.

### Can I modify a prompt for my team?

No. If a change is needed, propose it via Pull Request so it can be reviewed and shared consistently.

### What if my system doesn’t fit existing overlays?

Open a PR to propose a new overlay. Do not fork or locally modify prompts.

---

## Contact

For questions, changes, or clarification, contact the **Content Design team** or open an issue in this repository.
