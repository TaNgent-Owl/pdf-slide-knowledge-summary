# Historical Iteration Notes

Lessons learned from real audits. Keep entries general enough to apply to similar PDF slide decks.

## Iteration 1 — Table Pipe Safety

Always run a table pipe-count check after adding formulas. Absolute value bars like `$|\Delta\nu|$` can silently break Markdown tables; prefer `\lvert...\rvert` in table cells.

## Iteration 2 — Rendered Page Verification

Sample formula-table rows against rendered PDF pages, not only extracted text. Important formulas in PPT PDFs may be invisible or incomplete in text extraction, and OCR JSON may also miss them. Rendered-page checking caught wrong BL-product formulas that text extraction did not expose.

## Iteration 3 — Symbol Collision

Audit symbol collisions across chapters and sections. Slide decks often reuse symbols such as `\alpha` for unrelated quantities (e.g., graded-index profile exponent vs. attenuation coefficient). Prefer disambiguating notation and add explicit pitfalls for overloaded symbols.

## Iteration 4 — Deck Deduplication

When multiple slide decks cover the same lecture at different weeks/revisions, compare overlap first. Use the newest/most complete deck as primary, cite older decks only for cross-checking or deltas, and state this policy in the summary. Exported PPT page footers may differ from physical PDF page numbers — follow physical PDF pages.

## Iteration 5 — Non-Math Course Adaptation

For practical computing, embedded, Android, or tooling courses with few mathematical formulas, keep the `N.3` slot but rename it to command/API/configuration quick reference. Preserve exact commands, ports, paths, callbacks, and version numbers. Add safety notes for destructive or privileged operations (e.g., `rm -rf`, bootloader unlock, flashing, Root).

## Iteration 6 — PPTX Intake

For PPTX input, first try a real slide renderer/exporter (PowerPoint COM or LibreOffice) to create PDFs in an approved subfolder, then extract text from those PDFs so citation pages match rendered slides. Record conversion output, page counts, and whether citations refer to original or converted PDFs. If conversion fails, fall back to PPTX XML extraction and label citations as slide numbers.
