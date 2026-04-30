# PDF Slide Knowledge Summary

A Codex skill for creating, extending, and auditing exam-oriented knowledge summaries from PDF slide decks, especially Chinese university lecture PDFs exported from PowerPoint.

The skill focuses on:

- page-cited Markdown study notes
- Chinese-first summaries with English terminology labels
- chapter-by-chapter exam points, formulas, concepts, examples, and pitfalls
- formula handling for PPT-exported PDFs, including OCR caution rules
- verification loops for coverage, citations, formulas, and Markdown rendering
- clean workspace rules for course-material directories

## Files

```text
.
├── SKILL.md
├── agents/
│   └── openai.yaml
└── README.md
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
