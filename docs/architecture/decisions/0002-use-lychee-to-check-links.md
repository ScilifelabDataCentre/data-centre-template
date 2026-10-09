<!--
ADR (Architecture Decision Record) template

This template is copied from the linked template in the development-guidelines repository.
Link to guidelines (latest version): https://github.com/ScilifelabDataCentre/development-guidelines/tree/main/adrs

Name the file NNNN-short-title.md, where NNNN is the next number in line.
    Example: '0001-some-decision.md' exists -> next ADR file becomes '0002-another-decision.md'.
Use the unpadded number in the title.
    Example: '0001-some-decision.md' -> "# 1. Some decision"
-->

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

## Consequences

{{ CONSEQUENCES }} <!-- What becomes easier or more difficult to do because of this change? -->

<!-- Optional section.
Uncomment if relevant for decision and fill with sources.

## References

{{ REFERENCES }}
-->
