# Automatic dependency updates using Renovate

1. links
2. What is renovate
3. files in this setup
  - custom preset
  - example

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
