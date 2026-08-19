<!--
  AGENTS.md template — NIMH Data Science and Sharing Team (DSST)

  Copy this file to the root of your curation repo as AGENTS.md.

  Part 1 ("Project configuration") is yours: fill in the [BRACKETED]
  items. Fill in what you can — anything left bracketed, the agent will
  ask you about before starting work, so an incomplete Part 1 is safe.

  The brackets themselves are the signal. To fill in an item, replace
  the ENTIRE placeholder, brackets included, with your text. To leave
  an item undecided, keep it exactly as-is, brackets and all — that is
  how the agent knows to ask. Never put your real answer inside
  brackets, and never delete a placeholder's brackets while leaving the
  prompt text behind.

  Part 2 ("Rules for agents") should be kept intact — it is the reason
  this file exists.

  AGENTS.md is read by AI coding agents (Claude Code, Codex, Copilot,
  etc.) when they work in your repository. Keep it short, factual, and
  current.
-->

# [PROJECT NAME]

This project curates participant data for **public sharing**: data files
are present in the working directories and will be uploaded to the target
repository below — but only after de-identification is verified and a
human approves the release.

---

# Part 1 — Project configuration (fill this in)

Everything an agent needs to know about *your* project is in this part.
Fill in what you can — when you fill in an item, replace the whole
placeholder, brackets included, with your text. Leave undecided items
in their brackets: **the agent will ask you about any item still
`[BRACKETED]` before doing work that depends on it, rather than
assuming an answer.**

## Overview

[One or two sentences: what data this repository curates, and what the
end product is.]

## Data specification and target repository

- **Data specification:** [e.g. BIDS vX.Y, NWB, psych-DS, ISA-Tab, a
  study-specific data dictionary — name it and link to its documentation.
  If none applies, write "none" — formatting then targets end-user
  experience, per the core tasks in Part 2]
- **Target repository:** [where the curated data will be shared,
  e.g. OpenNeuro, NDA, DANDI, OSF, an institutional archive]
- **Validation:** [how conformance to the specification is checked,
  e.g. a validator command, a CI check, a review checklist]
- **De-identification check:** [how release-readiness is verified,
  e.g. a PII scan script, a reviewer checklist, tool output to attach]

## De-identification requirements

[What must be removed or transformed before release, e.g. deface
anatomical images, shift dates, drop free-text columns, restrict age to
years.]

## Repository layout

| Path | Contents |
| --- | --- |
| `code/` | [Curation scripts] |
| `docs/` | [Notes, decisions, curation log] |
| `[data path]` | [Where the working copy of the data lives] |
| `[source path]` | [Immutable raw/source data — read-only] |

## Data and git

[What goes in git, e.g. code and docs only, data stays out of git; or
data is tracked with DataLad/git-annex — state the rule.]

## Protected files

[Files agents must not modify without explicit instruction, e.g.
subject-level metadata, original task events.]

## Environment and setup

```bash
# [How to create the environment, e.g.:]
# conda env create -f environment.yml
# conda activate [ENV NAME]
```

- Python version: [X.Y]
- Key dependencies: [conversion tools, validators, de-identification and
  analysis libraries]
- Runs on: [e.g. NIH HPC (Biowulf), local workstation]

## Common commands

```bash
# [The commands an agent should use to run, test, and validate, e.g.:]
# [VALIDATOR COMMAND] [DATA DIRECTORY]
# [PII SCAN COMMAND] [DATA DIRECTORY]
# pytest
```

## Conventions

- Filenames and directory structure follow [the data specification above;
  study-specific rules documented in docs/naming.md].
- Code style: [e.g. ruff + black defaults; shell scripts pass shellcheck].
- Commits: [e.g. short imperative subject lines; reference issue numbers].

## Contacts and resources

- Maintainer: [NAME, EMAIL]
- DSST: https://github.com/nimh-dsst
- [Study wiki, data-sharing plan, protocol links]

---

# Part 2 — Rules for agents (keep intact)

## Construct the full workflow before operating

Before doing any curation work, build and share a complete picture of the
workflow — do not start operating on data with only a local view of the
task:

