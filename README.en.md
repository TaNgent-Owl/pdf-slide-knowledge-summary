# PDF Slide Knowledge Summary Skill

[中文](README.md) | English

This is a Codex skill for creating, extending, and auditing exam-oriented knowledge summaries from PDF slide decks, especially Chinese university lecture PDFs exported from PowerPoint.

## Features

This skill focuses on:

- generating page-cited Markdown study notes
- writing Chinese-first summaries with English terminology labels
- organizing exam points, formulas, concepts, examples, pitfalls, and exam techniques by chapter
- handling low-confidence formula OCR issues common in PPT-exported PDFs
- checking coverage, page citations, formula correctness, and Markdown rendering in iterative review loops
- keeping course-material directories clean and avoiding temporary-file clutter in the root directory

## Files

```text
.
├── SKILL.md
├── agents/
│   └── openai.yaml
├── README.md
└── README.en.md
```

## Use

Install or copy this folder into a Codex skills directory, then ask Codex to use `pdf-slide-knowledge-summary` when summarizing or auditing PDF slide summaries.

Example prompt:

```text
Use the pdf-slide-knowledge-summary skill to summarize these lecture PDFs into an exam-oriented Markdown knowledge summary with page citations.
```

## Scope

This skill is designed for lecture slide PDFs and course review notes. It is not meant for generic article summarization, homework solving, or producing slide decks.

## Attribution

The initial version of `SKILL.md` was derived from and adapted for local course-summary workflows based on:

https://github.com/Li-Baichuan-James/summarize-slides-skill

This repository adds project-specific experience from summarizing PDF lecture slides, including UTF-8 extraction precautions on Windows, formula OCR limitations, Markdown table safety checks, and iterative summary auditing rules.

Subsequent changes in this repository after the initial adaptation are AI-generated.
