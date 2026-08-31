# Automatic dependency updates using Renovate

The SciLifeLab Data Centre has a self-hosted instance of [Renovate](https://github.com/ScilifelabDataCentre/k1h-platform-docs/tree/main/renovate). This README details how to use the [custom Renovate preset](./default.jsonc) defined in this template.

<p>
  <a href="https://github.com/ScilifelabDataCentre/k1h-platform-docs/tree/main/renovate">DC-internal Renovate instructions</a> ·
  <a href="https://docs.renovatebot.com/">Renovate documentation</a> ·
  <a href="https://docs.renovatebot.com/configuration-options/">Renovate configuration options</a>
</p>

## TL;DR

**To use the custom Renovate preset for SciLifeLab Data Centre in your repository:** See [How to use the custom preset](#how-to-use-the-custom-preset).

## Files in this setup

```bash
.
├── .github/
│   ├── renovate.jsonc # Renovate configuration for this repository
│   └── workflows/
│       └── renovate-validate.yml # Automatically validate the configuration files
└── .config/
    └── renovate/
        ├── README.md # This file
        └── default.jsonc # Custom preset
```

| File | Purpose |
| ------ | --------- |
| `.config/renovate/README.md` | This guide |
| `.config/renovate/default.jsonc` | Custom Renovate preset |
| `.github/renovate.jsonc` | Renovate configuration used in this repository |
| `.github/workflows/renovate-validate.yml` | Workflow for validating the Renovate configuration when there's a change in it |

> [!NOTE]
> This configuration uses `.jsonc` in order to allow comments in the JSON files.
> Renovate supports `.jsonc` for both configuration files and presets.

## How to use the custom preset

The changes detailed in the sections below need to be merged into your repository's default branch for it to take effect.

| Does your repository already have a Renovate configuration file? | Go to section... |
| --- | --- |
| No | [From scratch](#from-scratch) |
| Yes | [From an existing configuration](#from-an-existing-configuration) |

### From scratch

> [!IMPORTANT]
> This assumes that your repository does **not** already have a Renovate configuration file.

1. Create a `.github/renovate.jsonc` file in your repository.
2. Copy-paste the following into the new file:
  
    ```jsonc
    // Renovate configuration for this repository
    {
      "extends": [
        "github>ScilifelabDataCentre/data-centre-template//.config/renovate/default.jsonc#1.0.0"
      ]
    }
    ```

### From an existing configuration

> [!IMPORTANT]
> This assumes that your repository already has a Renovate configuration file.

1. Add the preset as the **first** entry of the `extends` list:

    ```jsonc
    // Renovate configuration for this repository
    // Extends the SciLifeLab Data Centre preset
    {
      "extends": [
        "github>ScilifelabDataCentre/data-centre-template//.config/renovate/default.jsonc#1.0.0",
        ":enablePreCommit" // Any presets your repository already lists in 'extends'
      ],
      "prConcurrentLimit": 20 // Any options your repository already set
    }
    ```

    Note that the order matters. The DC preset should come first.

2. Remove configuration options from your previous setup if redundant or out of date.

### Adding your own settings

Put repository-specific options _below_ the `extends` list. Anything you set there wins over the preset.

  ```jsonc
  {
    "extends": [
      "github>ScilifelabDataCentre/data-centre-template//.config/renovate/default.jsonc#1.0.0"
    ],
    "schedule": ["* 0-6 * * 1"], // Only open branches on Mondays (preset sets it to every day)
    // example with pinning digests -- both extends and packagerules
  }
  ```

> [!NOTE]
>
> 1. `packageRules` are additive. Yours are appended after the preset's.
> 2. Use `addLabels` instead of `labels`. `labels` will replace the `labels` settings defined in the preset, `addLabels` will append more labels.

**Useful links:**

- [Configuration options](https://docs.renovatebot.com/configuration-options/)
- [Default presets (to use in `extends`)](https://docs.renovatebot.com/presets-default/)
- [`packageRules`](https://docs.renovatebot.com/configuration-options/#packagerules)
- [Shareable Config Presets](https://docs.renovatebot.com/config-presets/)

## What does the preset do?

<!-- 
TO ADD
  - The preset is referenced by a version tag, which means changes to the preset never reach your repository unannounced. When a new version is tagged, Renovate opens a PR in your repository to bump the version. You review and merge the PR like any other update.
 - what happens if i enable this -- how did i get to these choices? just brief - that this is based on the orgs repos contents
 - default.jsonc has comments with information on what the different configs do but here are some useful links to find out more
-->

## What does the preset not do?

<!-- 
TO ADD
- what does the preset not do?
- things that were decided against
- things they should consider adding e.g. pinning github digests
-->
