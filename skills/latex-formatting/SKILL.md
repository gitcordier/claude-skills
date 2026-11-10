---
name: "latex-formatting"
description: "Format LuaLaTeX .tex files to the user's line-length, indentation, comment and end-of-file rules. Use when the user asks to format a .tex file. Not for proofreading."
---
## PHILOSOPHY

- Format LuaLaTeX documents strictly following the rules in FORMATTING RULES. No other rule applies.
- Formatting NEVER changes meaning or typeset output. When a rule cannot be met without risking that, leave the text as is and report it.
- Formatting is idempotent: formatting an already formatted file changes nothing.

## SCOPE

Use IF the user explicitly asks to format one or more .tex files.
OTHERWISE do NOT use. In particular, do NOT use for proofreading (see the proofreading skill), reading, listing or exporting files.

## FORMATTING RULES

### Specifications: 
The following FORMATTING RULES involve formal specifications encoded in Python-like pseudo-code and predicate calculus.

### File MUST-BE

- Encoding: UTF-8, no BOM.
- Line endings: Unix (LF, `\n`) only.
- No TAB characters.

### End of document MUST

The file ends with exactly these two lines, each terminated by LF, and nothing after them:
  `% END`
  `% `

### File MUST-MATCH

Every line L of a file must satisfy, in this order of precedence (a higher rule overrides a lower one):

1. LINE-END: L ends with LF.
2. LINE-LENGTH: L has at most 128 bytes, includig the terminal byte for LF; see 1.
3. LINE-BEGINNING: IF L begins with an odd number of space(s), THEN round this indentation to the next even integer.
4. LINE-CONTENT:
   4.0. General principle: line break of a line L, which splits L into two lines L_1 (the first) and L_2, must maximize the length of l_1.
      Formally: IF the split occurs after codepoint of index N, so that L_1 == L[0:N+1] + '\n', then

        N:= max{k: len((L[0:k+1] + '\n').encode('utf-8')) <= 128},

      i.e., the largest k such that L[0:k+1] + '\n' satisfies LINE-END and LINE-LENGTH. Bytes are counted, not codepoints;
      for pure ASCII, N = 126. Rule 4.1 may lower N further. Reference implementation: <OUTPUT_DIR>/line_split/line_split.py.
   
   4.1. LINE-CONTENT-COMMENT: L is a comment line IF AND ONLY IF L starts with the character '%'. IF L starts with "%!", THEN L is a comment addressed to Claude. L MUST follows RULES 1, 2, 3. Splitting a commment line $L_0$ (to comply with Rule 2) yields two comment lines L_1 (the first) and L_2. L_2 MUST start with '%'. L_2 MUST NOT start with "%!". A criterion is that IF the splict occurs after codepoint of index N, THEN the following Python implementation MUST return **true**.

  assert L_1 == L_0[0:N+1]+ '\n' and L_2[0] == '%' and  L_2[1:] == L_0[N+1:]

4.2: LINE-CONTENT-REGULAR:

- Scope: IF L IS NOT a comment line, THEN L consists of atomic elements separated by spaces. Atomic elements are: English words, numbers, punctuation, characters with escaping, e.g., "\%", quoted expressions, parenthesized expressions ((...)), inline math , commands with their arguments, e.g., `\begin{equation}` or `\hyperref[...]`. Inside an environment body, a line CAN be broken at any space between terms.
- Rule: Line breaking (IF AND ONLY IF a line violates LINE-LENGTH). Break between atomic elements, at the last space before the limit. In particular no break can occur inside inline math, a command name, the argument of \label, \ref, \eqref or \cite, \verb, or \url. If no safe break exists, leave the line and report it.

## PROCESSING

Rules apply in the given chronological order:

1. File MUST-BE
2. End of document MUST
3. File MUST-MATCH: They are implemeneted apply line per line, from beginning of the file to its end; they apply in the given priority order (1, 2, 3, 4.1, 4.2).

Whenever a rule cannot be followed, skip the line and notice the failure in file review.md (a precise specification of review is out of the scope of the current SKILL).

## VERIFICATION (no compilation)

Never compile. After formatting, check with a script or grep that: there are no TAB characters; no CRLF; no line exceeds 128 bytes, LF included (except the reported exceptions); the file ends with the exact END-OF-DOCUMENT lines; every align-family environment contains `\\`; and, ideally, that a diff shows only whitespace, comment lines, line breaks and appended `\\`. Report any rule that could not be met.

## EDITORIAl CHOICES

- In enumeration environemnts, *e.g.*, **itemize**, an item **\item** may be bracketed or indented for code readability. 

## INPUTS

- .tex files named by the user. Write them ONLY when the user explicitly asks for a create or update.

## OUTPUTS

- The formatted .tex files (in place, unless the user asks for a copy).
- A short summary of what changed and any rule that could not be applied. Do not export files to chat unless asked.
