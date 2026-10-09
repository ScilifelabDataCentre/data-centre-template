# 2. Use Lychee to check links

Date: 2026-10-09 <!-- YYYY-MM-DD -->

## Status

Accepted <!-- One of: Accepted, Deprecated, Superseded -->
<!-- If Superseded, add: Superseded by {{ SUPERSEDING_ADR_NUMBER }} -->

## Context

<!-- What is the issue that we're seeing that is motivating this decision or change? -->

It's impossible to keep track of all links in a repository and manually make sure that all internal and external links continue to work. Without an automated link checker, we therefore rely on others finding and reporting the broken links. Currently, the development-guidelines repository has a link checker (markdown-link-check) but fails frequently and has been quite slow. It did sometimes pass on reruns, but not always, and it started to feel like we needed something more conistent and reliable, that we could also introduce as a template workflow in this reposity that others could copy paste.

Even if we chose the most optimal link checker, external links can break without any change to our repository. Therefore it's not enough to run the check only when there's a pull request (PR) opened. Also, while the usual approach should be to fix broken links when found in PRs, there will be cases where we need to merge without a passing link checker. In this case, we risk loosing the information about the broken links and get the same issue on the next open PR.

## Decision

<!-- What is the change that we're proposing and/or doing? -->

### Tool

- Use [`lychee`](https://github.com/lycheeverse/lychee) through the [`lychee-action`](https://github.com/lycheeverse/lychee-action):
  - Open Source, dual license MIT / Apache-2.0 which means there are no restrictions for us to use it
  - Actively maintained by the [lycheeverse GitHub Organisation](https://github.com/lycheeverse) (same as `lychee` itself). Last release 2026-07-09 (3 months prior to writing this ADR).
  - Fast - written in Rust
  - Can check links in a variety of different formats, and both internal and external links
  - Configurable via e.g. the `lychee.toml`: Can exclude url patterns, add accepted status codes, enable scanning internal link anchors, add timeouts and retries, etc.
  - Allows caching of results
  - Can be run locally before pushing

### Behavior

- On opened or updated PR: Fail when there are broken links
- On scheduled and manual runs: Fail and update a single issue in the repository
- Ignore false positives with [`lychee.toml`](../../../lychee.toml)
  - We could also use `.lycheeignore` but we want to be able to set more configuration options, and `lychee.toml` allows both ignoring patterns and configuring args.

### Alternatives that were not chosen

- alternatives and why not chosen

## Consequences

{{ CONSEQUENCES }} <!-- What becomes easier or more difficult to do because of this change? -->

### Pros

    - Broken links are caught before merge
    - Link rot gets tracked: Pages get moved, renamed or deleted. Whole sites disappear.

### Cons

    - PRs can fail on external outages that are out of our control (true for any tool)
    - Repositories implementing this will likely get a failing run initially (anticipated)

### Additional information

- There will always be an open issue with the latest link checker results
- We will need to have `lychee.toml` in the root, which is not ideal since we want to keep the root as clean as possible. It is possible to configure to a different directory, but that would potentially complicate the rest of the tool flow and configuration.
- If and then this workflow is copied to a different repository, they will not get updates to the template. They can, however, configure Renovate to track new releases of this repository. An example of this can be found in [this repository's Renovate configuration](../../../.github/renovate.jsonc), together with the [`.development-guidelines-version`](../../../.development-guidelines-version) in the repository root.

<!-- Optional section.
Uncomment if relevant for decision and fill with sources.

-->

## References

https://github.com/lycheeverse
https://github.com/lycheeverse/lychee
https://github.com/lycheeverse/lychee-action
https://lychee.cli.rs/guides/config/
