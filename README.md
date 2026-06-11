# Template Repository for the SciLifeLab Data Centre

![Status: Work in Progress](https://img.shields.io/badge/status-work%20in%20progress-yellow)

This repository provides example configurations for a set of different tools.

The end goal is for this repository to be populated with examples implementing guidelines detailed in the [SciLifeLab Data Centre's development guidelines repository](https://github.com/ScilifelabDataCentre/development-guidelines).

## Intended repository structure

```text
.
├── .github/        # GitHub-specific config, e.g. workflows and templates
│   ├── workflows/
│   │   └── ...
│   └── ...
├── .config/        # Tool config, one subdirectory per tool
│   ├── <tool-specific-config-1>/
│   │   └── ...
│   └── ...
└── README.md
```

## Repository contents

| Configuration | Purpose | Related guidelines |
|---------------|---------|--------------------|
| [Pull request template](.github/pull_request_template.md) | When a PR is opened, the description is automatically filled with the contents of this file. It helps make pull requests easier to prepare and review. | [PR Guidelines Intro](https://github.com/ScilifelabDataCentre/development-guidelines/pull/54), [Preparing a PR](https://github.com/ScilifelabDataCentre/development-guidelines/pull/61), [Reviewing a PR](https://github.com/ScilifelabDataCentre/development-guidelines/pull/63) |

> Links pointing to PRs will be replaced once PRs in `development_guidelines` repository are merged.
