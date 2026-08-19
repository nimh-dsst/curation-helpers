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

This repository prepares participant data for **public release**. The
working directories contain not-yet-public data; no data is uploaded
to the target repository until it has been verified as de-identified
and a human has approved the release.

---

# Part 1 — Project configuration (the user fills this in)

No item here is required. The agent will ask about any item still
`[BRACKETED]` before doing work that depends on it, rather than
assuming an answer — and "none", "no opinion", or "I don't know yet"
are fine answers when it does.

## Overview

[One or two sentences: what was collected and from whom, and what will
be shared publicly once curation is done. Add context that helps the
agent understand the dataset — e.g. cite any associated publication;
its study design, sample size, task descriptions, and variable
definitions are all useful during curation.]

## Data specification and target repository

- **Data specification:** [e.g. BIDS vX.Y, NWB, psych-DS, a study data
  dictionary — name and link it. If none applies, write "none";
  formatting then targets end-user experience, per "The two core
  curation tasks" in Part 2]
- **Target repository:** [e.g. OpenNeuro, NDA, DANDI, Zenodo, an
  institutional archive]
- **Validation:** [how spec conformance is checked — e.g. a validator,
  a CI check, a review checklist; the command goes under "Common
  commands"]
- **De-identification check:** [how release-readiness is verified —
  e.g. a PII scan, a reviewer checklist; the command goes under
  "Common commands"]

## De-identification requirements

[What must be removed or transformed before release, e.g. deface
anatomical images, shift dates, drop free-text columns, restrict age
to years.]

- **Participant IDs cleared for release?** [yes/no — governs whether
  real IDs may appear in code, commits, and conversation, per "Data
  handling" in Part 2]

## Repository layout

| Path | Contents |
| --- | --- |
| `AGENTS.md` | This file, at the repository root |
| `README.md` | High-level overview of the repository's contents |
| `code/` | Curation scripts |
| `logs/` | Curation history: decisions, transforms, flagged anomalies — see "Anomalies and the curation log" in Part 2 |
| `src/` | Original data as received — immutable, read-only |
| `to_upload/` | Working copy where curation happens; what gets uploaded |

The path names above are the DSST convention. If your repo uses
different names, edit the Path column to match; Part 2's references to
`src/` and `to_upload/` always mean whichever directories serve those
roles in your table.

## Data and git

[What goes in git: e.g. code and logs only, data stays out; or data
tracked with DataLad/git-annex.]

## Protected files

[Files agents must not modify without explicit instruction, e.g.
subject-level metadata, original task events.]

## Environment and setup

```bash
# [e.g.:]
# uv sync
# uv run [SCRIPT]
```

- Python: [X.Y]
- Key dependencies: [conversion tools, validators, de-identification
  libraries]
- Runs on: [e.g. NIH HPC (Biowulf), local workstation]

## Common commands

All runnable commands live here.

```bash
# [VALIDATOR COMMAND] to_upload/
# [PII SCAN COMMAND] to_upload/
# [e.g. pytest, conversion or reporting scripts]
```

## Conventions

- Naming and structure: [the data specification above; any
  study-specific rules and where they are documented]
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

Required before any work that modifies data or metadata; read-only
exploration and answering questions are exempt.

1. Read Part 1. If the task depends on an item still `[BRACKETED]`,
   ask the user — never assume a default, especially for the data
   specification or target repository. "None", "no opinion", and
   "I don't know yet" are valid answers — accept them, then propose an
   approach for the user to confirm, or scope the work to what doesn't
   depend on the open item.
2. Map the end-to-end path (typically source data → transforms →
   formatting → de-identification → validation → human review →
   upload; stages may interleave) and place the task within it.
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

- The dataset is not public yet. Do not send data — including file
  listings or metadata with participant information — to any service
  except the target repository, and there only when explicitly
  instructed. Every upload or "ready to share" step requires human
  sign-off.
- De-identification is non-negotiable before release: remove or
  transform direct identifiers (names, birth dates, contact details,
  record numbers, exact dates where the sharing rules prohibit them,
  identifying free text, faces in imaging) per Part 1's requirements.
- If you find suspected PII/PHI — anywhere, including filenames,
  headers, sidecar metadata, log files, spreadsheet comments — stop
  and flag it. Describe its location without reproducing it; never
  silently delete or "fix" it.
- Never delete or overwrite source data (`src/`). Curation is one-way:
  fixes happen on downstream copies via documented, reversible
  transforms.
- Follow Part 1's "Data and git" rule; when in doubt, ask before
  committing any data file.
- Use real participant IDs in code, commits, or conversation only if
  Part 1 says they are cleared for release; otherwise use fake IDs
  (`sub-XXXX`).

## Anomalies and the curation log

- Watch for the unexpected as you work: missing data (subjects,
  sessions, files, or values), out-of-range or impossible values, and
  inconsistencies between the data and its documentation (README, data
  dictionary, an associated publication). Notify the user when you find
  one, and record it in `logs/` so it isn't lost with the conversation.
- `logs/` must hold the complete curation history — every decision and
  every transform, recorded as it happens, so the full path from `src/`
  to `to_upload/` can be reconstructed later. It is a record, not a
  transcript: write concise, human-readable entries (what was done, to
  which files, and why), never pasted conversation text, and when the
  log grows repetitive or disorganized, rewrite it for clarity —
  preserving every decision and transform.

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

- Ask before anything that writes to the data directories (`src/`,
  `to_upload/`), and prefer dry-run flags. Writing to `logs/` needs no
  permission — it's expected.
- Run the project's validation after any change to dataset structure,
  filenames, or metadata.
- Surface consent- or sharing-related questions rather than working
  around them.

Don't:

- Commit or push without being asked.
- "Fix" validation errors by deleting files or records — flag them for
  a human decision.
- Modify Part 1's protected files without explicit instruction.
