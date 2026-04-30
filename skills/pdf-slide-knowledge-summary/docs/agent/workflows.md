# Workflows

## Append New PDF Chapters

When new chapter PDFs are added:

1. Extract text to an approved temporary location using UTF-8 (never print to terminal).
2. Run or update the formula extraction pipeline if formulas matter.
3. Summarize each new chapter independently using the six-section structure.
4. Merge into the existing final Markdown, updating the table of contents and cross-chapter appendix.
5. Delete temporary text and cache files.
6. Run the full [audit checklist](verification.md#audit-checklist).

For one or two new PDFs, append to the existing summary rather than regenerating all chapters unless the previous structure is broken.

## PDF Text Extraction (Encoding-Safe)

On Windows:

- Use `python`, **not `python3`** — `python3` may be a Microsoft Store stub that exits with code 49.
- Prefer `pypdf.PdfReader` over `pdfplumber` for Python 3.14 / Windows compatibility.
- **Never** `print()` extracted Chinese text to terminal — Windows GBK consoles raise `UnicodeEncodeError`.
- Always write to UTF-8 files: `open(path, 'w', encoding='utf-8')`.

Example pattern:

```python
from pypdf import PdfReader
reader = PdfReader('chapter.pdf')
with open('out.txt', 'w', encoding='utf-8') as f:
    for i, page in enumerate(reader.pages):
        text = page.extract_text()
        if text:
            f.write(f'\n=== Page {i+1} ===\n')
            f.write(text + '\n')
```

- `=== Page N ===` corresponds to physical PDF page number. Use that for citations.
- For PDFs with hundreds of pages, segment by chapter/topic before detailed summarization.

## Multi-PDF Parallel Strategy

Chapters are independent — summarize them in parallel:

1. Extract text from each PDF → `chN_text.txt` (in an approved subfolder).
2. Dispatch background agents, each reading one text file.
3. All agents return → merge into final `.md`.
4. Delete intermediate `.txt` files.

Each agent prompt must require: Chinese main text + English terms in parentheses + page citations + six-section output (structure / exam points / formulas / concepts / problems / pitfalls).

## PPTX Intake

For PPTX input:

1. First try a real slide renderer/exporter (PowerPoint COM or LibreOffice) to create PDFs in an approved subfolder.
2. Extract text from those PDFs so citations match rendered slides.
3. Record the conversion output, page counts, and whether citations refer to original PDFs or converted PDFs.
4. If conversion fails, fall back to PPTX XML extraction and label citations as slide numbers rather than PDF pages.
