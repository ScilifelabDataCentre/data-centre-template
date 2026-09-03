# Automatic dependency updates using Renovate

The SciLifeLab Data Centre has a self-hosted instance of Renovate. This template includes a custom Renovate preset ([`.config/renovate/deault.jsonc`](default.jsonc)) that can be adopted as the Renovate configuration in any repository within the ScilifelabDataCentre GitHub Organisation. This README details how the preset is configured, how to use it, and how to extend it if it needs tightening or loosening to suit a specific repository's needs.

The aim of this template is to:

- Reduce volume of PRs opened by Renovate. They are useful, but they can overwhelm and distract the teams from other tasks.
- Provide a configuration that is useful for most Data Centre repositories, while allowing the teams to extend it where needed.
- Reduce the amount of duplicate work. Currently all teams need to create their own Renovate configurations, which is time- and energy consuming. There are also a lot of configuration options to choose from.

## TL;DR

See [How to use the custom preset in your repository](#how-to-use-the-custom-preset-in-your-repository).

## Useful links

- [DC-internal Renovate instructions](https://github.com/ScilifelabDataCentre/k1h-platform-docs/tree/main/renovate)
- [Renovate documentation](https://docs.renovatebot.com/)
- [Renovate configuration options](https://docs.renovatebot.com/configuration-options/)
- [Default presets (to use in `extends`)](https://docs.renovatebot.com/presets-default/)
- [`packageRules`](https://docs.renovatebot.com/configuration-options/#packagerules)
- [Shareable Config Presets](https://docs.renovatebot.com/config-presets/)

## Files in this setup

<!-- fill this in after the rest is done -->

## How to use the custom preset in your repository

- If the repository has an existing Renovate configuration file, e.g. `renovate.json`: [Update an existing configuration](#update-an-existing-configuration)
- If the repository **does not** have a Renovate configuration file: [Start from scratch](#start-from-scratch)

> [!IMPORTANT]
> Note that the preset should be followed by a version tag (`#1.0.0` in the examples)
>
> What this does:
>
> - Renovate will automatically open a new PR when there's a new version of the preset
> - You can decide if you want to stay on the old version or update to the new one
> - No changes will happen in your repository without you knowing

> [!TIP]
> Aside from the Renovate configuration files explained in the sections below, we also recommend that you copy the [config validation workflow (`.github/workflows/renovate-validate.jsonc`)](../../.github/workflows/renovate-validate.yml) into your repository. See [renovate-validate.yml](#renovate-validate section).

### Start from scratch

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

### Update an existing configuration

Some repositories will already have a Renovate configuration file, e.g. `renovate.json` in the repository root. In this case, the config file will contain something like this:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json"
}
```

In some cases, the config has already been expanded to include presets (`extends`), different options, package-specific rules (`packageRules`) etc, below the `$schema`.

**To use the new custom preset:**

1. Optional but **recommended**: Move/rename the config file to `.github/renovate.jsonc` (`jsonc` suffix to allow comments).
2. Remove the `$schema` line
3. Add `"github>ScilifelabDataCentre/data-centre-template//.config/renovate/default.jsonc#1.0.0"` to `extends`. Your file should now have this structure:

    ```jsonc
    // 
    // Extends the custom preset defined in .config/renovate/default.jsonc
    {
    "extends": [
        "github>ScilifelabDataCentre/data-centre-template//.config/renovate/default.jsonc#1.0.0",
        "<some-other-preset>" // Any presets your repository already lists in extends
    ],
    "<some-option>": "<some-value>", // Any options your repository already set
    "packageRules": ... // Any package rules your repository already set
    }
    ```

4. Remove configuration options from your previous setup if redundant or out of date.

### How to tailor the configuration to the repository needs

The examples/guides in [Start from scratch](#start-from-scratch) and [Update an existing configuration](#update-an-existing-configuration) only show very basic examples of what the configuration could look like. In some cases the teams will need to adjust the Renovate configuration to suit the repository's and teams needs. The section [What the preset does not do](#what-the-preset-does-not-do) shows a couple of examples of what this could entail.

When extending your config file:

- Use the Renovate documentation (see [Useful links](#useful-links) at the top of this file)
- Always leave the DC custom preset at the top of the `extends` list, and the `extends` at the top of the config; Configurations later in the file will override the previous options

## What happens if you use the custom preset in your repository?

Starting with the most obvious point: If you follow the guide(s) in [How to use the custom preset in your repository](#how-to-use-the-custom-preset-in-your-repository), your repository will start using the custom preset configuration built for the SciLifeLab Data Centre. The preset itself uses the SciLifeLab Data Centre self-hosted Renovate instance.

The preset is defined in [`.config/renovate/default.jsonc`](default.jsonc) and contains comments in order to help the reader understand what each line and section does. This section provides more of an overview, while at the same time explaining important details that are left out from and irrelevant to the actual config file.

### "Straight forward" details

- The preset uses the recommended default presets: [`config:recommended`](https://docs.renovatebot.com/presets-config/#configrecommended)
- Packages without any release for one year are flagged as abandoned: [`abandonments:recommended`](https://docs.renovatebot.com/presets-abandonments/#abandonmentsrecommended). The flag normally shows up in the Dependency Dashboard (enabled by default via `config:recommended`), but in our preset, the Dependency Dashboard has been disabled (next item in list).
- The Dependency Dashboard is disabled ([`:disableDependencyDashboard`](https://docs.renovatebot.com/presets-default/#disabledependencydashboard)) since it's a regular GitHub issue and lists all pending Renovate PRs. Leaving this enabled would display all potential vulnerabilities to the public. The exception to this is of course private repositories, but the vast majority of our repositories are public.
- Commit messages in the Renovate PRs get an "Signed-off-by" line: [`:gitSignOff`](https://docs.renovatebot.com/presets-default/#gitsignoff). This is **not** cryptographic commit signing.
- Lockfiles (e.g. `package-lock.json`) are updated weekly (Monday mornings): [`:maintainLockFilesWeekly`](https://docs.renovatebot.com/presets-default/#maintainlockfilesweekly)
- Some configurations are not technically needed and the preset would _currently_ behave the same way even if we were to remove them from the preset. They are included for clarity:
  - All branches opened by Renovate are prefixed with `renovate/`: [`:renovatePrefix`](https://docs.renovatebot.com/presets-default/#renovateprefix).
  - Unstable versions are not updated, unless you're already on an unstable version: [`"ignoreUnstable": true`](https://docs.renovatebot.com/configuration-options/#ignoreunstable).
  - PRs opened by Renovate are **not** automerged; It always waits for a human to review and merge: [`"automerge": false`](https://docs.renovatebot.com/configuration-options/#automerge).

### More complicated explanations

- **When**
  - Our self-hosted Renovate instance runs at a specific schedule.
  - The preset also defines a [`schedule`](https://docs.renovatebot.com/key-concepts/scheduling/#scheduling-syntax), but this does **not** affect when Renovate runs; it only tells Renovate that it's allowed to create new branches between midnight and 06:59 AM (Stockholm time).
  - Renovate is only allowed to create one PR per hour ([`"prHourlyLimit": 1`](https://docs.renovatebot.com/configuration-options/#prhourlylimit)) and only 10 PRs can be open simultaniously ([`"prConcurrentLimit": 10`](https://docs.renovatebot.com/configuration-options/#prconcurrentlimit))
  - Vulnerability PRs bypass all of the rules mentioned above though; vulnerability PRs are created no matter what.
- **npm and PyPI packages**
  - Renovate only updates npm and PyPI packages when they have been released for at least three days. This allows the package authors to potentially fix bugs or retract malicious code, reducing the risk of us merging unsafe code.
  - The npm rule is defined in a Renovate preset: [`security:minimumReleaseAgeNpm`](https://docs.renovatebot.com/presets-security/#securityminimumreleaseagenpm)
  - The PyPI rule **is** a Renovate preset as well, **but** not in our version; our self-hosted instance has Renovate version `43.244.0` at the time of writing, and `security:minimumReleaseAgePyPI` was introduced in version `44.57.1`.
    - This is the reason for us having two `packageRules` for PyPI. The first rule tells Renovate to wait three days for all PyPI updates, and the second tells it to ignore this rule for specific update types since they do not have a "minimum release age" and therefore would never be updated otherwise.
    - When our version is updated to version `44.57.1`, we should replace the `packageRules` with the predefined preset.
- **Labels**
  - Our custom preset labels all PRs made by Renovate with `type: dependency`
  - Security PRs (Renovate reads these from Dependabot) are labelled with `type: security`
  - Major updates are labelled with `update: major`, minor updates are labelled with `update: minor` and patches are labelled with `update: patch`. These are the most common and a more granular labelling configuration is left to each team and repository.
  - GitHub Actions, Python, Docker and npm updates are also labelled.
    - [`matchCategories`](https://docs.renovatebot.com/configuration-options/#packagerulesmatchcategories) is used for Python since it is widely used in our GitHub Organisation and categories let us avoid choosing every specific manager.
    - [`matchManagers`](https://docs.renovatebot.com/configuration-options/#packagerulesmatchmanagers) is used for the others
    - See [Supported Managers](https://docs.renovatebot.com/modules/manager/#supported-managers)
- **Grouping practically excluded**
  - Grouping updates introduces risk: One update per PR leads to easier reviews and a greater chance of finding issues we do not want merged. For this reason, grouping of updates has been practically excluded in the preset and it's up to each team to add it to their Renovate configurations if needed.
  - The preset only groups minor and patch updates for GitHub Actions; Renovate groups these updates into a single PR.
  - All other packages, managers and categories get one PR per update.
  - There's an example of grouping in [`.config/renovate/examples/grouping.jsonc`](./examples/grouping.jsonc) which you can have a look at if you're interested in implementing this into your repositories.

## What the preset does **not** do

The following points are not implemented in the DC Renovate preset. There are likely more options that have been left out, but this mentions two. Both of these also point to example files which you can either use as inspiration or copy-paste into your own configuration.

### 1. **It does not activate digest pinning for GitHub Actions**

Renovate will not update GitHub Action "versions" to specific commits - it will update the version to the version tags, e.g. from `v1` to `v2` or `v1.0.0` to `v1.0.1`. As a result of this, your workflows can and will be affected when there's a push to the main branch of the action, or if you've pinned a major version and they update a minor or patch - for example, `v1.0.1` still points to `v1`. Sometimes these changes will not be noticable, and sometimes the workflows can break without you doing anything, making it difficult to understand why a workflow suddenly fails or behaves differently.

Pinning the actions to an exact commit hash would mean that your workflows would be protected from these types of issues, but it would also make Renovate noisy since it would be opening more updates to pin new updated commits. While grouping might mitigate some of this, this should still be an opt-in configuration.

If you do activate digest pinning, we recommend that you also activate grouping of the digests.

The file [`.config/renovate/examples/digest-pinning.jsonc`](./examples/digest-pinning.jsonc) gives an example on how to activate this in your configuraton.

### 2. **It barely uses grouping**

As explained in [What happens if you use the custom preset in your repository](#more-complicated-explanations), the preset only groups GitHub Actions minor and patch updates. This might make Renovate more noisy for the repositories using the preset as is, but setting a useful grouping strategy that can be reasonably applied to all Data Centre repositories is difficult. While we have set a limit to the number of PRs opened per hour and the number of PRs open simultaneously (which should reduce the volume some), some teams might find it useful to add grouping to their configuration.

See [`.config/renovate/examples/grouping.jsonc`](./examples/grouping.jsonc) for an example on how to add grouping to your configuration.

## Automatic validation of the preset and config

In addition to the custom preset and repository-specific configuration, this template also includes a workflow for automatically validating the different Renovate files: `.github/workflows/renovate-validate.yml`.

> [!TIP]
> While this was initially added for this repository, we recommend that you adopt it in your repository as well, since it will help you catch issues in your configuration if and when you decide to extend it.

### Why

- While we strive to be as thorough as possible, mistakes in the different files are very likely, both due to human error and due to some options and settings only being available after a certain release.
  - The validation workflow uses version `43` by design instead of a specific version, e.g. `43.224.0`. This is to avoid having to update the workflow every time our Renovate version changes and instead leave those updates to when there's a major version update.
- If there are mistakes introduced in the preset, those issues will of course propagate into all repositories that use the preset in the Renovate configuration.

### What it won't catch

The validating workflow will not catch:

- Rules that match nothing
  - The validator checks that e.g. `matchPackageNames` is an array of strings
  - The validator does not check that the strings (e.g. packages) in that array are correct
- Rules overridden by a later rule
- Typos in preset paths
  - `extends` targets are never resolved
- Labels that don't exist in the repository
- Options valid in current Renovate version but removed later
