# PDF Slide Knowledge Summary

[中文](README.md) | English

A skill for creating, extending, and auditing exam-oriented knowledge summaries from PDF slide decks, especially Chinese university lecture PDFs exported from PowerPoint.

## Features

- Generates page-cited Markdown study notes
- Chinese-first summaries with English terminology labels
- Organizes exam points, formulas, concepts, examples, pitfalls, and exam techniques by chapter
- Two-tier formula policy: human-verified formulas as inline LaTeX, uncertain OCR formulas as screenshots
- Structured verification loop covering coverage, citations, formulas, and rendering
- Keeps course directories clean — root limited to PDFs, instructions, and final summary
- Supports PPTX input via PDF conversion

## File Structure

```text
.
├── README.md
├── README.en.md
└── skills/
    └── pdf-slide-knowledge-summary/
        ├── SKILL.md              # Skill entry point (read first)
        ├── agents/
        │   └── openai.yaml
        └── docs/
            └── agent/
                ├── formulas.md      # Formula detection, OCR, LaTeX validation
                ├── verification.md  # Verification loop & audit checklist
                ├── workflows.md     # New PDF intake, extraction, parallel strategy
                └── history.md       # Lessons from past audit iterations
```

## Environment Notes

Several technical constraints are specific to a **Windows 11 + Python 3.14** environment:

- `python` not `python3` — on Windows, `python3` may be a Microsoft Store stub
- Terminal default GBK encoding → always write UTF-8 files, never `print()` Chinese PDF text to terminal
- `pypdf` is more compatible than `pdfplumber` on Python 3.14 / Windows
- Formula OCR pipeline requires Conda Python 3.12 (pix2tex C extensions fail on 3.14)

Other constraints (directory cleanliness, formula policy, verification workflow) apply to all platforms.

## Use

### cc switch

When adding this as a custom repository in cc switch, use:

```text
Repository: TaNgent-Owl/pdf-slide-knowledge-summary
Branch: main
Subdirectory: skills
```

`Subdirectory: skills` is required. This repository uses a multi-skill layout — the actual skill lives at `skills/pdf-slide-knowledge-summary/`.

### Manual install

Copy `skills/pdf-slide-knowledge-summary/` into a skills directory, then invoke `pdf-slide-knowledge-summary` when summarizing or auditing PDF slide decks.

Example prompt:

```text
Use the pdf-slide-knowledge-summary skill to summarize these lecture PDFs into an exam-oriented Markdown knowledge summary with page citations.
```

## Scope

Designed for lecture slide PDFs and course review notes. Not meant for generic article summarization, homework solving, or producing slide decks.

## Attribution

The initial version of `SKILL.md` was adapted from:

https://github.com/Li-Baichuan-James/summarize-slides-skill

This repository adds practical experience from summarizing PDF lecture slides, including Windows UTF-8 extraction precautions, formula OCR limitations and LaTeX validation, Markdown table safety checks, and iterative auditing rules.

Subsequent changes after the initial adaptation are AI-generated.
