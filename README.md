# AAML — AI-Native Arbitration Markup Language

**Version 1.1.1**

A lightweight, open standard for AI-assisted arbitration drafting.

---

## What is AAML?

AAML is a markup format designed to reduce friction between humans and AI when drafting international arbitration submissions.

**The problem:** Current workflows are inefficient. Uploading large PDFs to LLMs wastes context. Copying AI output into Word requires manual reformatting. Every iteration adds friction.

**The solution:** A simple, shared format that both humans and AI can read and write natively — enabling seamless collaboration.

---

## Key Features

- **Open source** — MIT license, free for anyone to use or build upon
- **Model agnostic** — Works with any LLM (Harvey, Legora, ChatGPT, Claude, local models)
- **Platform independent** — No special software required; works in any text editor
- **Human and AI native** — Easy to learn (~10 minutes), easy for LLMs to generate
- **Separates output from production** — Draft in lightweight text, convert to polished PDF/Word when ready
- **Symbolic reference system** — The key innovation that makes AAML more than just Markdown (see below)

---

## Basic Markdown Syntax — The Foundation

AAML is built on Markdown, a lightweight formatting language that takes minutes to learn. If you can write an email, you can write AAML.

### Headings (6 levels)

```markdown
# Level 1 — Main Title
## Level 2 — Major Section
### Level 3 — Subsection
#### Level 4 — Sub-subsection
##### Level 5 — Minor heading
###### Level 6 — Smallest heading
```

In production, these become numbered sections (I, II, III or 1, 2, 3 depending on style).

### Text Formatting

```markdown
**Bold text** — use for emphasis or defined terms

_Italic text_ — use for case names, foreign terms, or quotations

> Block quotes are used for longer citations.
> They can span multiple lines.
> Perfect for quoting treaty provisions or tribunal findings.
```

### Why Markdown?

- **Human-readable** — You can read the source file and understand it immediately
- **AI-native** — Every LLM understands Markdown; it's in their training data
- **Universal** — Works in any text editor, any operating system
- **Lightweight** — No hidden formatting, no corrupted files, no compatibility issues
- **Version-control friendly** — Track changes with Git like code

The syntax table:

| Element | Syntax | Result |
|---------|--------|--------|
| Heading 1 | `# Title` | Main section heading |
| Heading 2 | `## Title` | Subsection |
| Bold | `**text**` | **text** |
| Italic | `_text_` | _text_ |
| Block quote | `> text` | Indented quotation |

This covers 90% of what is needed for arbitration drafting. The remaining 10% — references — is also part of the AAML standard.

---

## The Reference System — AAML's Core Innovation

Standard Markdown has no built-in way to handle legal citations. In arbitration, references are everything: exhibits, legal authorities, witness statements, expert reports, procedural orders. A typical submission might contain thousands of footnotes and cited documents.

**AAML's solution: Symbolic References**

In AAML, you never type exhibit numbers. You reference documents symbolically — by name, ID, or alias. The production engine assigns all numbers automatically and renders citations as properly formatted footnotes.

### Three Ways to Reference Documents

**1. By Filename (simplest — just copy-paste)**

```markdown
The Employer acknowledged the delay. [[2019-04-01_Email_NE_Site_Area_Access_Resolved.md|p. 2]]
```

**2. By Unique ID (short 4-character code)**

```markdown
The technical report confirmed contamination. [[TR03|Section 4.2]]
```

**3. By Alias (natural language)**

```markdown
Under the Contract, access was required within 14 days. [[contract|cl. 8.3]]

The Tecmed tribunal emphasized legitimate expectations. [[tecmed|para. 154]]
```

### Pinpoint Citations

The `|` separator allows you to specify exactly where in the document you're citing:

```markdown
[[document|page 5]]
[[document|para. 42]]
[[document|cl. 8.3(a)]]
[[document|Section IV.B]]
[[document|pp. 12-15]]
```

### What Happens in Production

When you convert AAML to PDF, the production engine:

1. **Resolves** each symbolic reference to the correct document
2. **Assigns** exhibit numbers based on document type and order (C-1, C-2, R-1, CL-1, etc.)
3. **Formats** citations as footnotes with proper legal style
4. **Generates** exhibit lists automatically

**Example — What you write:**

```markdown
The Contractor requested access on multiple occasions. [[2019-02-04_Email_Site_Access_Confirmation.md|p. 1]] The Employer's response was delayed. [[SPCC_BEU_2019_0005_COM.md]]
```

**What the PDF shows:**

> The Contractor requested access on multiple occasions.¹ The Employer's response was delayed.²
>
> _______________
> ¹ Exhibit C-12, Email from Contractor to Employer re Site Access Confirmation, 4 February 2019, p. 1.
> ² Exhibit C-15, Letter from SPCC to BEU, Reference SPCC/BEU/2019/0005/COM.

### Under the Hood - Document Metadata (YAML Headers)

Each document in your library includes a YAML header with metadata:

```yaml
---
uid: C012
title: "Email from Contractor to Employer re Site Access Confirmation"
date: 2019-02-04
type: exhibit
party: claimant
aliases:
  - site access confirmation
  - february access email
---
```

