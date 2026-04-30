---
name: pdf-slide-knowledge-summary
description: Create, extend, or audit exam-oriented knowledge summaries from PDF slide decks, especially Chinese university lecture PDFs exported from PPT. Use when Codex needs to summarize course PDFs into page-cited Markdown notes, append new PDF chapters to an existing summary, verify a summary against source slides, preserve formulas, manage OCR/formula screenshots, or maintain a clean slide-summary workspace.
---

# PDF Slide Knowledge Summary

## Outcome

Produce or improve a Markdown study summary that is concise, exam-oriented, source-cited, and safe to extend as more PDF chapters are added.

Default output style:

- Chinese main text.
- Important terms include English in parentheses.
- Each nontrivial point has exact PDF physical page references such as `P42` or a tight range like `P42-P44`.
- Use the same chapter structure throughout: chapter structure, core exam points, core formulas, bilingual concepts, typical problems, pitfalls and exam techniques.
- Embed formulas in the relevant knowledge area. Do not create a separate formula appendix.

## Workspace Rules

Read local project instructions first, especially `AGENTS.md`, `CLAUDE.md`, or equivalent. Course directories often have stricter cleanup rules than generic skills.

For this optical-fiber course layout, keep the root directory clean:

- Root may contain only source PDFs, repository instructions, the final summary Markdown, and approved subfolders.
- Put helper scripts in `scripts/`.
- Put formula OCR JSON in `formula_data/`.
- Put formula screenshots in `formula_images/`.
- Put skill files or experiments in a dedicated subfolder such as `.Codex/skills/<skill-name>/`.
- Do not leave temporary `.txt`, `.json`, `.py`, cache, or intermediate `.md` files in the root.

If project rules conflict with a user request to "put the file in the current directory", prefer a subfolder under the current directory that satisfies the cleanup rule, and state the path.

## PDF Intake

Use explicit, encoding-safe extraction.

- On Windows, use `python`, not `python3`; `python3` may be a Microsoft Store stub.
- For text extraction, prefer `pypdf.PdfReader` and write UTF-8 files with `open(..., encoding="utf-8")`.
- Do not print extracted Chinese PDF text directly to the terminal; GBK consoles can corrupt output or raise `UnicodeEncodeError`.
- Treat `=== Page N ===` from extraction as the physical PDF page number. Citations should use that number.
- If a PDF has hundreds of pages, segment by chapter/topic before detailed summarization. Do not one-shot summarize long decks.

Temporary extracted text belongs in an approved subfolder and should be deleted after the summary is updated.

## Formula Handling

Use a two-tier formula policy:

1. Human-verified core formulas go directly into the chapter's `N.3 核心公式` table as inline math.
2. OCR formulas from `formula_data/*.json` are supporting evidence only. Embed them only when both syntactically valid and semantically checked against the slide or screenshot.

For PPT-exported PDFs, expect low formula OCR reliability. Common failure modes:

- Fragmented superscripts/subscripts become separate formulas.
- Long `\begin{array}` output is often OCR noise.
- Formula-dense pages may produce many small fragments.
- Bracket-balanced LaTeX can still be semantically wrong.

When uncertain, prefer a screenshot reference from `formula_images/` over wrong LaTeX. Use relative paths when the summary Markdown sits beside `formula_images/`.

Markdown table safety:

- Avoid raw `|` inside inline math in tables; use `\mid`, `\vert`, or `\lvert...\rvert`.
- Keep units in math mode with `\mathrm{}` and spacing such as `$0.20\,\mathrm{dB/km}$`.
- Use standard commands such as `\log`, `\sin`, `\frac{}{}`, superscripts `^{}`, and subscripts `_{}`.

## Summary Structure

For each chapter, use:

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

Core exam points should be compact but not cryptic:

- State what to remember.
- Add why it matters or how it is tested.
- Include formulas only where they support that point.
- Prefer comparison tables for systems, device classes, standards, and modulation schemes.
- Add typical problem patterns when the slides include calculable quantities or standard comparisons.

## Verification Loop

Run at least one fresh verification pass before finalizing a summary. For iterative improvement, use this loop:

