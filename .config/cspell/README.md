# Spell checking with CSpell

## What is CSpell?

CSpell is a spell checker for code and documentation. It scans repository files and flags words that are not recognised by the configured languages, dictionaries, ignore patterns, or project-specific word lists.

In this template, CSpell runs automatically on pull requests to help catch spelling mistakes before changes are merged.

Useful links:

- [CSpell GitHub Action (cspell-action)](https://github.com/marketplace/actions/cspell-action)
- [CSpell documentation](https://cspell.org/)
- [Available CSpell dictionaries](https://github.com/streetsidesoftware/cspell-dicts#cspell-dicts)

### Why CSpell?

CSpell is designed for spell checking code repositories, not only documentation. It can check documentation, comments, configuration files, and other repository content, while allowing us to configure it for specific projects and repositories.

CSpell has an official GitHub Action and a large set of dictionaries that can be enabled when needed. Other tools may be better for specific use cases, for example:

- [Vale](https://github.com/vale-cli/vale-action) is a style and writing-rule linter. It is better suited for enforcing style guides, preferred terminology, tone, and wording conventions in documentation and other written text. Vale could be added alongside CSpell if stricter writing-style checks are needed.
- [Codespell](https://github.com/marketplace/actions/codespell-with-annotations) checks code for common misspellings. It is better suited for catching known typo patterns than for dictionary-based spell checking against full language and technical dictionaries.

For this template, CSpell gives the best balance between useful typo detection, configurability, and ease of use in the GitHub Actions.

## Files in this setup

This template uses one GitHub Actions workflow file and one CSpell configuration directory. The configuration files are kept in `.config/cspell` instead of the repository root to keep the repository structured and make the template as easy as possible to use out of the box.

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

| File | Purpose |
| ------ | --------- |
| `.github/workflows/spellcheck.yml` | Runs CSpell |
| `.config/cspell/cspell-config.yml` | CSpell configuration file used by the workflow. Defines languages, dictionaries, ignored patterns and project-specific word lists. |
| `.config/cspell/project-specific-words.txt` | List of valid repository-specific words that are not covered by any available CSpell dictionaries but that CSpell should allow |
| `.config/cspell/README.md` | This guide |

## How this setup works

The workflow explicitly includes some default action settings. The workflow would work the same way without these settings being present, but including them increases clarity and reduces the risk of confusion.

- The spell check runs when a PR is opened or updated
- The workflow is configured to check changed files in the PR
  - `incremental_files_only: true` tells CSpell to only check the PR diff, meaning files changed in the PR. It does not check the rest of the repository.
  - `files: ''` tells CSpell to check all file types selected by the action. This is the _default_.
- The CSpell action uses `.config/cspell/cspell-config.yml` for language, dictionary, ignored patterns, and project-specific-word settings.
  - `language` configures the languages used during the spell check, here British English and Swedish
  - `import` imports dictionaries that need to be installed in the workflow before CSpell runs, in this case Swedish and People Names
  - `caseSensitive` allows CSpell to distinguish between different casing, e.g. SciLifeLab and scilifelab.
  - `dictionaries` list dictionaries from the [`cspell-dicts` repository](https://github.com/streetsidesoftware/cspell-dicts#cspell-dicts) that do not require installation before use. They are bundled with CSpell and are enabled when listed under the `dictionaries` section
  - `dictionaryDefinitions` imports the custom `project-specific-words.txt` as a dictionary. These are words that are not included in any other [CSpell-available dictionary](https://github.com/streetsidesoftware/cspell-dicts#cspell-dicts) but that we consider correct and CSpell should not flag.
  - `ignoreRegExpList` tells CSpell to ignore specific patterns
- If CSpell finds spelling issues, the workflow fails. Spelling issues are reported as GitHub annotations, and suggestions are shown when available.

## What to do when CSpell flags a correct word

If CSpell flags a word that you know is correct, **first** check whether it's already covered by a CSpell dictionary. **Do not** immediately add it to `project-specific-words.txt`.

### Summary

1. Check if the word is found in your CSpell configuration

    - If the word exists in an enabled dictionary, check the spelling, casing and configuration.

2. If the word exists in a bundled dictionary that is not enabled, add it to `dictionaries` in your `cspell-config.yml`.
3. If the word exists in a dictionary that requires installation, install and import the dictionary.
4. If no suitable dictionary covers the word, add it to `project-specific-words.txt`.
5. Verify the result.

### 1. Check if the word is found in your CSpell configuration

Open a terminal window and run the following command in the repository root:

```bash
npx cspell trace --config .config/cspell/cspell-config.yml [YOUR-WORD]
```

#### Example output

```bash
# Word: The word you searched for
# Dictionary: Name of a dictionary that was searched. If there is a '*' next to the name, the dictionary is enabled in your configuration
# F: '*' if the word is found in the dictionary to the right, '-' if the word is not found
Word        F   Dictionary
[YOUR-WORD] -   eng-gb*     # 'eng-gb' is enabled, but [YOUR-WORD] was not found in
[YOUR-WORD] *   sv          # [YOUR-WORD] was found in 'sv', but 'sv' is not enabled
[YOUR-WORD] *   cpp*        # [YOUR-WORD] was found in 'cpp', and 'cpp' is enabled
```

In the last row of the example, `[YOUR-WORD]` was found in a dictionary that is already enabled in your configuration (`cpp`). In this case, CSpell should not be flagging `[YOUR-WORD]` as incorrect. Check spelling, casing and your configuration. >Elaborate here?<

### 2. If the word if found in a dictionary that is not enabled: Add it to `dictionaries` in `cspell-config.yml`


## Notes and limitations

The configuration in this template sets British as the English version because that is the standard at Data Centre. However, 

- en-gb but it does not flag all american spellings due to... plus also the other dicts include american spellings