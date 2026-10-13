# Lychee Link Checker

Links are checked and verified to work with Lychee. This guide explains how the link checking is configured in this setup, how it works and how to implement it in another repository.

[Lychee tool (`lychee`)](https://github.com/lycheeverse/lychee) ·
[Lychee GitHub Action (`lychee-action`)](https://github.com/lycheeverse/lychee-action) ·
[Lychee Configuration Guide](https://lychee.cli.rs/guides/config/) ·
[ADR: 0002](https://github.com/ScilifelabDataCentre/data-centre-template/blob/main/docs/architecture/decisions/0002-use-lychee-to-check-links.md)

> [!NOTE] 
> **Jump straight to setting it up?**
> 
> Go to [#](#)

## Files in this setup

This template uses one GitHub Actions workflow file and one Lychee configuration file ([`.config/lychee/lychee-config.toml`](lychee-config.toml)). For information about why Lychee was chosen, see the `ADR: 0002` link above.

```text
.
├── .github/
│   └── workflows/
│       └── lychee.yml
└── .config/
    └── lychee/
        ├── README.md
        └── lychee-config.toml
```

| File                                                               | Purpose                                                                                                                            |
| ------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------- |
| `.github/workflows/lychee.yml`                                     | Runs the lychee link checker                                                                                                                        |
| `.config/cspell/lychee-config.toml`                                 | Lychee configuration file used by the workflow |
| `.config/cspell/README.md`                                         | This guide                                                                                                                         |

## What does the workflow do?

The lychee link checker runs on different events and behaves differently depending on when the workflow is run.

| When | lychee </br> run? | Result uploaded </br> as artefact? | Result uploaded to </br> GitHub issue? |
| --- | --- | --- | --- |
| PR is opened / updated | :white_check_mark: | :x: | :x: |
| Scheduled run: Every day at midnight (UTC) | :white_check_mark: | :white_check_mark: | :white_check_mark: |
| On manual trigger | :white_check_mark: | :white_check_mark: | :white_check_mark: |



## How to use it in your repo 

1. Copy-paste this file into your repository: .github/workflows/lychee.yml
2. Optional: Create a 'lychee.toml' file in your repository root.
    - Add urls that the workflow should ignore.
    - See https://github.com/lycheeverse/lychee-action#excluding-links-from-getting-checked
3. Push to your remote branch and open a PR.
    - No issue should be created. Issue only created during manual or scheduled run after merge of PR.
