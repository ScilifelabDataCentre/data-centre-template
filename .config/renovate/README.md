# Automatic dependency updates using Renovate

<renovate is activated / available for our org. this files explains how to use the custom renovate preset to make it more useful for us>
<links, to renovate>

1. TLDR; 
  <just copy paste this / replace your existing renovate.json file with this> 
  <if your repo already has a renovate config, add the preset to extends instead of replacing the entire thing>
  <if you want a more detailed example, see here>
  <more information on what this config does: link to below sections>
2. files in this setup
  - custom preset intended for shared use
  - minimal example
  - detailed example
  - readme
3. how to use the custom preset 
  - no existing config --> create renovate.json or renovate.jsonc 
      - copy paste the contents of one of the example files into the file
  - existing config --> add shared preset to extends 
  - want comments --> jsonc
  - want local behavior --> add overrides below extends
  - links to available presets your can enable
  - links to available config for packageRules
4. what the preset does -- what happens if i enable this -- how did i get to these choices? just brief - that this is based on the orgs repos contents
5. what the preset does not do -- doesn't automerge, does not group risky ecosystems. etc.
6. examples and overrides 
- what are the examples for, the details are in the example files

https://docs.renovatebot.com/presets-config/#configrecommended
config:recommended

```jsonc
{
  "extends": [
    ":dependencyDashboard", // Enable Renovate Dependency Dashboard creation.
    ":semanticPrefixFixDepsChoreOthers", // Use semantic commit type fix for dependencies and chore for all others if semantic commits are in use.
    ":ignoreModulesAndTests", // Ignore node_modules, bower_components, vendor and various test/tests (except for nuget) directories.
    "group:monorepos", // Group known monorepo packages together.
    "group:recommended", // Use curated list of recommended non-monorepo package groupings.
    "mergeConfidence:age-confidence-badges", // Show only the Age and Confidence Merge Confidence badges for pull requests.
    "replacements:all", // Apply crowd-sourced package replacement rules.
    "workarounds:all", // Apply crowd-sourced workarounds for known problems with packages.
    "helpers:forgejoDigestChangelogs", // Ensure that every dependency pinned by digest and sourced from Forgejo contains a link to the commit-to-commit diff
    "helpers:giteaDigestChangelogs", // Ensure that every dependency pinned by digest and sourced from Gitea contains a link to the commit-to-commit diff
    "helpers:githubDigestChangelogs", // Ensure that every dependency pinned by digest and sourced from GitHub.com and Github enterprise contains a link to the commit-to-commit diff
    "helpers:gitlabDigestChangelogs", // Ensure that every dependency pinned by digest and sourced from GitLab.com contains a link to the commit-to-commit diff
    "helpers:goXPackagesChangelogLink", // Correctly link to the source code for golang.org/x packages
    "helpers:goXPackagesNameLink", // Link to pkg.go.dev/... for golang.org/x packages' title
    "helpers:renovateChangelog", // Provide a link to octochangelog's improved breakdown for Renovate's changelogs
  ],
}
```

":configMigration", -- seems to be experimental at this point
https://docs.renovatebot.com/configuration-options/#configmigration

abandonments:recommended
https://docs.renovatebot.com/presets-abandonments/#abandonmentsrecommended

https://docs.renovatebot.com/presets-security/#securityminimumreleaseagenpm

https://docs.renovatebot.com/presets-default/#maintainlockfilesweekly

https://docs.renovatebot.com/presets-default/#automergedisabled

https://docs.renovatebot.com/presets-default/#gitsignoff

https://docs.renovatebot.com/configuration-options/#ignoreunstable

https://docs.renovatebot.com/presets-default/#prconcurrentlimit10

https://docs.renovatebot.com/presets-default/#prhourlylimit1

https://docs.renovatebot.com/presets-default/#renovateprefix

https://docs.renovatebot.com/config-overview/#default-config

https://docs.renovatebot.com/faq/#what-is-the-default-behavior
What is the default behavior?¶

Renovate will:

    Look for configuration options in a configuration file (e.g. renovate.json) and in each package.json file
    Find and process all package files (e.g. package.json, composer.json, Dockerfile, etc) in each repository
    Use separate branches/PR for each dependency
    Use separate branches for each major version of each dependency
    Pin devDependencies to a single version, rather than use ranges
    Pin dependencies to a single version if it appears not to be a library
    Update yarn.lock or package-lock.json files, if found
    Create Pull Requests immediately after branch creation

// Available at: https://docs.renovatebot.com/modules/manager/#supported-managers
