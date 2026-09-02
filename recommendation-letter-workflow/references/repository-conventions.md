# Repository Conventions

## Folder Layout

- `template/` contains the master Quarto source.
- `YYYY-MM-DD-institution/` contains one application package.
- `archived-docx-letters/` stores older `.docx` and `.pdf` versions.
- `supporting-documents/` stores applicant CVs, theses, and other reference material.
- `docs/plans/` stores planning notes and implementation documents.

## Naming

- Use dated folder names: `2026-03-23-protaiomics/`.
- Use matching filenames inside each folder, for example `protaiomics-recommendation-letter.qmd` and `protaiomics-recommendation-letter.pdf`.
- Prefer lowercase, hyphenated filenames (see `naming-conventions`).
- For upload forms, follow the requested naming convention exactly, often `SurnameForenameProgramme`.

## Letter Structure

- Keep a right-aligned date block.
- Keep a recipient block and subject line near the top.
- Use exactly five competency section headings in every letter:
  1. Theoretical Knowledge
  2. Technical Proficiency
  3. Interpretation and Analytical Insight
  4. Independence, Initiative, and Professionalism
  5. Communication Skills
- Follow competency sections with `Overall Assessment`.
- Each competency section must be 3-4 sentences. Overall Assessment: 2-3 sentences.
- Maximum total length: 2 pages. Cut adjectives and redundant framing first when trimming.
- Do not include optional sections or comment blocks in the template or final letters.
- See `references/editorial-guidelines.md` for content quality rules.

## Sign-off

- Include institutional affiliation and department only.
- Do not add contact details unless explicitly requested by the institution.
- No em dashes anywhere in the letter body or header.

## Rendering

- Use `quarto render` to produce the PDF.
- Verify the output before delivery.
- If the environment is missing TeX or fonts, fix the toolchain rather than changing the letter content unless necessary.
