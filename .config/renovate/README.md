# Automatic dependency updates using Renovate

-->

The SciLifeLab Data Centre has a self-hosted instance of [Renovate](https://github.com/ScilifelabDataCentre/k1h-platform-docs/tree/main/renovate). This README details how to use the [custom Renovate preset](./default.jsonc) defined in this template.

<p>
  <a href="https://github.com/ScilifelabDataCentre/k1h-platform-docs/tree/main/renovate">DC-interal Renovate instructions</a> ·
  <a href="https://docs.renovatebot.com/">Renovate documentation</a> ·
  <a href="https://docs.renovatebot.com/configuration-options/#configuration-options">Renovate configuration options</a>
</p>

## TL;DR

**To use the custom Renovate preset for SciLifeLab Data Centre in your repository:** See [How to use the custom preset](#how-to-use-the-custom-preset).

## Files in this setup

```bash
.
├── .github/
│   └── renovate.jsonc # Renovate configuration for this repository
└── .config/
    ├── renovate/
    │   ├── README.md # This file
    │   └── default.jsonc # Custom preset
    └── examples/
        ├── minimal.jsonc # Minimal example
        └── detailed.jsonc # Detailed example
```

| File | Purpose |
| ------ | --------- |
| `.config/renovate/README.md` | This guide |
| `.config/renovate/default.jsonc` | Custom Renovate preset |
| `.config/renovate/examples/minimal.jsonc` | Minimal example of how to use the preset in your repository |
| `.config/renovate/examples/detailed.jsonc` | An example of how to use the preset in your repository and extend if to a specific repository's needs |
| `.github/renovate.jsonc` | Renovate configuration used in this repository |
| `.github/workflows/renovate-validate.yml` | Workflow for validating the Renovate configuration when there's a change in it |

> [!NOTE]
> This configuration uses `.jsonc` in order to allow comments in the JSON files. 

## How to use the custom preset

| Does your repository already have a Renovate configuration file? | |
| --- | --- |
| No | 1. Create a `.github/renovate.jsonc` in your repository. <br/> 2. Copy-paste the contents of [`./examples/minimal.jsonc`](../../.github/renovate.jsonc) into the new file. |
| Yes | 1. Add the preset to the `extends` list (see [`.config/renovate/examples/`](./examples/)). <br/> 2. Remove configuration options from your previous setup if redundant or out of date. |

The changes need to be merged into your repository's default branch for it to take effect.

<!-- 
TO ADD 
  - want local behavior -> add overrides below extends
  - links to available presets your can enable
  - links to available config for packageRules
-->

## What does the preset do?

<!-- 
TO ADD
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

## Examples and overrides

<!--
TO ADD
- what are the examples for, the details are in the example files
-->