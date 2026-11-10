# claude-skills

A personal collection of Claude Code skills. Each skill is a `SKILL.md` file (YAML frontmatter + instructions) in its own folder under [skills/](skills/).

## Skills

| Skill | Purpose |
|---|---|
| [grammar](skills/grammar/SKILL.md) | Rules for using and punctuating "so", "thus", "therefore", "hence" in English. |
| [latex-formatting](skills/latex-formatting/SKILL.md) | Format LuaLaTeX `.tex` files (line length, indentation, comments, end of file). |
| [mathematical-writing](skills/mathematical-writing/SKILL.md) | Rule graph for writing, refereeing and polishing mathematics (after Knuth, Halmos et al.). |
| [proofreading](skills/proofreading/SKILL.md) | Proofread LuaLaTeX math books (American English); findings go to `review.md`. |
| [python-programming](skills/python-programming/SKILL.md) | Create/update Python code and documentation following best practices. |
| [web-app](skills/web-app/SKILL.md) | Build web apps, scoped to your `<OUTPUT_DIR>` folder. |

## Usage

Copy or symlink a skill folder into `~/.claude/skills/` (user-wide) or `<project>/.claude/skills/` (per project).

## Placeholder

Some skills (web-app, proofreading, python-programming, latex-formatting) use `<OUTPUT_DIR>` as the folder where outputs are written. Replace it with your own folder name before use, e.g. `sed -i '' 's/<OUTPUT_DIR>/my_folder/g' skills/*/SKILL.md`.
