# 2. Use Lychee to check links

Date: 2026-10-09 <!-- YYYY-MM-DD -->

## Status

Accepted <!-- One of: Accepted, Deprecated, Superseded -->
<!-- If Superseded, add: Superseded by {{ SUPERSEDING_ADR_NUMBER }} -->

## Context

<!-- What is the issue that we're seeing that is motivating this decision or change? -->

It's impossible to keep track of all links in a repository and manually make sure that all internal and external links continue to work. Without an automated link checker, we rely on others finding and reporting the broken links. Currently, the development-guidelines repository has a link checker ([`markdown-link-check`](https://github.com/tcort/github-action-markdown-link-check)) but fails frequently and has been quite slow. It did sometimes pass on reruns, but not always. This sparked the urge to look into a potential tool switch, that we could also introduce as a template workflow in this repository that others could copy paste.

Even if we chose the most optimal link checker, external links can break without any change to our repository. Therefore it's not enough to run the check only when there's a pull request (PR) opened. Also, while the usual approach should be to fix broken links when found in PRs, there will be cases where we need to merge without a passing link checker. In this case, we risk loosing the information about the broken links and get the same issue on the next open PR.

## Decision

<!-- What is the change that we're proposing and/or doing? -->

### Tool

- Use [`lychee`](https://github.com/lycheeverse/lychee) through the [`lychee-action`](https://github.com/lycheeverse/lychee-action):
  - Open Source, dual license MIT / Apache-2.0 which means there are no restrictions for us to use it
  - Actively maintained by the [lycheeverse GitHub Organisation](https://github.com/lycheeverse) (same as `lychee` itself). Last release 2026-07-09 (3 months prior to writing this ADR).
  - Fast - written in Rust
  - Can check links in a variety of different formats, and both internal and external links
  - Configurable via e.g. the `lychee-config.toml`: Can exclude url patterns, add accepted status codes, enable scanning internal link anchors, add timeouts and retries, etc.
  - Allows caching of results
  - Can be run locally before pushing

### Behaviour

- On opened or updated PR: Fail when there are broken links
- On scheduled and manual runs: Fail and update a single issue in the repository
- Ignore false positives with [`lychee-config.toml`](../../../.config/lychee/lychee-config.toml)

### Alternatives that were not chosen

There are a number of different link checkers. We can not look into absolutely all of them. In this case, the choice stood between switching to Lychee or Linkspector, or to stay with (and improve / reconfigure) markdown-link-check. The points listed below are the main reasons why they were decided against.

- `markdown-link-check`
  - Only checks markdown files - we might need to check links in other file types.
  - Is not maintained as actively, and is a fork of an unmaintained action.
- `Linkspector`
  - Is built for PR review, not scheduled checks.
  - Has narrower file support.

## Consequences

<!-- What becomes easier or more difficult to do because of this change? -->

### Pros

- **General**:
  - Broken links are caught before merge
  - Link rot gets tracked - We won't miss pages getting moved, renamed or deleted, or when whole sites disappear.'
- `lychee-action`:
  - We catch link issues in a variety of different file types
  - We get a detailed job summary by default
  - Fast: Approximately 1 minute for 576 links (according to `lychee-action`'s own docs)
  - We can enable caching if needed
  - Schedules runs open/update an issue when it finds broken links

### Cons

- **General**:
  - PRs can fail on external outages that are out of our control (true for any tool)
  - Repositories implementing this will likely get a failing run initially (anticipated)
- `lychee-action`:
  - If we only want to check files changed in a PR, we have to configure this ourselves. A link breaking anywhere blocks unrelated PRs. This is not covered in the initial setup. If it becomes a problem, we'll have another look, and hopefully there will be an existing tool or solution for it that feels less hacky than what would be needed at the moment.
  - We can't add any inline-ignores like markdown-link-check allows.
  - Opening an issue needs a separate third-party action.

### Additional information

- There will always be an open issue with the latest link checker results
- The workflow would run without a configuration file and we technically would not **need** it.
  - We could either skip the configuration completely and let lychee use the default options, or we could be configuring the tool via `args` in the workflow file.
  - We could also be using the `.lycheeignore` file instead of or as well as the `lychee-config.toml` file, but we want to be able to set more configuration options, and `lychee-config.toml` allows both ignoring patterns and configuring args. <!-- cspell:ignore lycheeignore -->
- If and then this workflow is copied to a different repository, they will not get updates to the template. They can, however, configure Renovate to track new releases of this repository. An example of this can be found in [this repository's Renovate configuration](../../../.github/renovate.jsonc), together with the [`.development-guidelines-version`](../../../.development-guidelines-version) in the repository root.

## References

- `lycheeverse` GitHub Organisation: https://github.com/lycheeverse
- `lychee`, the tool: https://github.com/lycheeverse/lychee
- `lychee-action` GitHub Action: https://github.com/lycheeverse/lychee-action
- `lychee` configuration guide: https://lychee.cli.rs/guides/config/
- Linkspector: https://github.com/UmbrellaDocs/linkspector
- `markdown-link-check` GitHub Action: https://github.com/tcort/github-action-markdown-link-check
