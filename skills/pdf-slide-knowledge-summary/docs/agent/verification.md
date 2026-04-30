# Verification Loop & Audit Checklist

## Verification Loop

Run at least one fresh verification pass before finalizing a summary. For iterative improvement:

1. **Coverage**: every chapter/topic range in the slides is represented or intentionally omitted.
2. **Citations**: sample important bullets and formulas against the cited physical pages.
3. **Formulas**: verify units, constants, variable names, Markdown table safety, and whether OCR-derived math is trustworthy.
4. **Exam usefulness**: each chapter includes typical problems and pitfalls, not only concepts.
5. **Rendering hygiene**: headings, anchors, tables, inline math, and image paths render cleanly.
6. **Update**: update the summary, then update the skill with any new reusable lesson learned.

Do not treat the first polished draft as final if verification finds recurring issues. Record patterns, not one-off corrections.

## Audit Checklist

Use when reviewing an existing summary:

- **Encoding**: source files display as valid UTF-8, no mojibake in saved Markdown.
- **Page count**: each chapter's stated page count matches the PDF.
- **Section map**: `N.1` page ranges cover the PDF without large unexplained gaps.
- **Citations**: high-value facts, examples, formulas, and standards have tight page citations.
- **Formula table**: formulas are mathematically plausible, use consistent units, and avoid table-breaking characters.
- **OCR policy**: no raw OCR formula list or unchecked noisy LaTeX has been pasted into the summary.
- **Terminology**: core Chinese terms have English labels where useful.
- **Cross-chapter appendix**: if present, synthesize high-frequency exam points rather than repeat each chapter verbatim.
- **Cleanup**: no temporary extraction files, cache folders, or intermediate Markdown files remain in the root.

## Symbol Collision Check

Slide decks often reuse symbols (e.g., `\alpha` for both graded-index profile exponent and attenuation coefficient). Prefer disambiguating notation in the summary and add an explicit pitfall when slides use overloaded symbols.

## Deck Deduplication

When multiple slide decks cover the same lecture at different weeks/revisions:
- Compare overlap before writing chapters.
- Use the newest/most complete deck as primary source.
- Cite older decks only for cross-checking or deltas.
- State the de-duplication policy in the summary.
- Warn that exported PPT page footers may differ from physical PDF page numbers; citations follow physical PDF pages.
