# 2. Use Lychee to check links

Date: 2026-10-09 <!-- YYYY-MM-DD -->

## Status

Accepted <!-- One of: Accepted, Deprecated, Superseded -->
<!-- If Superseded, add: Superseded by {{ SUPERSEDING_ADR_NUMBER }} -->

## Context

{{ CONTEXT }} <!-- What is the issue that we're seeing that is motivating this decision or change? -->

- Had markdown-link-check in development-guidelines repo
    - was failing all the time, but working again after rerun sometimes
- Didn't have any link checker in this repo
- Common that links break and as our repos grow it's impossible to keep track or to manually debug -- currently often rely on user input or randomly finding non working links
- External links can break without any change to the repo, so checking on PRs alone isn't enough
- Sometimes we want to merge a PR even though a link is not working -- we don't want to loose the information about the broken link

## Decision

{{ DECISION }} <!-- What is the change that we're proposing and/or doing? -->

- Use lychee through lychee-action, in a template workflow https://github.com/lycheeverse/lychee
    - fast, written in rust
    - can check a variety of different formats and both internal and external links, plut anchors
    - official github action maintained by the same project
    - can be run locally too before pushing
    - configurable with e.g. lychee.toml -- excludes, accepted status codes, timeouts, retries
    - cache can be enabled
    - open source, dual license MIT / Apache-2.0  which means no restrictions to use
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
