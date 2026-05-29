# Spell checking with CSpell

## What is CSpell?

<!--
- what it does
- en-gb but it does not flag all american spellings due to... plus also the other dicts include american spellings
- links
- possibly why this is a good choice? 
-->

## Files in this setup

```text
.
├── .github/
│   └── workflows/
│       └── spellcheck.yml
└── .config/
    └── cspell/
        ├── README.md
        ├── cspell-config.yml
        └── project-specific-words.txt
```

<!--
- .github/workflows/spellcheck.yml 
  - Workflow file
  - Runs the spell check when a PR is opened or updated
- .config/ -- why? 
  - Default is in root but can become messy with config files mixed in with the rest
- .config/cspell/ -- why? 
  - Repo becomes more structured 
  - We can add configurations for other workflows etc without confusion
- .config/cspell/README.md -- this file -- aim is to make it as easy as possible for anyone to just use this out of the box
- .config/cspell/cspell-config.yml 
  - Configation used by the workflow file
  - Allows us to specify which languages we want to spell check for and add dictionaries for specific fields that are not included in the default language dictionaries
- .config/cspell/project-specific-words.txt
  - List of words that are not present in any CSpell dictionaries but that we don't want CSpell to flag as incorrect
  - The examples in the file are taken from this repo (data-centre-template) and the development_guidelines repo by running cspell with the above mentioned configuration
  - What do to with the file: expand with your project specific words and remove words where necessary
-->

## How the spell check works

- The spell check runs when a PR is opened or updated
- The workflow is configured to check changed files in the PR
  - The `files: ''` setting keeps the action's default file selection
  - `incremental_files_only: true` limits the check to files changed in the PR.
- The CSpell action uses `.config/cspell/cspell-config.yml` for language, dictionary, ignore-pattern, and project-specific-word settings.
- Some dictionaries are installed in the workflow before CSpell runs. These dictionaries also need to be imported in the CSpell config file.
- If CSpell finds spelling issues, the workflow fails. Spelling issues are reported as GitHub annotations, and suggestions are shown when available.

## What to do when CSpell flags a correct word

<!-- 
What to do if Cspell flags a word that you know is correct:
1. Check if Cspell has a dictionary to import
npx cspell trace --config .config/cspell.json [word]
2. If there's an asterisk (*) next to a dictionary, add the 
-->

### 1. Check whether a dictionary already covers the word

### 2. Add a dictionary if appropriate

### 3. Add the word to project-specific words if no dictionary covers it

## Notes and limitations

