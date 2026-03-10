---
name: writing-labarchive-entries
description: Use when creating lab notebook documentation, LabArchive entries, or detailed records for datasets, analysis pipelines, tools, packages, or educational content. Use when documenting a completed workflow, evaluating a new tool, recording literature notes, or creating reference documentation for a project.
---

# Writing LabArchive Entries

## Overview

Create comprehensive, verbose LabArchive notebook entries in Markdown format. Entries are detailed enough that a reader unfamiliar with the project can understand every decision made, reproduce the work, and build upon it. Use PubMed MCP to back biological claims with literature and to cite source papers for packages and methodologies.

## PubMed MCP — Literature Integration

When an entry involves biological insight, methodological claims, or package citations:

1. **Find supporting literature**: Use `search_articles` with targeted queries
2. **Get citation details**: Use `get_article_metadata` for author, year, journal, DOI
3. **Verify known references**: Use `lookup_article_by_citation` when you have partial citation info
4. **Convert IDs**: Use `convert_article_ids` to get DOIs from PMIDs

Apply PubMed lookups for:
- Biological claims ("HIF-1a is a master regulator of hypoxic response")
- Methodological choices ("SCTransform uses regularized negative binomial regression")
- Package citations ("Seurat v5 introduced layer-based architecture")
- Dataset source papers (always cite the original publication)

## Entry Header

All entry types share this header format. Fill every field — no placeholders.

```markdown
# LabArchive Entry: [Descriptive Title]

**Entry Date:** YYYY-MM-DD
**[Type-specific metadata fields — see templates below]**

---
```

Do **not** include at the end of the document:
- "Last Updated" dates
- "Author" fields
- "Status" fields

Include GitHub links at the end only if a repository exists.

## Entry Types

### A. Reference Dataset

For documenting scRNA-seq datasets, spatial transcriptomics references, or any dataset used as a reference atlas.

**Header metadata:**
```markdown
**Entry Date:** YYYY-MM-DD
**Dataset Accession:** GSE/SRA/ArrayExpress ID
**Publication:** Author et al., Journal Year
**Title:** "Full paper title"
**Role in Pipeline:** How this dataset is used
```

**Required sections:**

1. **Dataset Overview**
   - 1.1 Biological Context — what the dataset represents, why it matters
   - 1.2 Sample Information — table with tissue, donors, cells, platform, depth
   - 1.3 Cell Type Composition — table with types, proportions, markers

2. **Data Acquisition and Format**
   - 2.1 Data Download — source, accession, commands
   - 2.2 File Structure — full directory tree with comments
   - 2.3 Data Format — matrix characteristics, gene IDs, value types

3. **Quality Control Pipeline**
   - 3.1 QC Metrics — table of all metrics calculated
   - 3.2 Outlier Detection — methodology with code and rationale
   - 3.3 Thresholds — table with bounds and cells removed
   - 3.4 Gene Filtration — criteria and counts (if applicable)

4. **Normalization Strategy**
   - Method selection with rationale
   - Parameters with justification table
   - Technology-specific considerations

5. **Dimensionality Reduction**
   - PCA variance analysis
   - UMAP parameters with justification table

6. **Cell Type Annotation**
   - Annotation source and rationale
   - Cell type label table (Label, Full Name, Count)
   - Quality assessment for automated annotations

7. **Reference Export**
   - Conversion to SCE or other formats
   - File paths and verification code

8. **Usage Considerations**
   - Cross-platform compatibility
   - When to use vs alternative references
   - Known limitations

9. **Technical Notes**
   - Version information table
   - Compute requirements
   - Reproducibility notes

10. **References**
    - Numbered list, formatted: Author et al. (Year). Title. *Journal*, Volume:Pages.

### B. Analysis Pipeline

For documenting a workflow built end-to-end (e.g., differential expression, trajectory analysis, integration).

**Header metadata:**
```markdown
**Entry Date:** YYYY-MM-DD
**Project:** Project name
**Pipeline Version:** v1.0
**Input Data:** Description of input
**Output Data:** Description of outputs
```

**Required sections:**

1. **Objective** — what the pipeline achieves and why
2. **Input Data** — source, format, preprocessing state
3. **Pipeline Architecture** — stage overview (numbered list or flow diagram)
4. **Stage Details** — one subsection per stage with:
   - Rationale for the approach
   - Code with self-contained chunks
   - Parameter tables
   - Intermediate outputs
5. **Results Summary** — key findings with tables/figures
6. **Validation** — how results were verified
7. **Technical Notes** — versions, compute, reproducibility
8. **References**

### C. Tool/Package Evaluation

For documenting a new tool, package, or software evaluated for potential use.

**Header metadata:**
```markdown
**Entry Date:** YYYY-MM-DD
**Tool Name:** Package or tool name
**Version:** Specific version evaluated
**Source:** CRAN / Bioconductor / GitHub / PyPI
**Purpose:** What problem it solves
```

**Required sections:**

1. **Overview** — what the tool does, who maintains it, maturity
2. **Installation** — commands, dependencies, potential issues
3. **Core Functionality** — key functions/features with code examples
4. **Integration with Existing Workflow** — how it fits with current tools
5. **Benchmarking/Comparison** — vs alternatives (table format)
6. **Limitations** — known issues, edge cases, missing features
7. **Configuration** — recommended settings for our use case
8. **References** — tool paper, documentation links

### D. Educational/Literature Notes

For documenting insights from papers, courses, tutorials, or conferences.

**Header metadata:**
```markdown
**Entry Date:** YYYY-MM-DD
**Topic:** Subject area
**Source:** Paper citation / Course name / Tutorial URL
**Key Concepts:** Comma-separated list
```

**Required sections:**

1. **Summary** — what was learned, core message
2. **Key Findings** — bullet points with detail
3. **Methodology** — how the authors/instructors approached the problem
4. **Relevance to Current Work** — concrete connections to active projects
5. **Code Examples** — if applicable, working code demonstrating concepts
6. **Open Questions** — what remains unclear or needs follow-up
7. **References** — full citations for all sources

## Writing Style

- **Verbose and detailed**: err on the side of too much information
- **Academic register**: professional scientific tone throughout
- **Tables for structured data**: sample info, parameters, results, comparisons
- **Code blocks with language tags**: always use ` ```r ` for R, ` ```bash ` for shell
- **Numbered sections**: always number top-level (## 1.) and subsections (### 1.1)
- **Bold key terms**: in bullet points, bold the concept then explain
- **Nested bullet points**: key term → explanation → rationale/justification
- **No placeholder text**: every field must be filled with real content
- **Cite everything**: methods, packages, datasets, biological claims

## Reference Formatting

Always use numbered references at the end of the document:

```markdown
## References

1. MacParland SA, et al. (2018). Single cell RNA sequencing of human liver
   reveals distinct intrahepatic macrophage populations.
   *Nature Communications*, 9(1):4383.

2. Hafemeister C, Satija R. (2019). Normalization and variance stabilization
   of single-cell RNA-seq data using regularized negative binomial regression.
   *Genome Biology*, 20:296.
```

Use PubMed MCP to verify citation details before writing them.

## Common Mistakes

| Mistake | Correct Approach |
|---------|-----------------|
| Placeholder text like "[Insert name]" | Fill every field with real content |
| Missing rationale for methods | Every choice needs a "why" |
| Flat bullet points | Use nested hierarchy: term → detail → justification |
| Uncited biological claims | Use PubMed MCP to find supporting references |
| "Last Updated" or "Status" at document end | Only Entry Date in the header |
| Vague file paths | Full directory trees with inline comments |
| Code without context | Precede code blocks with methodology explanation |