1. Check coverage: every chapter/topic range in the slides is represented or intentionally omitted.
2. Check citations: sample important bullets and formulas against the cited physical pages.
3. Check formulas: verify units, constants, variable names, Markdown table safety, and whether OCR-derived math is trustworthy.
4. Check exam usefulness: ensure each chapter includes typical problems and pitfalls, not only concepts.
5. Check rendering hygiene: headings, anchors, tables, inline math, and image paths should render cleanly.
6. Update the summary, then update this skill with any new reusable lesson learned.

Do not treat the first polished draft as final if verification finds recurring issues. Record patterns, not one-off corrections.

## Audit Checklist

Use this checklist when reviewing an existing summary:

- Encoding: source files display as valid UTF-8, and no mojibake appears in saved Markdown.
- Page count: each chapter's stated page count matches the PDF.
- Section map: `N.1` page ranges cover the PDF without large unexplained gaps.
- Citations: high-value facts, examples, formulas, and standards have tight page citations.
- Formula table: formulas are mathematically plausible, use consistent units, and avoid table-breaking characters.
- OCR policy: no raw OCR formula list or unchecked noisy LaTeX has been pasted into the summary.
- Terminology: core Chinese terms have English labels where useful.
- Cross-chapter appendix: if present, it should synthesize high-frequency exam points rather than repeat each chapter verbatim.
- Cleanup: no temporary extraction files, cache folders, or intermediate Markdown files remain in the root.

## Append New PDFs

When new chapter PDFs are added:

1. Extract text to an approved temporary location using UTF-8.
2. Run or update the formula extraction pipeline if formulas matter.
3. Summarize each new chapter independently using the six-section structure.
4. Merge into the existing final Markdown, updating the table of contents and cross-chapter appendix.
5. Delete temporary text and cache files.
6. Run the audit checklist.

For one or two new PDFs, append to the existing summary rather than regenerating all chapters unless previous structure is broken.

## Red Flags

Stop and fix before final delivery if you see:

- Missing page citations on major points.
- A long PDF summarized in one pass with no segmentation.
- Formulas copied from OCR without semantic checking.
- A standalone OCR formula appendix.
- Root directory clutter from temporary files.
- Markdown tables broken by inline `|` in math.
- English terms omitted from key concept tables.
- A summary that lists concepts but lacks typical problem patterns or pitfalls.

## Iteration Notes

When this skill is improved from a real audit, add short, reusable lessons here. Keep lessons general enough to apply to similar PDF slide decks.

- Iteration 1: Always run a table pipe-count check after adding formulas. Absolute value bars like `$|\Delta\nu|$` can silently break Markdown tables; prefer `\lvert...\rvert` in table cells.
- Iteration 2: Sample formula-table rows against rendered PDF pages, not only extracted text. Important formulas in PPT PDFs may be invisible or incomplete in text extraction, and OCR JSON may also miss them. In this audit, rendered-page checking caught wrong BL-product formulas that text extraction did not expose.
- Iteration 3: Audit symbol collisions across chapters and sections. Slide decks often reuse symbols such as `\alpha` for unrelated quantities, for example graded-index profile exponent and attenuation coefficient. Prefer a disambiguating notation in the summary and add an explicit pitfall when the slides use overloaded symbols.
- Iteration 4: When multiple slide decks cover the same lecture at different weeks or revisions, compare overlap before writing chapters. Use the newest or most complete deck as the primary source, cite older decks only for cross-checking or deltas, and state this de-duplication policy in the summary. Also warn that exported PPT page footers may differ from physical PDF page numbers; citations should follow physical PDF pages.
- Iteration 5: For practical computing, embedded, Android, or tooling courses with few mathematical formulas, keep the `N.3` slot but rename it to command/API/configuration quick reference. Preserve exact commands, ports, paths, callbacks, and version numbers, and add safety notes for destructive or privileged operations such as `rm -rf`, bootloader unlock, flashing, and Root.
- Iteration 6: For PPTX intake, first try a real slide renderer/exporter such as PowerPoint COM or LibreOffice to create PDFs in an approved subfolder, then extract text from those PDFs so citation pages match rendered slides. Record the conversion output, page counts, and whether citations refer to original PDFs or converted PDFs. If conversion fails, fall back to PPTX XML extraction and label citations as slide numbers rather than PDF pages.
