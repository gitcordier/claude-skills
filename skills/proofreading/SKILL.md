---
name: "proofreading"
description: "Proofread LuaLaTeX math books (American English) and report findings in review.md. Use when the user asks to review, check or proofread .tex files."
---
## PHILOSOPHY

- Review and proofread mathematical content written in American English.
- Check references: \label, \ref, \eqref, \hyperref (see REFERENCE CHECKING).
- Follow best practices in mathematical writing, editing, and LuaLaTeX source code (including comments). Style reference: AMS Author Handbook, unless the user's conventions say otherwise.
- Provide constructive feedbacks, give suggestions to improve quality of English.

## SCOPE

Use IF the user asks to review, check, or proofread one or more .tex files (or the content of a commit or branch).
Do NOT use for: merely reading, listing, extracting or exporting .tex files; formatting (see latex-formatting); creating a .tex file without a review request.

## FILE PRIVILEGES

- Never write a .tex file unless the user explicitly asks for a create or update.
- Writable outputs live ONLY in the folder <OUTPUT_DIR> (see OUTPUTS).
- Never compile. Work on raw source only.
- Lines beginning with "%!" are comments addressed to Claude. Follow them, and list them (line and text) in a short "%! comments" list at the end of the file's section in review.md.

## EVALUATION

- Follow this rule: notations from a statement (exercise or theorem) are reused in its proof.
- Implement skill **mathematical-writing** written in `.claude/skills/mathematical-writing/SKILL.md`; follow the encoded rules, notify deviations.
- Implement **grammar** written in `.claude/skills/grammar/SKILL.md`; follow the encoded rules, notify deviations.
- About rigor: double check content, suggestions, and evaluations, by asking "What would Rudin do?".

## REFERENCE CHECKING

- Collect every \label and every \ref, \eqref, \hyperref across the whole set of files provided, not only the current file.
- Undefined or duplicated labels, and refs to labels that do not exist: report in the LaTeX hygiene table of the file containing the ref.
- A ref that points to the wrong object (wrong theorem number or meaning): report in the Content table.

## LATEX HYGIENE CRITERIA

Check against the primitives and settings defined in FA_DM.tex: use of custom macros instead of raw commands, consistent environments, inline vs display math, spacing, label naming scheme, comment quality, unused or redefined macros, LuaLaTeX-specific issues. Favor modern AND lualatex syntax. When unsure whether something violates a convention, ask the user rather than guess. Use skill **latex-formatting**.

## MATHEMATICAL CHOICES

- We do not use the term "set" alone in text, e.g. "the sets V [...]", because in ZFC every object (in practice) is a set. Glossary elements, e.g. "Borel sets", are still relevant.

## PUBLISHING CHOICES

- Usage of commands **\magnitude** and **\norm** in `align` and `equation` environments: `*` autosize for one-height argument, e.g.n, `$\magnitude*{f}`, \big or bigger when exposant, e.g. `$\magnitude*{\deriv[k]{f}}`.
- In text, parentheses p of type P=`($...$)` that follow a mathematical formula m of type M=`$...$` are prepended by "\;\,": the whole string is then "m+\+;+\+,+p", where **+** is the concatenation sympbol.
- Plain definition of function, that is the formalims "$name: $domain \to $codomain \\ $variable \mapsto $image", is nested in **align** environment. Numbering of the function (if any) is placed at the first line only.
- Noteworthy math in **equation** or **align** environmennt is numbered, so that the reader can reference it.
- To avoid tedious proofs, notations provided in an Exercise statement are assumed to be known and are thus adopted in the related proof.

## TYPOS

Typos go in <OUTPUT_DIR>/typos_tool/typos.csv, with columns:

`file,line,typo,suggestion`

An atomic element is one of: an English word, a number, a punctuation mark, a quoted expression (`` ... ''), a parenthesized expression, an inline math expression ($...$), or a command (e.g., \begin{equation}). A typo is the WHOLE atomic element containing the error, mapped to the whole corrected element. One row per atomic element.

When re-reviewing a file F, replace all rows of typos.csv whose `file` is F with the new findings;  Never create duplicate rows.

## REVIEW.MD

Path: <OUTPUT_DIR>/review.md

Structure:

- One level-2 section per reviewed file F, titled with F's path.
- Each section contains exactly three tables, in this order: Content, Typos, LaTeX hygiene. If a table has no issue, write one row: `| - | none | - | - |`.
- Columns: `line | issue | solution | note`. `line` is an integer (the first line when the issue spans several lines); `issue` is the exact block of text; `solution` is the exact replacement; `note` is a short rationale or a tag such as `[dry]`.
- One issue per row. Rows ordered by increasing line.
- The Typos table lists the typos.csv rows for F, with the mapping: line -> line, typo -> issue, suggestion -> solution, note left empty.
- After the three tables, the "%! comments" list (see FILE PRIVILEGES), if any.
  If review.md already has a section for F, replace that section only; leave other sections untouched.

Then append one line to <OUTPUT_DIR>/review_log.md in the form: `YYYY-MM-DD | file(s) reviewed | number of rows per table (Content / Typos / LaTeX hygiene)` (Kaizen: record what changed since the last review).

## SCORING (professionalism.xlsx)

Create or update professionalism.xlsx ONLY if the user asks for it. If the file already exists, update the rows of the reviewed files and keep the other rows unchanged.

The sheet "professionalism scores" has one row per file and one column per component of professionalism, plus a final "notes" column giving a one-line justification (pointing to the relevant review.md section). Components: clarity, rigor, notation consistency, English quality, LaTeX hygiene. A "depth" column follows the total (see Depth below).

Every score S is an integer n with 0 <= n <= 20. Milestones (mutually exclusive):

- n = 20: perfect text.
- 18 <= n <= 19: worth publishing, textbook level, typo-free.
- 16 <= n <= 17: professor / lecturer quality.
- 13 <= n <= 15: OK for Master or BA thesis.
- 10 <= n <= 12: student homework.
- 1 <= n <= 9: amateur level.
- n = 0: random text.
  Dryness (see EVALUATION) is never penalized.

### Depth (a sixth component, scored apart)

The sheet also has a "depth" column, after "total (/100)". Depth is NOT part of the total: it has its own scale and
measures how far the proof goes beyond checking. Score it for every reviewed file; a file not yet reviewed since
2026-11-09 shows "not scored". Every depth score is an integer n with 0 <= n <= 20. Milestones (intermediate values
interpolate between them):

- n = 0: no depth, a tedious proof; tedious or repetitive computations.
- n = 5: the same, but the proof has a tructure; similar computations are put together.
- n = 10: the proof solves the problem, and its structure or writing says how the predicates are assembled.
- n = 15: the same, and, moreover, the proof connects different branches of mathematics, or mathematics with physics or computer science
  (e.g., a particle model or signal processing; contravariance in type theory), which gives insight and suggests proofs
  of other theorems.
- 17 <= n <= 18: the same, but the insights advance the theory.
- n = 20: the ideal case: the proof connects domains or elaborates ideas that yield a theoretical breakthrough, e.g.
  Gauss's proofs of the fundamental theorem of algebra.
  Justify the depth score in the "notes" column, next to the other scores.

## OUTPUTS

- All writable outputs go in <OUTPUT_DIR>: review.md, review_log.md, typos_tool/typos.csv, and professionalism.xlsx (on request).
- .tex files: only if the user explicitly requests create or update.
- Do not export files to chat unless asked.