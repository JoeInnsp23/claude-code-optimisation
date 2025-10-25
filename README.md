# Claude Code Optimisation (v3)

A structured optimisation framework for **Claude Code** designed to enforce deterministic behaviour, token efficiency, and safe reasoning across development projects.  
v3 introduces explicit **tool usage**, **Context7 MCP integration**, and **clarification-driven interactivity**.

---

## 🔍 Overview

Claude Code’s default behaviour can drift — verbose, compliant yet passive, or inconsistent in structure.  
This repo defines a **system layer (`CLAUDE.md`)**, **output style policies**, and **usage templates** that enforce high-confidence, auditable outputs across all Claude Code projects.

---

## ⚙️ Key v3 Upgrades

### 🧩 Explicit Tool Usage
Claude v3 **explicitly invokes available tools** instead of inferring actions.  
This enforces reproducibility and accountability, ensuring model behaviour remains within defined scope.

### 🧠 Context7 MCP Integration
v3 now uses **Context7 MCP** for:
- Formatting validation (Markdown, JSON, YAML)
- Structure audits before final output
- Auto-repair of formatting drift during generation

> ⚠️ **Requirement:**  
> All A/B tests and production prompts must use Context7 MCP enabled to validate structure compliance.

### 💬 askUserQuestion Integration
v3 actively uses the `askUserQuestion` tool when uncertain — making Claude more **interactive** and **human-aligned** rather than overly compliant.  
This feature triggers targeted clarifications before producing final outputs, reducing hallucinations and assumption-based errors.

---

## 📁 Repository Contents

| File | Purpose |
|------|----------|
| [`CLAUDE.md`](./CLAUDE.md) | Global configuration and behavioural rules |
| [`CLAUDE_MD_USAGE.md`](./CLAUDE_MD_USAGE.md) | Explains how Claude Code consumes this repo |
| [`strict-concise-v3.md`](./strict-concise-v3.md) | **Current standard style** (explicit tool + Context7 + askUserQuestion) |
| [`strict-concise-v2.md`](./strict-concise-v2.md) | Legacy compact mode for comparison |
| [`claude-pr-checklist.md`](./claude-pr-checklist.md) | Consistency and compliance checklist |
| [`claude-project-template.md`](./claude-project-template.md) | Boilerplate for new repos |

---

## 🚀 Quick Start

1. Copy `CLAUDE.md` into your project root.  
2. Reference your selected style file in your prompts (e.g., `strict-concise-v3.md`).  
3. Enable **Context7 MCP** and **askUserQuestion** in your Claude Code runtime.  
4. Run your A/B tests comparing `v2` vs `v3`:
   - Check structure compliance via Context7
   - Review interactivity and clarification quality
   - Record token efficiency and hallucination rate

---

## 📊 A/B Testing Metrics

| Metric | Purpose |
|---------|----------|
| **Token efficiency** | Lower output cost without data loss |
| **Clarification rate** | How often `askUserQuestion` triggers |
| **Compliance score** | Pass/fail on Context7 formatting checks |
| **Output stability** | Reproducibility under identical prompts |

> Use `claude-pr-checklist.md` for structured scoring and iteration tracking.

---

## 🧠 Design Philosophy

v3 aims to make Claude **collaborative, accountable, and verifiable**:
- Treats user queries as dynamic dialogues, not static completions  
- Performs **pre-flight checks** on structure and validity  
- Surfaces **low-confidence states** for user resolution  
- Integrates **Context7 MCP** for programmatic linting and schema fidelity

---

## 🪪 License
MIT (or project-specific license)

---

## 🧩 Credits
Developed by **Joseph Stephenson-Mouzo (Innspired / s-Mouzo)**  
Operationalising AI-assisted coding with structured safety, consistency, and real-world deployment in mind.