This metadata tells the production engine:
- How to display the document title in footnotes
- What type of document it is (for numbering: C-1, R-1, CL-1, etc.)
- Which party submitted it
- Alternative names for easy referencing

### Why This Matters

The reference system means:

- **No manual numbering** — ever
- **Consistent citations** — across human and AI-drafted text
- **Easy reorganization** — add, remove, or reorder documents without breaking references
- **Multiple output formats** — same source produces PDF with footnotes, web with clickable citations and potentially other formats
- **LLM compatibility** — AI can generate properly referenced text that plugs directly into your submission

---

## Files in This Repository

| File | Description |
|------|-------------|
| `AAML_v1_1_1_Specification.md` | The core specification — syntax, reference management, production rules |
| `AAML_LLM_Prompt_Card_v1_1_1.md` | Instructions to give your LLM for generating valid AAML output |

---

## Quick Start

### Basic Syntax

AAML is based on Markdown with symbolic references:

```markdown
# Claimant's Memorial

## Background

The Claimant is a construction company incorporated in **Germany**.

On 1 March 2018, the parties entered into a Construction Contract. [[Construction Contract.pdf|cl. 8.3]]

> The Contractor shall be granted unrestricted access to the Site
> no later than fourteen (14) days following the Effective Date.

The Employer failed to provide access as required. [[2019-03-29_Email_URGENT_Water_Table.md]]
This caused significant delay to the works. [[TR-007_Programme_Analysis_2019-07-23.md|Section 5]]
```

| Element | Syntax |
|---------|--------|
| Headings | `#` through `######` (6 levels) |
| Bold | `**text**` |
| Italic | `_text_` |
| Block quote | `> quoted text` |
| Reference | `[[document]]` or `[[document\|pinpoint]]` |

### For AI Integration

Upload `AAML_LLM_Prompt_Card_v1_1_1.md` as system instructions. The LLM will output properly formatted AAML with correct reference syntax that you can copy directly into your working document — no reformatting needed.

---

## Two Modes

| Mode | Purpose | Who Uses It |
|------|---------|-------------|
| **Output Mode** | Drafting and developing submissions | Humans, LLMs |
| **Production Mode** | Converting to final deliverables (PDF, Word) | Production engine |

You never manually number paragraphs or format footnotes. That happens automatically in Production Mode.

---

## Who Can Use AAML?

- **Lawyers** — Draft submissions faster with AI assistance
- **Students** — Learn efficient AI-assisted legal drafting (e.g., Vis Moot)
- **Arbitral institutions** — Accept AAML submissions or use for procedural documents
- **Tribunals** — Review submissions, draft procedural orders
- **Vendors** — Build tools and integrations on top of the standard

---

## Live Demo Limitations

The [live converter demo](https://aaml-decoder-production.up.railway.app/) includes a pre-loaded set of **sample exhibits** for testing the reference linking function. This is a demonstration environment — in a real-world implementation, users would connect their own document library.

**To test references, use the filenames from the sample exhibit library below.**

### Sample Exhibit Library

The demo includes ~180 synthetic documents from a fictitious construction arbitration (SPCC Power Plant Project). Document types:

| Category | Prefix | Example Filename |
|----------|--------|------------------|
| Emails | `2019-XX-XX_Email_...` | `2019-03-29_Email_URGENT_Water_Table.md` |
| BEU Letters | `BEU_SPCC_2019_...` | `BEU_SPCC_2019_0005_COM.md` |
| DPI Letters | `DPI_SPCC_2019_...` | `DPI_SPCC_2019_0002.md` |
| SPCC Letters (to BEU) | `SPCC_BEU_2019_...` | `SPCC_BEU_2019_0009_COM.md` |
| SPCC Letters (to DPI) | `SPCC_DPI_2019_...` | `SPCC_DPI_2019_0002_TEC.md` |
| Project Meetings | `PMM-...` | `PMM-009_Minutes_2019-09-30.md` |
| Weekly Meetings | `WPM-...` | `WPM-022_Minutes_2019-07-24.md` |
| Technical Reports | `TR-...` | `TR-008_Geotechnical_Report_2019-08-29.md` |
| Incident Reports | `IR-...` | `IR-2019-008_Crane_Incident_2019-09-18.md` |
| Non-Conformance | `NCR-...` | `NCR-001_GT1_Foundation_Concrete_2019-09-30.md` |

**Example usage in AAML:**

```markdown
The Contractor notified the Employer of unexpected ground conditions. [[TR-003_Unexpected_Subsurface_Conditions.md|Section 4.2]]

This was discussed at the project meeting. [[PMM-004.md|Item 7]]
```

> **Note:** In production use, you would reference your own case documents. The demo library exists only to demonstrate the reference linking functionality.

---

## License

MIT License — free for personal and commercial use.

---

## Links

- [Video introduction]([https://vimeo.com/XXXXX](https://vimeo.com/1162061050?fl=ip&fe=ec))
- [Live converter demo](https://aaml-decoder-production.up.railway.app/)

---

## Contact

Questions or feedback? Email: ldeferrari@whitecase.com

---

*Submitted for the LCIA GAR Hackathon 2026*