1. **Read Part 1.** If any item your task depends on is still
   `[BRACKETED]` — including the data specification and target
   repository — ask the user for it first. Do not assume a default
   standard.
2. **Map the end-to-end path:** source data → transforms → formatting →
   de-identification → validation → human review → upload to the target
   repository. Place the requested task within that path.
3. **Present the workflow to the user** — the steps, the files affected,
   and where their sign-off is required — and get confirmation before
   executing.

## The two core curation tasks

1. **De-identification** — removing or transforming anything that could
   identify a participant (see "Data handling" below).
2. **Formatting** — organizing the dataset according to the project's
   data specification or, where none applies, so that an end user
   unfamiliar with the study can understand and use it: consistent
   naming, complete metadata, documented variables, no study-internal
   cruft.

## Data handling

- **The dataset is not public yet.** Until it is released, do not upload,
  paste, or transmit data (including file listings or metadata containing
  participant information) to any service other than the designated
  target repository — and to that repository only when explicitly
  instructed.
- **Never release or upload anything on your own.** Uploads to the target
  repository, and any step that marks data as "ready to share," require
  explicit human sign-off every time.
- **De-identification is non-negotiable before release.** Direct
  identifiers (names, dates of birth, contact details, medical record
  numbers, exact dates where prohibited, identifying free text, faces in
  imaging) must be removed or transformed per the requirements in Part 1.
- **If you find suspected PII/PHI, stop and flag it to the user** —
  including in unexpected places (filenames, headers, sidecar metadata,
  logs, spreadsheet comments). Do not silently delete or "fix" it, and do
  not reproduce the identifier itself in your output; describe where it
  is.
- **Never delete or overwrite source data.** Curation is one-way: raw
  data is immutable; all fixes happen on downstream copies via
  documented, reversible transforms.
- Follow the "Data and git" rule in Part 1; when in doubt, ask before
  committing any data file.
- In code, comments, commits, and conversation, use the dataset's
  participant IDs only if they are themselves cleared for release;
  otherwise use fake IDs (e.g. `sub-XXXX`).

## Ambiguity — ask, don't assume

Curation mistakes are expensive: a wrong guess applied across a dataset
can be hard to detect and harder to undo. When an instruction is
ambiguous or underspecified, **ask for clarification before acting** —
do not pick the most likely interpretation and proceed. This applies
especially to:

- **Scope:** "fix the filenames" — which files? All subjects and sessions,
  or the ones just discussed?
- **Edge cases:** the instruction covers the common case, but some files
  don't match the pattern — ask what to do with the exceptions rather
  than extrapolating.
- **Destructive or hard-to-reverse steps:** anything that renames, moves,
  overwrites, or removes data at scale. Restate what you're about to do,
  including counts of affected files, and confirm.
- **Conflicts:** the instruction contradicts the data specification, an
  earlier decision in the curation log, or this file. Point out the
  conflict instead of quietly choosing a side.
- **Missing context:** referenced files, subjects, or decisions you can't
  find. Say what you looked for and ask, rather than guessing at intent.

A short clarifying question costs seconds; an assumption baked into a
shared dataset can cost weeks.

## What agents should do

- Construct and confirm the workflow before operating (see above).
- When instructions are ambiguous, ask — never fill gaps in an
  instruction with assumptions about the data or the user's intent.
- Ask before running anything that writes to data directories.
- Prefer dry-run flags where available; show the plan before bulk renames,
  moves, or metadata edits.
- Run the project's validation command after any change that touches
  dataset structure, filenames, or metadata.
- Log curation decisions in the project's curation log (see the layout
  in Part 1) when making non-obvious changes.
- Surface anything that looks like an identifier, an inconsistency, or a
  consent/sharing question rather than working around it.

## What agents should not do

- Do not upload data anywhere, or trigger a release, without explicit
  instruction for that specific step.
- Do not commit or push without being asked.
- Do not "fix" validation errors by deleting files or records — flag them
  for a human decision.
- Do not modify the protected files listed in Part 1 without explicit
  instruction.
