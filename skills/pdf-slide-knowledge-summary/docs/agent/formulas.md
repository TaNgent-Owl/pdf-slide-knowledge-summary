# Formula Handling — Detailed Rules

## Two-Tier Formula Policy

1. **Human-verified core formulas** → inline math (`$...$`) in `N.3 核心公式` table.
2. **OCR formulas from `formula_data/*.json`** → supporting evidence only. Embed only when both syntactically valid and semantically checked against the slide or screenshot.

When uncertain, prefer a screenshot reference from `formula_images/` over wrong LaTeX.

## OCR Reliability (PPT PDFs)

For PPT-exported PDFs, expect low formula OCR reliability (~5-10% fully correct). Common failure modes:

- Fragmented superscripts/subscripts become separate formulas.
- Long `\begin{array}` output is often OCR noise — short arrays (<200 chars) may be real multi-line, long arrays are almost certainly noise.
- Formula-dense pages may produce many small fragments.
- Bracket-balanced LaTeX can still be semantically wrong.

## LaTeX Output Convention

- Variables use default math italic: `n_{1}`, `\Delta`, `\alpha`
- Text in math mode: `\mathrm{P}_{\mathrm{out}}`, `\mathrm{dBm}`
- Standard functions: `\log`, `\sin`, etc.
- Fractions: `\frac{}{}`
- Superscripts/subscripts: `^{}`, `_{}`

## Markdown Table Safety

- Avoid raw `|` inside inline math in tables; use `\mid`, `\vert`, or `\lvert...\rvert`.
- Keep units in math mode with `\mathrm{}` and spacing: `$0.20\,\mathrm{dB/km}$`.
- `*` and `_` inside `$...$` are safe from Markdown parsing.

## LaTeX Validation Rules

From `merge_formulas.py`:

### Bracket Matching
- Count `{` vs `}` — correctly handle `\{` and `\}` escapes.
- Count `[` vs `]` — correctly handle `\[` and `\]`.
- `\right.` is valid LaTeX (empty right delimiter) — not an error.

### Environment Matching
- Every `\begin{env}` must have a matching `\end{env}`.

### Noise Detection
- `[[object Object]]` — OCR crash artifact.
- >400 characters — likely concatenated formulas.
- Consecutive control sequence pile-up.
- Trailing backslash — possibly truncated formula.

### Validation Scores
- 0 = pass, 1 = warning, 2 = error.

## Format Selection Rules

From `merge_formulas.py`:

| Formula Type | Output | Criteria |
|---|---|---|
| Single-line | `$latex$` | No multi-line environments |
| Multi-line | `![screenshot](path)` | Has `aligned/gathered/cases/split/...` or `\\\\` |
| Single-line, failed validation | `![screenshot](path)` + OCR hint | Bracket mismatch or noise |
| 20+ fragments on one page | Most are OCR debris | PPT fragments subscripts/exponents into separate items |

## Embedding Rules

- **Never** create a standalone "formula appendix" section.
- Embed formulas in the knowledge area they support.
- Core formula table and body text both use `$...$` — the table is a quick reference, the body explains.
- Screenshot references use relative paths: `formula_images/<chapter>_pXXX_fXX.png`.
