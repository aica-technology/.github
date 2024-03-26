# Tracking changes with a changelog

Each package should have a `CHANGELOG.md file` to track changes between release versions in a human-readable way.

## What is a package?

A package is defined as a collection of code that produces an output and has a version. The package output can be static
files, compiled binaries or other files, which may be packaged in a docker image for distribution. The version of a
package should be defined either by a VERSION file or by a version specifier in a package manifest file (package.json,
package.xml or similar).

Packages can fall under some different categories. In some cases (such as the frontend repository), the entire
repository can be considered as the package. Other repositories contain multiple packages.

### cmake projects

Some packages (such as control-libraries) are a cmake project and have a corresponding CMakeLists.txt file which defines
the project (package) version.

### ROS 2 packages

Most of the packages in our stack are ROS 2 packages. A ROS 2 package is a folder containing at minimum a package.xml
manifest file with a specific format and compilation instructions in a CMakeLists.txt file. and source code or other
metadata.

### Meta-packages

The hardware-collections repository bundles multiple ROS2 packages into one collection that produces a versioned output
image. The collection is versioned separately from the included packages by a package.xml file.

## Updating the changelog

Whenever a PR changes a file in a package or changes the output the package produces, the changelog should be updated to
describe the change. In the simplest case, the changelog entry is equivalent to the commit message.

In some cases, a PR might not change any files in a package nor the package output. For example, a CI commit may only
touch GitHub action or workflow files. In this case, the changelog might not need to be updated. However, in other
cases, some infrastructure changes to the CI, dockerfile or build system outside of the package source files may still
affect the package output. In that case, the changelog should address those changes.

As a general rule, if a new release would change the package output in any way as a result of the PR, that change should
be written into the changelog.

## Why have a changelog when conventional commits are used?

If conventional commits are used effectively, the history of code changes can be reconstructed and interpreted quite
clearly from the commit history. Many tools exist to automatically generate nicely formatted release notes from commit
messages alone. See also the [COMMIT_STYLE_GUIDE.md](./COMMIT_STYLE_GUIDE.md)

However, there are several reasons to prefer a manually curated changelog:

- A linear history of discrete changes between commits may not efficiently or effectively communicate the overall
  changes
  in a release. For example, fix commits that follow a feat may not actually fix an issue in the previous release but
  rather fix implementation mistakes in the new feature. With hindsight, such a series of commits could be squashed into
  a
  single feature, and so too the change notes might just be condensed to a single feature description.
- The brief and imperative nature of conventional commit message subjects may not be the most elegant or readable format
  for overall release notes.
- A repository can contain multiple packages, and some commits may not affect the package files or output at all; as a
  result, the actual changes to individual packages may not be clear from the commit history alone.

## Changelog format

The changelog should be a `CHANGELOG.md` markdown file at the top level of the package. It should start with
a `# CHANGELOG` heading, optionally followed by a table of `Versions:` linking to subheadings.

### Changelog entries during development

Changes between releases should be tracked in a special subheading with the name `## Upcoming changes (in development)`.

A changelog entry should be a list item (starting with `-`) and formatted as a conventional commit subject. The entry
should end with the **parent issue number** in parentheses (e.g. `#99`). If there is no parent issue, the PR number can
be used instead.

```
- feat: add a spin animation on button click (#99)
```

The list of entries should generally be ordered from oldest to newest; new entries are added to the end of the list.

When editing the changelog as part of a PR, it is permitted and encouraged to group or rewrite previous sections to
improve clarity, with the goal to make it as easy as possible to write clear and concise release notes in future.

Consider that the changelog must be updated before the PR is approved and merged; an approved change entry can then be
re-used as the commit message when closing the PR.

### Updating the changelog for release

When making a release, the changelog entries under the `## Upcoming changes` section should be moved to a new section
titled with the corresponding release version (e.g. `## 1.1.0`). The section should start with a high-level summary of
the release to highlight the key changes to the previous version.

The changelog entries can be divided into different subheadings to separate features from fixes. The entries can be also
re-ordered or re-grouped as necessary to keep releated changes together. For example, if one change entry adds a feature
and then another extends it, those changes can be combined as one new feature in the release notes. Similarly, if a later
change entry completely reverts a previous change entry, the two entries can be dropped from the release notes.

The overall goal is to maximize readability and clarity of the release notes.

## Changelog example

```
# CHANGELOG

Versions:

- [1.1.0](#110)
- [1.0.1](#101)
- [1.0.0](#100)

## Upcoming changes (in development)

- fix: update on_start in editor when the graph changes (#339)
- fix: remove duplicate edges when moving the node around (#342)
- feat: add persistent position for on_start (#357, #358)

## 1.1.0

<!-- summary of changes comapred to 1.0.1 -->

### Features

- some feature (#103)

### Fixes

- some fix (#102)

## 1.0.1

<!-- summary of changes comapred to 1.0.0 -->

### Fixes

- some fix (#101)

## 1.0.0

<!-- general description of the initial version -->
```
