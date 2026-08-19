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
what the end product is — e.g. "Curation code for the [STUDY] MRI dataset,
converting raw DICOMs to BIDS for sharing on OpenNeuro."]

## Data safety — read first

- **Never commit participant data.** No imaging data, phenotype files,
  DICOM headers, logs, or console output containing subject identifiers.
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
- Key dependencies: [e.g. heudiconv, dcm2niix, pydeface, bids-validator]
- Runs on: [e.g. NIH HPC (Biowulf), local workstation]

## Common commands

```bash
# [The commands an agent should use to run, test, and validate, e.g.:]
# bids-validator [BIDS DIRECTORY]
# python code/check_naming.py
# pytest
```

Always run [VALIDATION COMMAND] after any change that touches dataset
structure, filenames, or metadata.

## Conventions

- Data standard: [e.g. BIDS vX.Y — https://bids-specification.readthedocs.io]
- Filenames and entities follow [e.g. the BIDS specification; study-specific
  rules documented in docs/naming.md].
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
- Do not "fix" validator errors by deleting files or scans — flag them
  for a human decision.
- Do not modify [PROTECTED FILES, e.g. participants.tsv, original task
  events] without explicit instruction.

## Contacts and resources

- Maintainer: [NAME, EMAIL]
- DSST: https://github.com/nimh-dsst
- [Study wiki, data-sharing plan, protocol links]
