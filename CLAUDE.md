# Agent Rules

## Session & Understanding

**Pending Questions Hook**
- At the start of every session, check if `.claude/PENDING.md` exists.
- If present, read it immediately and ask the listed questions to the user before doing new work.
- Once the user answers, delete or mark the questions as resolved in the file.
- Treat the pending-questions hook as a critical alignment mechanism; never ignore it.
- **PENDING.md = current action items**, may be rewritten per session.

**Backlog Hook**
- `.claude/BACKLOG.md` is **append-only**. NEVER overwrite or clear it.
- At session start: peek first 20 lines only (progressive reading). Full read only if relevant to current task.
- To add items: append at the bottom. To mark done: add ~~strikethrough~~.
- Future plans, deferred tasks, and external suggestions go here, NOT in PENDING.
- **Archive rule**: when BACKLOG.md exceeds 200 lines, move completed (strikethrough) items to a new file in `.claude/backlog_archive/` (e.g., `archive_20260306.md`) and keep only active items in BACKLOG.md.

**Entropy Reduction (CRITICAL)**
- Rephrase and confirm the request before executing.
- Flag conflicts with prior decisions; request confirmation before switching approaches.
- Break multi-part requests into sub-tasks and confirm each step.
- List assumptions explicitly and ask user to validate.
- Summarize agreements in bullets for sign-off.

**Collaboration**
- After meaningful edits, summarize progress and ask targeted questions.
- When user spots mistakes, acknowledge and fix immediately.
- Co-evolve solutions; treat collaboration as dialogue, not one-way report.
- Tie check-ins to concrete artifacts (files touched, behaviors changed) for quick verification.
- Remind the user to document important decisions under `docs/` using timestamped format when appropriate.

**Execution Protocol**
- **Protected Scopes**: Changes to `CLAUDE.md` or `.env` are high-stakes. MUST pause for explicit confirmation before applying.
- **Instruction Priority**: Explicit constraints in Plan (e.g., "part-by-part", "step-by-step", "wait for review") are **BINDING**. They override default autonomy.
- **Default Mode**: For all other tasks, act autonomously and efficiently.

---

## Code Style & Quality

**Code Style**
- Keep functions short and focused.
- CSS: use CSS custom properties (variables) for theming; group related rules with section comments.
- JS: vanilla ES6+, no frameworks. Use data-attributes for DOM hooks, not class names.
- HTML: semantic elements, accessible markup (aria-labels, alt text).

**Code Review**
- After implementing a feature, explicitly walk through key design decisions and edge cases.
- Don't assume user has reviewed every detail—they may trust the code blindly.
- Before deploying, summarize what the code actually does vs. what user might expect.
- Use ASCII diagrams or code snippets to show critical logic paths for user verification.

**Error Handling**
- Add try-catch blocks only around operations expected to fail (fetch, DOM queries on optional elements).
- Do not use blanket or deeply nested exception handling.

---

## Development Workflow

**Modular Development**
- Implement new functionality as self-contained sections.
- Test visual changes in browser before committing.

**Manageable Milestones**
- Keep the active milestone small and tractable.
- If the gap to the next big goal is large, refine it into smaller sub-milestones instead of jumping straight there.
- After finishing the current milestone, define the next set of sub-milestones.
- Balance big-picture roadmap awareness with flexible, stepwise execution.

**Sub-Agent Delegation (CRITICAL)**
- **Never use sleep to wait**: Don't block main thread with `sleep` for long-running tasks.
- **Delegate monitoring to sub-agents**: Use `Task` tool with `run_in_background=true` for tasks requiring polling/waiting.
- **Design concise deliverables**: When prompting sub-agents, explicitly request:
  - Summary-only output (no raw logs)
  - Structured results (key findings, pass/fail)
  - Bounded response length

**Sub-Agent Progressive Delivery**

Reports and analyses delivered in layers; readers drill down as needed:

| Level | Lines | Content |
|-------|-------|---------|
| 0: Verdict | 1 | "DONE: Goal achieved" |
| 1: TL;DR | 5 | Core conclusions |
| 2: Key Sections | 50 | Key findings |
| 3: Full Report | Full | Complete details |

**Reading Strategy** (when reading reports/docs):
1. **Verdict first**: Read first 2-5 lines for pass/fail status
2. **TL;DR next**: If verdict unclear, read first 10-20 lines for summary
3. **Sampling**: If still need more, read head + middle + tail sections
4. **Full read**: Only when absolutely necessary

> Principle: Minimize context consumption while getting enough info to decide.

---

## Files & Documentation

**Files Management**
- Put new documents in `docs/`.
- Place reference code in `ref_codes/` (gitignored; use as inspiration, do not copy verbatim).
- Website assets (images, logos, videos) in `assets/`.

