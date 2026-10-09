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

pros:

- broken links caught before merge
- link rot gets tracked

cons:

- PRs can fail on external outages
- existing repos may fail on their first run
- don't want lychee.toml or .lycheeignore in root really but the alternative here is to configure a different working directory in the workflow and we don't want to over complicate
- copied worfklows won't get template updates, but they can configure this with renovate

neutral:

- there's an open issue

<!-- Optional section.
Uncomment if relevant for decision and fill with sources.

## References

{{ REFERENCES }}
-->
