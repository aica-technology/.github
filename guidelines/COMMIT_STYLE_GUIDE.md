# Commit message format

All commits on `main` branches must follow a format for the commit message (subject, body and optional footers)
according to the [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/#summary) specification. The basic
convention is explained and extended in the following section.

## Commit type

We define the following types as supported by
[commitlint/config-conventional](https://github.com/conventional-changelog/commitlint/tree/master/%40commitlint/config-conventional#type-enum),
which in turn are derived from [Angular](https://github.com/angular/angular/blob/main/CONTRIBUTING.md#type):

- `build:` change the build system or external dependencies (for example, CMakeLists.txt, Dockerfile, package.xml)
- `ci:` change CI configuration files and scripts (for example, GitHub actions and workflows)
- `chore:` perform any general maintenance or work that is not accurately described by any other type
- `docs:` change only documentation lines or files
- `feat:` introduce, enhance or modify a feature
- `fix:` resolve an issue, bug, or error
- `perf:` improve performance without changing existing features or functionality
- `refactor:` change code structure or organization without changing existing features or functionality
- `release:` change only the package version and changelog to mark a new release
- `revert:` rollback a previous commit without making any other changes
- `style:` format / reformat code to adhere to style guidelines
- `test:` introduce, enhance or modify unit tests

## Commit scope

The commit scope is optional and can be used to categorize the change. It should preferably be a single noun but may be
a short phrase. For example,

- `feat(api): <description>`
- `fix(graph): <description>`
- `docs(component services): <description>`

For multi-package repositories, if a commit only changes a single package, the package name can be used as the commit
scope. For example, `feat(base-components)` would indicate a feature in the `base_components` package.

## Breaking changes

If a commit includes breaking changes to the ABI or API of the package output, this must be clearly marked by adding an
exclamation point `!` after the commit type (or after the scope, if there is one). In addition, the commit body should
include a `BREAKING CHANGE:` description to clearly summarize the effects of the change.

## Formatting

Some additional formatting guidelines should also be respected:

- The subject should be a single lowercase line, ending with the respective PR number in parentheses
- Prefer hyphens to underscores when using the package name as the commit scope
- The description in the subject should be in the imperative mood.
- Sentences in the body should be capitalized and punctuated
- The body should be written in paragraphs of proper sentences starting in the imperative
- Paragraphs in the body should be separated by a new line
- The line width of all commit messages should be limited to 72 characters, with line breaks added in sentences as
  needed

## General guidelines

The default commit types are `feat` and `fix`, which should always be associated with user-facing changes to the code.
Whether the code is actually public to human users or part of an internal library, user-facing changes are any changes
to the ABI, API or code behavior that affect the intended or expected usage. Everything else is infrastructural.

For this reason, do not combine the `feat` or `fix` types with a scope that overlaps an existing commit
type (`build`, `ci`, `docs`, `style`, `test`). Use the corresponding commit type instead.

Avoid:

- `feat(ci): <description>`
- `feat(docs): <description>`
- `fix(test): <description>`

Instead, use the appropriate commit type.

- `ci: <description>`
- `docs: <description>`
- `test: <description>`

To be explicit about changes that fix repository infrastructure, even if they do not constitute a user-facing fix,
simply use the appropriate imperative description:

`fix(build): <description>`    → `build: fix ...`

The body of the commit message can refer to issues, PRs or other commits in the same repository by using a hashtag
followed by the GitHub issue or PR number or by using the shortened commit hash. When referring to resources from other
repositories, use the full URL.

## Examples

### Subject only

```
feat: add the option to breathe underwater (#123)
```

### Subject and body

```
fix: prevent drowning when underwater breathing is enabled (#124)

Install and use the scuba diving equipment correctly.
```

### Subject and scope

```
feat(api): implement some new endpoint (#125)
```

### Subject and longer body

```
feat: add some other shiny new feature (#126)

Add library XYZ to the manifest and update the package lock.

Import tool ABC from library XYZ to add the feature described in #101.

Remove old feature X. Because the new feature replaces feature X,
it can safely be deleted.
```

### Breaking change

```
refactor!: rename function to lower_snake_case (#127)
```

### Breaking change with scope and breaking change description

```
feat(api)!: simplified payload format (#128)

BREAKING CHANGE: use JSON for the payload instead of XML.
```