**Documentation**
- Before naming a doc, run `date +%Y%m%d%H` to get the current timestamp.
- Put docs in `docs/` with format `yyyymmddhh_TITLE_IN_CAPS_WITH_UNDERSCORES.md`. Update existing docs rather than duplicating.
- Capture decisions, design rationale, and content changes rigorously in docs.

**Document Archiving** (when docs get too long)
- Use copy-rename-compress-link pattern:
  1. Copy the original file to `{filename}_ARCHIVE.md`
  2. Surgically edit/compress the original, keeping only active content
  3. Add a link at top: `> Archived content: [ARCHIVE](./path_to_archive.md)`

**Git Workflow**
- Never commit `.env` (secrets).
- **Commit before run**: After modifying code, MUST commit before testing/deploying. This ensures any deployed version maps to a specific commit.
- Commit after completing each meaningful feature or fix; don't accumulate too many changes.
- Before committing, review `git status` to avoid unintended deletions or additions.
- Commit message format: `type: description` (types: feat, fix, docs, refactor, chore).
- While the user acknowledges the credits of Claude, DO NOT include any information about Claude in the commit message.

**Security Rules**
- **Never read .env directly**: Do not use `cat`, `read_file`, or any tool to view `.env` contents.
- **Never expose credentials**: Do not display API keys, tokens, or passwords in command output.
- **Stop on credential errors**: If authentication fails, stop immediately and let the user troubleshoot.

---

## Output & Presentation

**Output Format**
- Default to Markdown for prose; fenced code for snippets.
- Use LaTeX for math: \( inline \), \[ block \].
- Provide symbol cheat-sheets for complex formulas.

**ASCII Diagrams**
- When user asks to explain something complex, prefer ASCII box-and-arrow diagrams.
- Use table format for comparisons, flow format for pipelines.
- Keep diagrams compact but complete.

---

## Communication

- Default language: Chinese (mixed Chinese-English OK).
- Be concise.

---

# Project-Specific

## Project Nature

This is a **static website project** for the NeurIPS 2026 RealPDE Competition.
Primary tasks: website development, content management, competition page design.
Stack: vanilla HTML/CSS/JS (single-page static site). No build system, no framework.

---

## Competition Overview

- **Title**: NeurIPS 2026 RealPDE Competition: Scientific ML for Real-world Physical Systems
- **Tracks**: Sim2Real Transfer Learning + Long-Term Test-Time Adaptation (LTTTA)
- **Dataset**: NACA4418 airfoil — paired real-world PIV + 3D CFD simulation data
- **Platform**: Codabench
- **Conference**: NeurIPS 2026, December 6, 2026, Sydney, Australia

### Key Dates

| Milestone | Date |
|-----------|------|
| Competition launch | June 15, 2026 |
| Warm-up phase end | July 19, 2026 |
| Development phase end | September 27, 2026 |
| Final phase end | October 25, 2026 |
| Results announced | November 10, 2026 |
| Code deadline | November 25, 2026 |
| NeurIPS presentation | December 6, 2026 |

### Prizes (per track)

1st: $5,000 / 2nd: $3,000 / 3rd: $1,000

---

## File Structure

```
index.html              — Main competition website (single-page)
CLAUDE.md               — Agent rules (this file)
docs/                   — Documentation & source materials
  competition_proposal.md — PDF proposal dumped to markdown
ref_codes/              — Reference codebases (gitignored)
  realpdebench-website/  — Original RealPDEBench site (MkDocs source)
assets/                 — Images, logos, videos for the website
  images/
  logos/
```

---

## Design System

Inherits from RealPDEBench website:
- **Fonts**: Fraunces (serif headings) + Inter (sans body)
- **Palette**: Lavender (`#D8D4E6`), Surface (`#F9F9FC`), Accent (`#6C5CE7`), Mint (`#4E776B`), Carbon (`#1D1D1F`)
- **Reference**: see `ref_codes/realpdebench-website/docs/stylesheets/custom.css`

---

## Development Rules

- Edit `index.html` directly — no build step needed
- Preview: `open index.html` or use a local server (`python3 -m http.server`)
- Keep all CSS inline in `<style>` (single file deployment)
- Keep all JS inline in `<script>` (single file deployment)
- Math rendering: MathJax 3 (CDN)
- Responsive: mobile-first, breakpoint at 768px

---

## Content Source

The competition proposal PDF (`neurips_2027_workshop_competition_draft.pdf`) is the single source of truth.
Markdown dump at `docs/competition_proposal.md` for easy reference.
Any content changes should be verified against the proposal.
