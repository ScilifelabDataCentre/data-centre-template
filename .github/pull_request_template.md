<!-- 
Pull Request Template 

What is this: 
A template to help PRs be prepared and reviewed thoroughly and consistently.

What to do: 
Copy this file to your own repository and edit / extend where needed.
The location of the file should be: <your-repository>/.github/pull_request_template.md 

What then?
When you open a PR, the PR description will automatically display the contents of this file. 
-->

## Description

_Use this section to:_

- _Explain why the change is needed_
- _Explain what the PR does_
- _Clarify anything that might be unclear to a reviewer_  
- _Highlight breaking changes if there are any_

### Related issues / tasks

_Mention and / or link to related issues or tasks, e.g. Jira_

### Notes for reviewers

_Optional: Mention anything you especially want reviewers to look at._

## For the author

The following items should be checked **before** marking the PR as **Ready for Review**.
For checklist items that do not apply, add a short note explaining why.

<!-- Optional: If your repository uses labels, add relevant checklist item(s) regarding this. -->

### PR context

- [ ] I've written an informative title, finishing the sentence "This PR will..."
- [ ] I've explained [what this PR does and why](#description)
- [ ] I've mentioned and linked [related issues / tasks](#related-issues--tasks)
- [ ] I've documented breaking changes, risks, assumptions, or open questions, if any

### [Definition of Done](#LINK-TO-MERGED-GUIDELINES)

- [ ] The branch is up to date with `<default-branch>`
- [ ] The PR aligns with the [Acceptance Criteria](#LINK-TO-MERGED-GUIDELINES)
- [ ] All configured checks pass
- [ ] Tests confirm the intended behaviour
- [ ] Unused code and debug leftovers have been removed
- [ ] Naming and structure follow the agreed [style guidelines](#LINK-TO-STYLE-GUIDELINES)
- [ ] The changes made in the PR are documented appropriately

### Self-review

- [ ] I've performed a [self-review](#LINK-TO-MERGED-GUIDELINES) of this PR

## For the reviewer

See the guidelines on [how to review a PR](#LINK-TO-MERGED-GUIDELINES). They offer useful tips that simplify the process for everyone involved.

Use the following checklist during the review.

Review broad to narrow:

- [ ] I understand the purpose of the PR
- [ ] The scope of the PR matches the [Acceptance Criteria](#LINK-TO-MERGED-GUIDELINES)
- [ ] The behaviour matches the description
- [ ] Risks, assumptions, edge cases, and possible side effects have been considered
- [ ] Tests are present and meaningful, or the absence of tests is acceptable
- [ ] The code is clean, readable, maintainable, and follows agreed standards

Once the PR aligns with the Definition of Done and any discussions are resolved, approve the PR.
