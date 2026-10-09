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

- Use lychee through lychee-action, in a template workflow https://github.com/lycheeverse/lychee
  - fast, written in rust
  - can check a variety of different formats and both internal and external links, plut anchors
  - official github action maintained by the same project
  - can be run locally too before pushing
  - configurable with e.g. lychee.toml -- excludes, accepted status codes, timeouts, retries
  - cache can be enabled
  - open source, dual license MIT / Apache-2.0 which means no restrictions to use
  - actively maintained by lycheeverse organisation, latest release date:
- behaviour: on PRs fail on broken links, on scheduled and manual runs fail and update a single issue
- ignore false positives with lychee.toml -- - could use .lycheeignore but we also want to be able to set more configs and lychee.toml both allows ignoring urls and configuring lychee args.
- alternatives and why not chosen
- use badge to show status

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
