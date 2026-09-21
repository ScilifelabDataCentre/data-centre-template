# Architecture Decision Records (ADRs)

This directory contains a copy-pastable template for architecture decision records (ADRs), aligned with the SciLifeLab Data Centre [development guidelines](https://github.com/ScilifelabDataCentre/development-guidelines/tree/main/adrs).

> [!IMPORTANT]
> Once accepted and merged, an ADR is not rewritten. Only the status of an ADR changes. See [Changing the status of an ADR](#changing-the-status-of-an-adr).

## How to add an ADR

1. Create a copy of the template file `adr-template.md`.
2. Name the file according to the instructions in the template file.
3. Replace the placeholders with information regarding your decision. Read the comments in the file for hints and instructions on what the sections should contain.
4. Push to your remote branch and open a PR to your default branch.

## Changing the status of an ADR

| From     | To         | When | Note                                                          |
| -------- | ---------- | ---- | ------------------------------------------------------------- |
| Proposed | Accepted   | ...  | ...                                                           |
| Proposed | Rejected   | ...  | ...                                                           |
| Accepted | Deprecated | ...  | ...                                                           |
| Accepted | Superseded | ...  | Always write the new ADR first, when point the old ADR at it. |
