---
name: naming-conventions
description: Use when renaming files, organising media assets, or establishing file naming standards for any project. Applies to images, documents, data files, and outputs. Enforces kebab-case with descriptive names, ISO 8601 dates, and zero-padded sequential numbering.
---

# Naming Conventions

## Overview

A strict, predictable naming convention for all files. Eliminates ambiguity, sorts correctly in every file manager, and makes batches of files self-describing. Every filename must be readable by both humans and scripts without parsing ambiguity.

**Core rule:** `descriptive-name-YYYY-MM-DD-NNN.ext`

- **kebab-case** only — hyphens as word separators, never spaces or underscores in the descriptive portion
- **ISO 8601 date** — `YYYY-MM-DD` for chronological sorting
- **Zero-padded sequential number** — `001`, `002`, so lexicographic sort matches chronological order
- **Descriptive prefix** — the folder name, project name, or content type that tells you what the file is

## The Pattern

```
<prefix>-<YYYY>-<MM>-<DD>-<NNN>.<ext>
```

| Component | Rule | Example |
|-----------|------|---------|
| `prefix` | Folder name, project slug, or content descriptor | `medicine-gyncology`, `capsular-contracture`, `figure-02` |
| `YYYY-MM-DD` | ISO 8601 date, always hyphens | `2026-05-30` |
| `NNN` | Zero-padded sequential integer, 3 digits minimum | `001`, `002`, `015` |
| `ext` | Lowercase file extension | `jpeg`, `png`, `pdf`, `csv`, `rds` |

### Examples

```
medicine-gyncology-2026-05-30-001.jpeg
medicine-gyncology-2026-05-30-002.jpeg
figure-02-2026-05-30-001.pdf
checkpoint-01-merged-harmony-2026-05-30-001.rds
```

## When to Apply

Apply this convention in these contexts:

- **Bulk media renaming** — WhatsApp downloads, screenshots, camera exports
- **Pipeline outputs** — figures, tables, checkpoints that need versioned filenames
- **Project deliverables** — reports, presentations, data exports
- **Reference collections** — literature PDFs, protocols, datasets

## Rules

### 1. kebab-case for the entire filename (before extension)

- Hyphens (`-`) separate all words and components
- Never spaces, never camelCase, never PascalCase in the filename body
- Underscores are forbidden in filenames — they are reserved for **code identifiers** (R variables, functions), never for files

| Bad | Good |
|-----|------|
| `WhatsApp Image 2026-05-30 at 19.05.22.jpeg` | `medicine-gyncology-2026-05-30-001.jpeg` |
| `figure_02_umap.pdf` | `figure-02-umap-2026-05-30-001.pdf` |
| `01_qc_violin.pdf` | `qc-violin-2026-05-30-001.pdf` |
| `seu_integrated.rds` | `checkpoint-merged-harmony-2026-05-30-001.rds` |

### 2. Date is mandatory for time-ordered collections

Any batch of files where order matters must include the date:

- Photos from an event
- Screenshots from a debugging session
- Daily data dumps
- Iterative figure revisions

If a file is a singleton with no temporal context (e.g., a static reference file), the date may be omitted, but the descriptive prefix and kebab-case remain.

### 3. Zero-pad numbers so lexicographic sort equals numeric sort

Always use enough leading zeros for the largest expected count:

| Batch size | Padding | Examples |
|-----------|---------|----------|
| 1–9 | 3 digits | `001` … `009` |
| 10–99 | 3 digits | `010` … `099` |
| 100–999 | 3 digits | `100` … `999` |
| 1000–9999 | 4 digits | `0001` … `9999` |

Default to 3 digits unless you know the batch will exceed 999.

### 4. Prefix describes content, not source

The prefix should tell you what the file *is*, not where it came from:

| Bad prefix | Good prefix | Why |
|-----------|-------------|-----|
| `whatsapp-image` | `medicine-gyncology` | Source is irrelevant; content matters |
| `img` | `tumour-microenvironment` | Descriptive, not generic |
| `screenshot` | `gsea-enrichment-plot` | What it shows, not how it was captured |

When renaming a folder-full of files, use the **folder name** as the prefix. This creates an instant namespace: every file in `medicine-gyncology/` starts with `medicine-gyncology-`.

### 5. Extension is always lowercase

| Bad | Good |
|-----|------|
| `.JPEG` | `.jpeg` |
| `.PDF` | `.pdf` |
| `.RDS` | `.rds` |

## Renaming Workflow

When asked to rename a batch of files:

1. **Sort chronologically** by timestamp (creation time, EXIF DateTimeOriginal, or embedded timestamp in the original filename)
2. **Determine the prefix** — usually the containing folder name, or a content descriptor if the folder name is generic
3. **Extract or assign the date** — use the file's actual date; if multiple files share the same second, use `(N)` suffixes in the original name to break ties
4. **Number sequentially** from `001` in chronological order
5. **Rename in place** — never move files to a different directory during renaming

### Python reference implementation

```python
import os
import re
from pathlib import Path

def rename_batch(dir_path: Path, prefix: str = None):
    """
    Rename all image files in dir_path to kebab-case convention.
    Prefix defaults to the folder name.
    """
    files = [f for f in dir_path.iterdir() if f.is_file()]

    # Sort by modification time (or parse embedded timestamps)
    files.sort(key=lambda f: f.stat().st_mtime)

    prefix = prefix or dir_path.name.replace(" ", "-").replace("_", "-")
    date_str = "2026-05-30"  # or datetime.now().strftime("%Y-%m-%d")

    for i, old_path in enumerate(files, 1):
        ext = old_path.suffix.lower().lstrip(".")
        new_name = f"{prefix}-{date_str}-{i:03d}.{ext}"
        new_path = dir_path / new_name
        os.rename(old_path, new_path)
        print(f"Renamed: {old_path.name} -> {new_name}")
```

## Common Mistakes

| Mistake | Why it's wrong | Correct |
|---------|---------------|---------|
| `image 1.jpeg` | Spaces break shell scripts and URLs | `image-001.jpeg` |
| `image_1.jpeg` | Underscores are reserved for code identifiers, not filenames | `image-001.jpeg` |
| `1-image.jpeg` | Number at front sorts before letters, mixing numeric and descriptive sorts | `image-2026-05-30-001.jpeg` |
| `image-1.jpeg` | No zero-padding — `image-10.jpeg` sorts before `image-2.jpeg` | `image-001.jpeg` |
| `image_2026_05_30.jpeg` | Underscores in date break consistency and ISO 8601 | `image-2026-05-30.jpeg` |
| `IMG_20260530_001.jpeg` | No separators — unreadable and unparseable | `image-2026-05-30-001.jpeg` |

## Relationship to Other Skills

- `creating-analysis-projects` — same rule: hyphens for filenames (`01-qc-violin.pdf`), underscores for R variables and functions. The two skills agree; this one adds the date and sequence-number pattern for asset collections.
- `md-to-html` — `.qmd` and `.html` filenames use hyphens, matching this convention.
- `tailoring-applications`, `recommendation-letter-workflow` — dated folder and document names use hyphens: `2026-03-20-oxford-snelling/`.
- `hugo-blog` — **documented exception**. Post filenames and `aliases` keep underscores (`tidyverse_to_scverse_part_01.rmd`) because the live site's published URLs and the existing post corpus depend on them. Do not "fix" them.
- `git-hygiene` — commit messages should reference renames as `style: rename images to kebab-case convention`
