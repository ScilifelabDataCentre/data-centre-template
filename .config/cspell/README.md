# Spell checking with CSpell

> This guide explains how spell checking is configured in this template, how the setup works and what to do when it incorrectly flags words.

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

summary here

### 1. Check whether the word is covered by an available CSpell dictionary: `cspell trace`

Open a terminal window and run the following command in the repository root:

```bash
npx cspell trace --config .config/cspell/cspell-config.yml [YOUR-WORD]
```

The output is a table with the following headers:

- `Word`: The word you searched for
- `F`: `*` if the word is found in the dictionary to the right, `-` if the word is not found
- `Dictionary`: Name of a dictionary that was searched. If there is a `*` next to the name, the dictionary is enabled in your configuration

The table will contain a variety of different dictionaries that are shipped together with the installed `cspell` package. Some are enabled by default, some need to be enabled before use.

The following sections describe the possible outputs and what you should do in those cases.

#### A. The word is found in enabled dict --> check spelling

Example: `[YOUR-WORD]` was found in a dictionary that **is** enabled by your current CSpell configuration.

```bash
Word        F   Dictionary
[...]
[YOUR-WORD] *   some-dict*  # [YOUR-WORD] was found in 'some-dict', and 'some-dict' is enabled
[...]
```

In this case, CSpell should not be flagging `[YOUR-WORD]` as incorrect.

**What to do:** Check spelling, casing and your configuration. >Elaborate here?<

#### B. The word is found in a non-enabled bundled dict --> see section about enabling bundled dicts

Example: `[YOUR-WORD]` was found in a dictionary that is **not** enabled by your current CSpell configuration. 

```bash
Word         F   Dictionary
[...]
[YOUR-WORDS] *   some-dict   # [YOUR-WORD] was found in 'some-dict', but 'some-dict' is not enabled
[...]
```

**What to do:** [Enable the dictionary](#LINK-HERE) **if** a dictionary containing the word is relevant to your project and makes sense to add.

#### C. The word is not found in any listed dicts --> see section about cspell-dicts repo

Example: `[YOUR-WORD]` was **not** found in any of the dictionaries shipped with `cspell`.

```bash
Word         F   Dictionary
[...]
[YOUR-WORDS] -   some-dict*    # 'some-dict' is enabled, but [YOUR-WORD] was not found in it
[YOUR-WORDS] -   another-dict  # 'another-dict' is not enabled, and [YOUR-WORD] was not found in it
[...]
```

**What to do:** [Search for an installable dictionary](#LINK-HERE).

### 2. The word is found in a non-enabled bundled dict --> Enable the dict (add it to dictionaries in config)

Open your `.config/cspell/cspell-config.yml` file and add the dictionary to `dictionaries`, in alphabetical order.

Example:

```yml
dictionaries:
  - companies
  - cpp-compound-words
  - fonts
  - some-dict  # some-dict is now enabled in the cspell configuration
  [...]
```

### 3. The word is not found in any listed dicts --> go to cspell-dicts repo

If the word was not found in any dictionary shipped together with `cspell`, you can search for the word in CSpell's (long) list of available dictionaries:

1. Go to the [`cspell-dicts` GitHub repository](#LINK-HERE).
2. Search for the word by typing `repo:streetsidesoftware/cspell-dicts [YOUR-WORD]` in the GitHub search field at the top of the page
3. Look for the word in the results. If the correct word is listed in a file with the name format `dictionaries/<some-dict>/dict/<some-dict>.txt`, the word was found in a CSpell-available dictionary. Some words exist in many dictionaries. Pick one that makes sense to add to your configuration.

    - Check if the dictionary is bundled with CSpell or if installation is needed by going back to the [`cspell-dicts` GitHub repository](#LINK-HERE) home page and searching for it (`Ctrl` + `F`). If the dictionary has the `1. Bundled with CSpell` footnote, [enable the dictionary](#LINK-HERE). If not, it needs to be [installed and imported](#LINK-HERE).

    - If there is a choice between one dictionary that is `Bundled with CSpell` and one that is not, both relevant to your project, prefer the bundled one. This keeps the CSpell configuration as simple as possible.

4. If your search results in "Your search did not match any code", the word is not found in any CSpell-available dictionary. Add the word to your `.config/cspell/project-specific-words.txt` file, in alphabetical order.

#### Install and import

CSpell dictionaries are installed by using the package name. You can either see this in the [search results](#LINK-HERE) or you can check the `Package` column in the [`cspell-dicts` README](#LINK-HERE). The package name always starts with `@cspell/dict-` and ends with the dictionary ID (in our example, `<some-dict>`).

1. Install the dictionary locally
2. Import the dictionary in your `.config/cspell/cspell-config.yml`

  ```yml
    import:
    - "@cspell/dict-sv/cspell-ext.json"
    - "@cspell/dict-people-names/cspell-ext.json"
    - "@cspell/dict-<some-dict>/cspell-ext.json"
  ```

3. [Verify that the CSpell configuration finds the word in the installed dictionary](#LINK-HERE)
4. Install the dictionary in the `.github/workflows/spellcheck.yml` workflow file

  ```yml
      - name: Install CSpell Dictionaries (...)
        run: npm install --no-save (...) @cspell/dict-<some-dict>
  ```
5. Import the dictionary in the `.config/cspell/cspell-config.yml` file. Note that the import is for the `.../cspell-ext.json` file in each dictionary. 

  ```yml
  import:
  - "@cspell/dict-sv/cspell-ext.json"
  - "@cspell/dict-people-names/cspell-ext.json"
  - "@cspell/dict-<some-dict>/cspell-ext.json"  # <some-dict> is now imported and therefore enabled in your CSpell config
  ```

6. Push the changes to your remote branch. 
7. If you have an open PR, check the `Files changed`. There should **not** be an annotation for the correct word in the PR diff (`Files changed` tab).

## Notes and limitations

The configuration in this template sets British as the English version because that is the standard at Data Centre. However, 

- en-gb but it does not flag all american spellings due to... plus also the other dicts include american spellings