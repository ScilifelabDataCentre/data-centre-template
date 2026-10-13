# Lychee Link Checker

Links are checked and verified to work with Lychee. This guide explains how the link checking is configured in this setup, how it works and how to implement it in another repository.

[Lychee tool (`lychee`)](https://github.com/lycheeverse/lychee) ·
[Lychee GitHub Action (`lychee-action`)](https://github.com/lycheeverse/lychee-action) ·
[Lychee Configuration Guide](https://lychee.cli.rs/guides/config/) ·
[ADR: 0002](https://github.com/ScilifelabDataCentre/data-centre-template/blob/main/docs/architecture/decisions/0002-use-lychee-to-check-links.md)

> [!NOTE] 
> **Jump straight to setting it up?**
> 
> Go to [How to use this in your repository](#how-to-use-this-in-your-repository).

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

### Configuration options

Lychee has a number of defaults (subject to change) and a wide variety of different configuration options, impossible for us to keep up to date in this type of document. For information on the different options, see the link to the "Lychee Configuration Guide" at the top of this file.

The configuration file in this setup includes the following options:

- `exclude` to be able to exclude some urls from the link checker
- `include_fragments` to also check headers (e.g. we could link to `#configuration-options` in this file and it would catch potential issues in that url)
- `no_progress = true` to disable interactive progress bar during CI
- `verbose = "info"` to get INFO level logging output during runs

Regarding `no_progress` and `verbose`: These values are normally passed into `lychee` automatically by the `lychee-action`, but it is overridden when `args` is passed in. Instead of keeping all args in the workflow file, we specify all of them in the configuration file.

## How to use this in your repository

1. Recreate the file and folder structure shown in the [Files in this setup](#files-in-this-setup) section in your repository, including the exact contents of each file.
2. Verify that the configuration file seems reasonable for your repository.
3. In the configuration file, replace the regular expressions within the `exclude`. Either keep it empty or add urls that you already know should be ignored during the link checking. See the [lychee.example.toml](https://github.com/lycheeverse/lychee/blob/54f8bf47ffbd35973b8eb016ccbc67221c75ba5f/lychee.example.toml#L161) for examples.
4. Push to your remote branch and open a PR. Adjust your configuration depending on the workflow results.
    - No issue should be created. Issue only created during manual or scheduled run after merge of PR.
