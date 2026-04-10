---
name: git-hygiene
description: Use when making git commits, writing commit messages, creating branches, or managing version control history. Use when the user asks to commit, when a task is complete and needs committing, or when reviewing commit message style. Also use when rebasing, squashing, or rewriting commit history.
---

# Git Hygiene

## Overview

Conventional Commits format with scoped types, lowercase subjects, and granular atomic commits. One logical change per commit — never bundle unrelated changes.

## Commit Message Format

```
type(scope): lowercase subject line
```

- **No period** at the end of the subject
- **No capitalisation** of the first word after the colon
- Subject line **≤ 72 characters**
- Imperative mood: "add", "fix", "update" — not "added", "fixes", "updated"

### Types

| Type | When to use |
|------|-------------|
| `feat` | New feature or capability |
| `fix` | Bug fix |
| `docs` | Documentation only (README, AGENTS.md, roxygen, vignettes) |
| `style` | Formatting, whitespace, visual changes — no logic change |
| `refactor` | Code restructuring without changing behaviour |
| `chore` | Build, CI, config, dependencies, housekeeping |
| `ci` | CI/CD workflow changes (GitHub Actions, etc.) |
| `test` | Adding or modifying tests |

### Scope (optional)

Parenthetical scope narrows the type to a specific module or area:

```
feat(violin): support metadata columns in do_ViolinPlot()
fix(creating-analysis-projects): correct .gitkeep explanation
refactor(md-to-html): remove knitr engine in favour of native Quarto engine
ci(R-CMD-check): drop oldrel-1 from matrix
```

Omit scope when the change is project-wide or the scope is obvious:

```
docs: add README
chore: bump package version to 1.3.0
```

### Body (optional)

Add a body when the subject alone doesn't explain **why**:

```
style: polish typst formatting and correct citations

- Shorten running head to "BadranSeq"
- Convert Software Design bullet list to flowing prose
- Increase figure heights for UMAP/PCA/scatter plots
- Italicize full figure captions, increase paragraph spacing
```

- Blank line between subject and body
- Body uses bullet points (hyphen prefix), not prose paragraphs
- Each bullet is a discrete change included in the commit

## Branching

- Feature work goes on a **named branch**: `<area>/<description>`
  - Examples: `paper/arxiv-polish`, `feat/featureplot-patchwork`, `feat/lifecycle`
- Merge commits use the branch name: `Merge feat/lifecycle: add lifecycle infrastructure`
- Keep branches short-lived — merge or rebase when done

## Commit Granularity

One logical change per commit. Never bundle:

| Good (atomic) | Bad (bundled) |
|---------------|---------------|
| `chore: add LaTeX build artefacts to .gitignore` | `chore: update gitignore, fix affiliations, and remove template` |
| `fix: correct author affiliation rendering in PDF` | |
| `chore: remove unused typst preprint template` | |

**When in doubt, split.** Three small commits are better than one large commit.

## Pre-Commit Checklist

Before every commit:

1. `git diff --staged` — review exactly what's being committed
2. Verify no secrets, credentials, or `.env` files are staged
3. Run relevant checks (build, lint, test) for the changed files
4. Write the commit message matching the format above

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| `Fix bug` | `fix: correct off-by-one in PCA variance calculation` |
| `Updated README` | `docs: add installation instructions to README` |
| `WIP` | Don't commit WIP — stash or use a branch |
| Capitalised subject | `Fix: ...` → `fix: ...` |
| Period at end | `fix: correct typo.` → `fix: correct typo` |
| Past tense | `fixed`, `added` → `fix`, `add` |
| Bundled changes | Split into atomic commits |

## .gitignore

For most projects, use a **whitelisting approach** to avoid accidentally committing generated files:

```
# Track everything by default
*
# Negate patterns to whitelist specific files/directories
!*/
!.gitignore
!.Rproj
!README.md
!*.py
!*.qmd
!*.md
!pyproject.toml
!uv.lock
```

This is more secure than blacklisting (which can miss new file types). Use blacklisting only when the project has many unpredictable generated files (e.g., LaTeX build artefacts).

### Common Generated Files to Exclude

**Never track these file types:**

| File Type | Pattern | Why Exclude |
|-----------|---------|-------------|
| HTML renders | `*.html` | Generated from Quarto/R Markdown/source files |
| Jupyter checkpoints | `.ipynb_checkpoints/` | Auto-generated save points |
| Rendered documents | `*.pdf`, `*.docx` | Build outputs from source |
| Data files | `*.rds`, `*.h5ad`, `*.csv` (large) | Raw data and checkpoints |
| Virtual environments | `.venv/`, `venv/`, `env/` | Reproducible via requirements |
| IDE files | `.vscode/`, `.idea/`, `*.swp` | User-specific settings |
| OS files | `.DS_Store`, `Thumbs.db` | System-generated |
| Build artifacts | `__pycache__/`, `*.pyc`, `node_modules/` | Compiled/generated code |

**Key principle:** Track the *source* (`.qmd`, `.Rmd`, `.py`), not the *output* (`.html`, `.pdf`). If you can regenerate it, don't version control it.

### Intermediate and Output Directories

**Never track these directories:**

```
# Data and checkpoints (regenerate from source)
checkpoints/
read/
write/
output/
results/

# Generated documents
*.html
*.pdf
*.docx
*.pptx

# Jupyter
.ipynb_checkpoints/
*.ipynb

# Build artifacts
__pycache__/
*.pyc
node_modules/
dist/
build/
```

**Key principle:** If you can regenerate it from source code, don't version control it. Track the recipe, not the product.
