# Lychee Link Checker

Links are checked and verified to work with Lychee. This guide explains how the link checking is configured in this setup, how it works and how to implement it in another repository.

[Lychee tool (`lychee`)](https://github.com/lycheeverse/lychee) ·
[Lychee GitHub Action (`lychee-action`)](https://github.com/lycheeverse/lychee-action) ·
[Lychee Configuration Guide](https://lychee.cli.rs/guides/config/)

> [!NOTE] 
> **Jump straight to setting it up?**
> 
> Go to [#](#)

## Why lychee

https://github.com/ScilifelabDataCentre/data-centre-template/blob/main/docs/architecture/decisions/0002-use-lychee-to-check-links.md

## This configuration 

- config/lychee/lychee-config.toml
- github/workflows/lychee.yml

On PR:
- Fail action if broken links, nothing else
On manual or scheduled run:
- Fail action if broken links and upload results to repo issue (issues must be enabled in repo) 

## How to use it in your repo 

1. Copy-paste this file into your repository: .github/workflows/lychee.yml
2. Optional: Create a 'lychee.toml' file in your repository root.
    - Add urls that the workflow should ignore.
    - See https://github.com/lycheeverse/lychee-action#excluding-links-from-getting-checked
3. Push to your remote branch and open a PR.
    - No issue should be created. Issue only created during manual or scheduled run after merge of PR.
