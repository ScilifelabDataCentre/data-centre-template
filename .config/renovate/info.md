# Automatic dependency updates using Renovate

The SciLifeLab Data Centre has a self-hosted instance of Renovate. This template includes a custom Renovate preset ([`.config/renovate/deault.jsonc`](default.jsonc)) that can be adopted as the Renovate configuration in any repository within the ScilifelabDataCentre GitHub Organisation. This README details how the preset is configured, how to use it, and how to extend it if it needs tightening or loosening to suit a specific repository's needs.

The aim of this template is to:

- Reduce the amount of duplicate work. Currently all teams need to create their own Renovate configurations, which is time- and energy consuming. There are also a lot of configuration options to choose from. 
- Reduce volume of PRs opened by Renovate. They are useful, but they can overwhelm and distract the teams from other tasks.
- Provide a configuration that is useful for most Data Centre repositories, while allowing the teams to extend it where needed

<!--
- the goal is to reduce the amount of work needed for each team and repository
    - if the teams only want those specific settings, they can have a very basic configuration in their repository, pointing to the preset (more details in #how to use it in your repo)
    - if the teams want to override the settings in the preset or add new options, they can add those to their config

- note?
    - while this preset and template is aimed toward the SciLifeLab Data Centre, this repository is public and open source -- anyone can copy and adjust it to their needs
-->

## TL;DR

<!-- 
See # how to use it in your repo 
Fill this in after the rest is done
-->

## Useful links

- [DC-internal Renovate instructions](https://github.com/ScilifelabDataCentre/k1h-platform-docs/tree/main/renovate)
- [Renovate documentation](https://docs.renovatebot.com/)
- [Renovate configuration options](https://docs.renovatebot.com/configuration-options/)
- [Default presets (to use in `extends`)](https://docs.renovatebot.com/presets-default/)
- [`packageRules`](https://docs.renovatebot.com/configuration-options/#packagerules)
- [Shareable Config Presets](https://docs.renovatebot.com/config-presets/)

<!-- are there more useful links? -->

## Files in this setup

<!-- fill this in after the rest is done -->

## How to use the custom preset in your repository

- The repository has an existing Renovate configuration file, e.g. `renovate.json`: #update-the-existing-config
- The repository **does not** have a Renovate configuration file: #start-from-scratch

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
3. Add `"github>ScilifelabDataCentre/data-centre-template//.config/renovate/default.jsonc#1.0.0"` to `extends`. Your file should now begin with this:

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

The preset is defined in `.config/renovate/default.jsonc` and contains comments in order to help the reader understand what each line and section does. This section provides more of an overview, while at the same time explaining important details that are left out from and irrelevant to the actual config file.

### "Straight forward" details

- The preset uses the recommended default presets: [`config:recommended`](https://docs.renovatebot.com/presets-config/#configrecommended)
- Packages without any release for one year are flagged as abandoned: [`abandonments:recommended`](https://docs.renovatebot.com/presets-abandonments/#abandonmentsrecommended). The flag normally shows up in the Dependency Dashboard (enabled by default via `config:recommended`), but in our preset, the Dependency Dashboard has been disabled (next item in list).
- The Dependency Dashboard is disabled ([`:disableDependencyDashboard`](https://docs.renovatebot.com/presets-default/#disabledependencydashboard)) since it's a regular GitHub issue and lists all pending Renovate PRs. Leaving this enabled would display all potential vulnerabilities to the public. The exception to this is of course private repositories, but the vast majority of our repositories are public.
- Commit messages in the Renovate PRs get an "Signed-off-by" line: [`:gitSignOff`](https://docs.renovatebot.com/presets-default/#gitsignoff). This is **not** cryptographic commit signing.
- Lockfiles (e.g. `package-lock.json`) are updated weekly (Monday mornings): [`:maintainLockFilesWeekly`](https://docs.renovatebot.com/presets-default/#maintainlockfilesweekly)
- Some configurations are not technically needed and the preset would _currently_ behave the same way even if we were to remove them from the preset. They are included for clarity:
  - All branches opened by Renovate are prefixed with `renovate/`: [`:renovatePrefix`](https://docs.renovatebot.com/presets-default/#renovateprefix). This is the default and is not technically needed in the preset.
  - Unstable versions are not updated, unless you're already on an unstable version: [`"ignoreUnstable": true`](https://docs.renovatebot.com/configuration-options/#ignoreunstable). This is also the default.
  - PRs opened by Renovate are **not** automerged; It always waits for a human to review and merge: [`"automerge": false`](https://docs.renovatebot.com/configuration-options/#automerge).

----

## default.jsonc -- what happens if you use it in your repo? 

"more complicated explanations"
- when?
    - our DC instance runs at a specific schedule
    - the preset also defines a schedule, but this does not affect when renovate runs
    - the schedule in the preset tells renovate that it's only allowed to create new renovate branches between midnight and 06:59 AM, stockholm time.
    - it's only allowed to create one PR per hour, and only 10 PRs are allowed to be open at the same time 
        - vulnerability PRs (security) bypass this though, they are created no matter what, which is why we've also labeled them
- renovate only bumps npm and pypi packages when they have been released for at least 3 days -- this allows the authors to potentially fix bugs or retract malicious code and it reduces the risk of us merging unsafe code
    - the npm rule is a renovate preset (in extends)
    - the pypi rule is a renovate preset in version 44.???.?? NOT in ours -- we have version 43.224.0 at the time of writing
        - this is why we have the two packagerules for pypi: the first one tells renovate to wait 3 days for all pypi updates, the second tells it to not wait for specific update types because they do not have the "minimum release age" set and would therefore never get a pr without this addition
        - when our renovate instance is bumped to the 44 version that supports the preset for pypi, we should switch to this.
- labels
    - all prs from renovate are marked as `type: dependency`
    - Security PRs (dependabot) are marked as `type: security`
    - semver update labels: major updates are labeled as `update: major`, minor as `update: minor` and patch as `update: patch`.
        - i decided against adding more than these three, these are the most common ones
    - also label github actions, python, docker, npm.
        - I have used a mix of `matchCategories`and `matchManagers`. (links) Managers is more specific, categories more broad, and python is widely used in our org --> categories to not choose every specific manager.
- groups only github-actions minor and patch updates.
    - the reason: riskier to bump multiple at the same time because it's easier to miss issues. with one bump per PR for most packages --> easier to review and catch potential issues
    - the examples/??? show an example of how to activate grouping. you can copy paste those if you want.

## renovate-validate.yml -- automatic validation of the preset, config and examples

- if there are mistakes in the preset, those mistakes will ofc propagate into all repos that use it in their config 
- this was added to avoid mistakes in the configuration
- while this was initially added for this repository, we recommend that you adopt it in your repository as well, since it will help you catch issues in your configuration if and when you decide to extend it
- it uses version 43 for validation instead of the exact version - that is by design, so that we don't need to edit this configuration every time there's a patch or minor bump -- only when there's a major bump to 44.
- what it won't catch: 
    - ??? 

## what the preset does not do

The following points are not implemented in the DC Renovate preset. There are likely more options that have been left out, but this mentions two. Both of these have examples below which you can either use as inspiration or copy-paste into your own configuration.

- it doesn't activate digest pinning for github actions
    - this means that github action prs will bump to specific versions
        - the details depend on what your current settings are e.g. v1 --> v2, or v1.0.0 --> v1.0.1 etc.
    - the prs will not pin digests -- not exact commits for that github action
        - this means that your workflows can and will be affected when there's a change to the action
        - v1.0.1 will still point to v1 
        - sometimes this change won't be noticable
        - sometimes things break even though you have done nothing -- sometimes difficult to understand why a workflow suddenly fails
    - pinning the actions to exact commit hashes means that your actions won't be affected when the action authors push changes
        - instead, when digest pinning is activated, renovate will open a pr to a new digest.
        - this would make it noisy, but grouping of github action prs could mitigate most of it
    - decided that this was an opt in
    - if you decide to activate it, you should also consider activating this [???]

```jsonc
// Example here for activating digest pinning
```

- it only groups github actions minor and patch updates, no other packages or managers or update types
    - this is also opt in
    - it might make it more noisy, but we have (as explained above) set a limit to the number of open prs
    - and setting a useful grouping strategy that should be applied to all repos is difficult
    - instead, the teams should implement themselves if they wish
        - you can use the examples, either as just help or copy paste them if useful

```jsonc
// Example here for activating grouping
```
