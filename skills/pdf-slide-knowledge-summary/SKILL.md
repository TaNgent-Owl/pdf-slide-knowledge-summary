---
name: pdf-slide-knowledge-summary
description: Create, extend, or audit exam-oriented knowledge summaries from PDF slide decks, especially Chinese university lecture PDFs exported from PPT. Use when Codex needs to summarize course PDFs into page-cited Markdown notes, append new PDF chapters to an existing summary, verify a summary against source slides, preserve formulas, manage OCR/formula screenshots, or maintain a clean slide-summary workspace.
---

# PDF Slide Knowledge Summary

## Outcome

Produce or improve a Markdown study summary that is concise, exam-oriented, source-cited, and safe to extend as more PDF chapters are added.

Default output: Chinese main text, English terms in parentheses, page citations (P42 or P42-P44) on every nontrivial point, six-section chapter structure (structure / exam points / formulas / concepts / problems / pitfalls). Embed formulas in the relevant knowledge area. No standalone formula appendix.

## Quick Start

1. Read the project's `CLAUDE.md` (or `AGENTS.md`) first — course directories often have stricter cleanup rules than this skill.
2. Extract PDF text with `pypdf.PdfReader` → UTF-8 file (never print to terminal).
3. Run formula extraction pipeline if formulas matter.
4. Summarize each chapter → merge into final `.md` → run verification pass.
5. Delete all temporary files and cache folders.

## Hard Rules

### Environment (Windows 11 特定)

> 以下约束基于 Windows 11 + Python 3.14 环境。其他平台可忽略。

- Use `python`, **not `python3`** — on Windows, `python3` may be a Microsoft Store stub.
- Prefer `pypdf.PdfReader` for text extraction on Python 3.14 / Windows.
- Formula OCR pipeline requires Conda Python 3.12 (pix2tex C extensions fail on 3.14).

### Encoding (Windows 11 特定)

> Windows 终端默认 GBK 编码，与 PDF Unicode 文本不兼容。

- **Never** `print()` extracted Chinese PDF text to terminal — write UTF-8 files with `open(path, 'w', encoding='utf-8')`.
- `=== Page N ===` from extraction = physical PDF page number. Use that for citations.

### Workspace Cleanliness
- Root directory: only source PDFs, repo instructions, final summary `.md`, and approved subfolders.
- Scripts → `scripts/`, formula OCR JSON → `formula_data/`, formula screenshots → `formula_images/`.
- No temporary `.txt`, `.json`, `.py`, `__pycache__/`, or intermediate `.md` in root.
- If a user request conflicts with cleanup rules, put the file in a compliant subfolder and state the path.

### Formulas
- Two-tier policy: verified core formulas → inline `$...$`; OCR formulas → embed only after syntax + semantic check.
- No standalone formula appendix. No raw OCR formula lists.
- In table cells, replace `$|x|$` with `$\lvert x\rvert$` to avoid breaking Markdown tables.

### Segmentation
- For PDFs with hundreds of pages, segment by chapter/topic before summarizing. Do not one-shot summarize long decks.

## Summary Structure

For each chapter, use exactly this template:

```markdown
# 第N章 <章名>

**课件：** <pdf 文件名>（<页数> 页）

## N.1 章节结构
## N.2 核心考点
## N.3 核心公式
## N.4 关键概念（中英双语）
## N.5 典型例题
## N.6 易错点与考试技巧
```

Core exam points must be compact but actionable: what to remember, why it matters, how it's tested. Prefer comparison tables for systems, device classes, standards, and modulation schemes. Add typical problem patterns when slides include calculable quantities.

For non-math courses (computing, embedded, Android), keep the `N.3` slot but rename it to command/API/configuration quick reference. Preserve exact commands, ports, paths, callbacks, version numbers, and safety notes for destructive operations.

## Formula Policy (Summary)

- **Tier 1**: Human-verified formulas → inline `$...$` in core formula table and body text.
- **Tier 2**: OCR formulas from `formula_data/*.json` → embed only when both bracket-balanced and semantically verified against the slide or screenshot. When uncertain, use a screenshot reference.
- PPT OCR is ~5-10% accurate. Expect fragmented subscripts, array noise, and bracket-balanced but semantically wrong output.

Full rules: [docs/agent/formulas.md](docs/agent/formulas.md)

## Verification

Run at least one verification pass before finalizing. Check coverage, citations, formulas, exam usefulness, and rendering hygiene.

Full checklist: [docs/agent/verification.md](docs/agent/verification.md)

## Red Flags

Stop and fix before delivery if you see:

- Missing page citations on major points.
- A long PDF summarized in one pass with no segmentation.
- Formulas copied from OCR without semantic checking.
- A standalone OCR formula appendix.
- Root directory clutter from temporary files.
- Markdown tables broken by inline `|` in math.
- English terms omitted from key concept tables.
- A summary that lists concepts but lacks typical problem patterns or pitfalls.

## Deep Docs

| Document | Content |
|---|---|
| [docs/agent/formulas.md](docs/agent/formulas.md) | Formula detection, OCR, LaTeX validation, format rules |
| [docs/agent/verification.md](docs/agent/verification.md) | Verification loop, audit checklist, symbol collision, deck dedup |
| [docs/agent/workflows.md](docs/agent/workflows.md) | Append PDFs, extraction patterns, parallel strategy, PPTX intake |
| [docs/agent/history.md](docs/agent/history.md) | Iteration lessons learned from past audits |
