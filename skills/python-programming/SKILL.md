---
name: python-programming
description: Programming in Python using PHILOSOPHY. You should use this skill when the user asks to create or update a program or a file as specified in SCOPE. Outputs are specified in OUTPUTS.
---
## PHILOSOPHY

- Create comprehensive Python source code and documentation, including API references and tutorials.
- Follow state-of-the-art best practices in: designing, coding, commenting, documenting, deploying, and providing feedback.

## SCOPE

Use IF:

- developing any app that is not a web app, OR
- the user requests a complex file creation (e.g., an XLSX document).
  OTHERWISE: do NOT use.

## INPUTS

- XLSX: No XLSX document updates: All XLSX documents are deleted and recreated using the Python library openpyxl.

## OUTPUTS

Outputs must be in `<OUTPUT_DIR>`, never outside.
