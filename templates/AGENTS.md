<!--
  AGENTS.md template — NIMH Data Science and Sharing Team (DSST)

  Copy this file to the root of your curation repo as AGENTS.md and fill in
  the [BRACKETED] placeholders. Delete sections that don't apply, but keep
  the "Data safety" section intact — it is the reason this file exists.

  AGENTS.md is read by AI coding agents (Claude Code, Codex, Copilot, etc.)
  when they work in your repository. Keep it short, factual, and current.
-->

# [PROJECT NAME]

[One or two sentences: what this repository is, what data it curates, and
what the end product is.]

## Data safety — read first

- **Never commit participant data.** No imaging data, phenotype files,
  headers, logs, or console output containing subject identifiers.
  If a file might contain data, it does not belong in git.
- **Never send data to external services.** Do not upload, paste, or
  transmit participant data (including file listings with subject IDs) to
  any API, web service, or third-party tool.
- **Treat data directories as read-only** unless the task is explicitly to
  modify them: [LIST DATA PATHS, e.g. /data/STUDY/rawdata, sourcedata/].
- **Never delete or overwrite source data.** Curation is one-way: raw data
  is immutable; all fixes happen downstream or via documented, reversible
  transforms.
- When showing examples in code, comments, or commits, use fake subject
  IDs (e.g. `sub-XXXX`), never real ones.

## Data specification and target repository

<!--
  Fill these in if the project has decided on them. If you leave them
  as-is, agents will ask you before doing any spec-dependent work.
-->

- **Data specification:** [e.g. BIDS vX.Y, NWB, psych-DS, ISA-Tab, a
  study-specific data dictionary — name it and link to its documentation]
- **Target repository:** [where the curated data will ultimately live,
  e.g. OpenNeuro, NDA, DANDI, OSF, an institutional archive]
- **Validation:** [how conformance to the specification is checked,
  e.g. a validator command, a CI check, a review checklist]

> **Agents:** if any item above is still a `[BRACKETED]` placeholder, ask
> the user which data specification and/or target repository this project
> uses before doing any work that depends on it (naming, directory
> structure, metadata, validation). Do not assume a default standard.

## Repository layout

<!-- Adjust to match your repo. -->

| Path | Contents |
| --- | --- |
| `code/` | [Curation scripts] |
| `docs/` | [Notes, decisions, curation log] |
| `[data path]` | [Where the data lives — usually outside the repo] |

## Environment and setup

```bash
# [How to create the environment, e.g.:]
# conda env create -f environment.yml
# conda activate [ENV NAME]
```

- Python version: [X.Y]
- Key dependencies: [conversion tools, validators, analysis libraries]
- Runs on: [e.g. NIH HPC (Biowulf), local workstation]

## Common commands

```bash
# [The commands an agent should use to run, test, and validate, e.g.:]
# [VALIDATOR COMMAND] [DATA DIRECTORY]
# python code/check_naming.py
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

- Ask before running anything that writes to data directories.
- Prefer dry-run flags where available; show the plan before bulk renames,
  moves, or metadata edits.
- Log curation decisions in [e.g. docs/curation-log.md] when making
  non-obvious changes.

## What agents should not do

- Do not commit or push without being asked.
- Do not "fix" validation errors by deleting files or records — flag them
  for a human decision.
- Do not modify [PROTECTED FILES, e.g. subject-level metadata, original
  task events] without explicit instruction.

## Contacts and resources

- Maintainer: [NAME, EMAIL]
- DSST: https://github.com/nimh-dsst
- [Study wiki, data-sharing plan, protocol links]
