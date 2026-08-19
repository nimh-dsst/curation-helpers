<!--
  AGENTS.md template — NIMH Data Science and Sharing Team (DSST)
  Source: https://github.com/nimh-dsst/curation-helpers

  Copy this file to your curation repo root as AGENTS.md. AI coding
  agents (Claude Code, Codex, Copilot, etc.) read it when working in
  the repository. Keep it short, factual, and current.

  To fill in a [BRACKETED] item, replace the whole placeholder,
  brackets included. Leave undecided items bracketed — the brackets
  are how the agent knows to ask.
-->

# [PROJECT NAME]

This project curates participant data for **public sharing**: data files
are present in the working directories and will be uploaded to the
target repository below — only after de-identification is verified and
a human approves the release.

---

# Part 1 — Project configuration (the user fills this in)

No item here is required. The agent will ask about any item still
`[BRACKETED]` before doing work that depends on it, rather than
assuming an answer — and "none", "no opinion", or "I don't know yet"
are fine answers when it does.

## Overview

[One or two sentences describing the dataset (what was collected, and
from whom) and what will be shared publicly once curation is done.
Add any context that would help the agent understand
the dataset — for example, if the data supports a publication, cite or
link it: study design, sample size, task descriptions, and variable
definitions drawn from the paper are all useful during curation.]

## Data specification and target repository

- **Data specification:** [e.g. BIDS vX.Y, NWB, psych-DS, a study data
  dictionary — name and link it. If none applies, write "none";
  formatting then targets end-user experience (see Part 2)]
- **Target repository:** [e.g. OpenNeuro, NDA, DANDI, Zenodo, an
  institutional archive]
- **Validation:** [how spec conformance is checked — validator command,
  CI check, review checklist]
- **De-identification check:** [how release-readiness is verified —
  PII scan script, reviewer checklist]

## De-identification requirements

[What must be removed or transformed before release, e.g. deface
anatomical images, shift dates, drop free-text columns, restrict age
to years.]

## Repository layout

| Path | Contents |
| --- | --- |
| `code/` | [Curation scripts] |
| `logs/` | [Notes, decisions, curation log] |
| `raw/` | [Original data as received — immutable, read-only] |
| `to_upload/` | [Working copy where curation happens; what gets uploaded] |

## Data and git

[What goes in git: e.g. code and docs only, data stays out; or data
tracked with DataLad/git-annex.]

## Protected files

[Files agents must not modify without explicit instruction, e.g.
subject-level metadata, original task events.]

## Environment and setup

```bash
# [e.g.:]
# conda env create -f environment.yml
# conda activate [ENV NAME]
```

- Python: [X.Y]
- Key dependencies: [conversion tools, validators, de-identification
  libraries]
- Runs on: [e.g. NIH HPC (Biowulf), local workstation]

## Common commands

```bash
# [VALIDATOR COMMAND] [DATA DIRECTORY]
# [PII SCAN COMMAND] [DATA DIRECTORY]
# pytest
```

## Conventions

- Naming and structure: [the data specification above; study-specific
  rules in logs/naming.md]
- Code style: [e.g. ruff + black; shell scripts pass shellcheck]
- Commits: [e.g. short imperative subjects; reference issue numbers]

## Contacts and resources

- Maintainer: [NAME, EMAIL]
- DSST: https://github.com/nimh-dsst
- [Study wiki, data-sharing plan, protocol links]

---

# Part 2 — Rules for agents (the user keeps this intact)

Here, "you" is the agent; "the user" is the human collaborator.

## Construct the full workflow before operating

1. Read Part 1. If the task depends on an item still `[BRACKETED]`,
   ask the user — never assume a default, especially for the data
   specification or target repository. "None", "no opinion", and
   "I don't know yet" are valid answers: accept them without pressing,
   then propose a sensible approach for the user to confirm, or scope
   the work to what doesn't depend on the open item.
2. Map the end-to-end path — source data → transforms → formatting →
   de-identification → validation → human review → upload — and place
   the task within it.
3. Present the workflow (steps, affected files, sign-off points) and
   get the user's confirmation before executing.

## The two core curation tasks

1. **De-identification** — remove or transform anything that could
   identify a participant.
2. **Formatting** — organize the dataset to the project's data
   specification or, if none, for an end user unfamiliar with the
   study: consistent naming, complete metadata, documented variables,
   no study-internal cruft.

## Data handling

- The dataset is not public yet. Transmit data — including file
  listings or metadata with participant information — to no service
  except the target repository, and there only when explicitly
  instructed. Every upload or "ready to share" step requires human
  sign-off.
- De-identification is non-negotiable before release: remove or
  transform direct identifiers (names, birth dates, contact details,
  record numbers, prohibited dates, identifying free text, faces in
  imaging) per Part 1's requirements.
- If you find suspected PII/PHI — anywhere, including filenames,
  headers, sidecar metadata, logs, spreadsheet comments — stop and
  flag it. Describe its location without reproducing it; never
  silently delete or "fix" it.
- Never delete or overwrite source data. Curation is one-way: raw data
  is immutable; fixes happen on downstream copies via documented,
  reversible transforms.
- Follow Part 1's "Data and git" rule; when in doubt, ask before
  committing any data file.
- Use real participant IDs in code, commits, or conversation only if
  they are cleared for release; otherwise use fake IDs (`sub-XXXX`).

## Ambiguity — ask, don't assume

A wrong guess applied across a dataset is hard to detect and harder to
undo; a clarifying question costs seconds. When an instruction is
ambiguous, ask instead of picking the likeliest interpretation —
especially about:

- **Scope:** "fix the filenames" — all subjects and sessions, or just
  the ones discussed?
- **Edge cases:** files that don't match the stated pattern — ask,
  don't extrapolate.
- **Hard-to-reverse steps:** bulk renames, moves, overwrites, removals
  — restate the plan with affected-file counts and confirm.
- **Conflicts:** the instruction contradicts the data specification,
  the curation log, or this file — point it out instead of quietly
  choosing a side.
- **Missing context:** files, subjects, or decisions you can't find —
  say what you looked for and ask.

## Operating rules

Do:

- Ask before anything that writes to data directories; prefer dry-run
  flags and show the plan before bulk changes.
- Run the project's validation after any change to dataset structure,
  filenames, or metadata.
- Log non-obvious curation decisions in the project's curation log.
- Surface anything that looks like an identifier, an inconsistency, or
  a consent/sharing question rather than working around it.

Don't:

- Upload data or trigger a release without explicit instruction for
  that specific step.
- Commit or push without being asked.
- "Fix" validation errors by deleting files or records — flag them for
  a human decision.
- Modify Part 1's protected files without explicit instruction.
