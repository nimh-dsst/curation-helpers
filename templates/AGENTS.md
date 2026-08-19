<!--
  AGENTS.md template — NIMH Data Science and Sharing Team (DSST)

  Copy this file to the root of your curation repo as AGENTS.md and fill in
  the [BRACKETED] placeholders. Delete sections that don't apply, but keep
  the "Data handling" section intact — it is the reason this file exists.

  AGENTS.md is read by AI coding agents (Claude Code, Codex, Copilot, etc.)
  when they work in your repository. Keep it short, factual, and current.
-->

# [PROJECT NAME]

[One or two sentences: what this repository is, what data it curates, and
what the end product is.]

This project curates participant data for **public sharing**. Data files
are present in the working directories and will ultimately be uploaded to
the target repository listed below — but only after de-identification is
verified and a human approves the release.

Curation here means two core tasks, in tension with neither:

1. **De-identification** — removing or transforming anything that could
   identify a participant (see "Data handling").
2. **Formatting** — organizing the dataset according to the data
   specification below or, where no formal specification applies, so that
   an end user unfamiliar with the study can understand and use it:
   consistent naming, complete metadata, documented variables, no
   study-internal cruft.

## Data handling — read first

- **The dataset is not public yet.** Until it is released, do not upload,
  paste, or transmit data (including file listings or metadata containing
  participant information) to any service other than the designated
  target repository — and to that repository only when explicitly
  instructed.
- **Never release or upload anything on your own.** Uploads to the target
  repository, and any step that marks data as "ready to share," require
  explicit human sign-off every time.
- **De-identification is non-negotiable before release.** Direct identifiers
  (names, dates of birth, contact details, medical record numbers, exact
  dates where prohibited, identifying free text, faces in imaging) must
  be removed or transformed before release. Project requirements:
  [LIST THEM, e.g. deface anatomical images, shift dates, drop free-text
  columns, restrict age to years].
- **If you find suspected PII/PHI, stop and flag it to the user** —
  including in unexpected places (filenames, headers, sidecar metadata,
  logs, spreadsheet comments). Do not silently delete or "fix" it, and do
  not reproduce the identifier itself in your output; describe where it
  is.
- **Never delete or overwrite source data.** Curation is one-way: raw
  data is immutable; all fixes happen on downstream copies via
  documented, reversible transforms.
- **What goes in git:** [e.g. code and docs only, data stays out of git;
  or data is tracked with DataLad/git-annex — state the rule]. When in
  doubt, ask before committing any data file.
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

## Data specification and target repository

<!--
  Fill these in if the project has decided on them. If you leave them
  as-is, agents will ask you before doing any spec-dependent work.
-->

- **Data specification:** [e.g. BIDS vX.Y, NWB, psych-DS, ISA-Tab, a
  study-specific data dictionary — name it and link to its documentation.
  If none applies, write "none" — formatting then targets end-user
  experience, per the core tasks above]
- **Target repository:** [where the curated data will be shared,
  e.g. OpenNeuro, NDA, DANDI, OSF, an institutional archive]
- **Validation:** [how conformance to the specification is checked,
  e.g. a validator command, a CI check, a review checklist]
- **De-identification check:** [how release-readiness is verified,
  e.g. a PII scan script, a reviewer checklist, tool output to attach]

> **Agents:** if any item above is still a `[BRACKETED]` placeholder, ask
> the user which data specification and/or target repository this project
> uses before doing any work that depends on it (naming, directory
> structure, metadata, validation, upload preparation). Do not assume a
> default standard.

## Repository layout

<!-- Adjust to match your repo. -->

| Path | Contents |
| --- | --- |
| `code/` | [Curation scripts] |
| `docs/` | [Notes, decisions, curation log] |
| `[data path]` | [Where the working copy of the data lives] |
| `[source path]` | [Immutable raw/source data — read-only] |

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

Always run [VALIDATION COMMAND] after any change that touches dataset
structure, filenames, or metadata.

## Conventions

- Filenames and directory structure follow [the data specification above;
  study-specific rules documented in docs/naming.md].
- Code style: [e.g. ruff + black defaults; shell scripts pass shellcheck].
- Commits: [e.g. short imperative subject lines; reference issue numbers].

## What agents should do

- When instructions are ambiguous, ask — see "Ambiguity" above. Never
  fill gaps in an instruction with assumptions about the data or the
  user's intent.
- Ask before running anything that writes to data directories.
- Prefer dry-run flags where available; show the plan before bulk renames,
  moves, or metadata edits.
- Log curation decisions in [e.g. docs/curation-log.md] when making
  non-obvious changes.
- Surface anything that looks like an identifier, an inconsistency, or a
  consent/sharing question rather than working around it.

## What agents should not do

- Do not upload data anywhere, or trigger a release, without explicit
  instruction for that specific step.
- Do not commit or push without being asked.
- Do not "fix" validation errors by deleting files or records — flag them
  for a human decision.
- Do not modify [PROTECTED FILES, e.g. subject-level metadata, original
  task events] without explicit instruction.

## Contacts and resources

- Maintainer: [NAME, EMAIL]
- DSST: https://github.com/nimh-dsst
- [Study wiki, data-sharing plan, protocol links]
