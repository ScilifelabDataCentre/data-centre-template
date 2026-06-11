# Template Repository for the SciLifeLab Data Centre

![Status: Work in Progress](https://img.shields.io/badge/status-work%20in%20progress-yellow)

This repository provides example configurations for a set of different tools.

The end goal is for this repository to be populated with examples implementing guidelines detailed in the [SciLifeLab Data Centre's development guidelines repository](https://github.com/ScilifelabDataCentre/development-guidelines).

## Repository structure

```text
.
├── .github/        # GitHub-specific config, e.g. workflows and templates
│   ├── workflows/
│   │   └── ...
│   └── ...
├── .config/        # Tool config, one subdirectory per tool
│   └── cspell/
└── README.md
```

## Repository contents

| Configuration | Purpose | Related guidelines |
|---------------|---------|--------------------|
| [Spell checking configuration](.config/cspell/README.md) | Allows spell checking for files changed in a PR | - |
