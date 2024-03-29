# Contributing Guide

This document aims to describe the workflows and rules used for developing this
project. This includes, but is not limited to:

- how to contribute code (coding standards, testing, documenting source code)
- how pull requests are handled
- how to file bug reports

## Getting Started

Follow the steps below to setup your local development environment:

1. Clone repository

   ```sh
   git clone https://github.com/flex-development/dist-tag-action
   cd dist-tag-action
   ```

2. Install binaries with [Homebrew][1]

   ```sh
   brew bundle --file ./Brewfile
   ```

3. Set node version

   ```sh
   nvm use
   ```

4. [Configure commit signing][2]

5. Update `~/.gitconfig`

   ```sh
   git config --global commit.gpgsign true
   git config --global user.email <email>
   git config --global user.name <name>
   git config --global user.username <username>
   ```

6. Install dependencies

   ```sh
   yarn
   ```

   **Note**: This project uses [Yarn 2][3]. Consult [`.yarnrc.yml`](.yarnrc.yml)
   for an overview of configuration options and required environment variables.
   Furthermore, if you already have a global Yarn configuration, or any `YARN_*`
   environment variables set, an error will be thrown if any settings conflict
   with the project's Yarn configuration, or the Yarn 2 API. Missing environment
   variables will also yield an error.

7. [ZSH](docs/ZSH.md) setup

8. Update `$ZDOTDIR/.zprofile`:

   ```sh
   # PATH
   # 1. local node_modules
   [ -d $PWD/node_modules/.bin ] && export PATH=$PWD/node_modules/.bin:$PATH

   # DOTENV ZSH PLUGIN
   # - https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/dotenv
   export ZSH_DOTENV_FILE=.env.zsh

   # NVM
   # - https://github.com/nvm-sh/nvm
   export NVM_DIR=$HOME/.nvm

   # ---------------------------------------------------------------------------

   # LOAD ENVIRONMENT VARIABLES IN CURRENT WORKING DIRECTORY
   # 1. $GITHUB_WORKSPACE
   [ -d $PWD/.git ] && export GITHUB_WORKSPACE=$(git rev-parse --show-toplevel)
   ```

9. Load `dotenv` plugin via `$ZDOTDIR/.zshrc`:

   ```zsh
   plugins=(dotenv)
   ```

10. Reload shell

   ```sh
   exec $SHELL
   ```

### Environment Variables

#### Development

| name                    |
| ----------------------- |
| `CI`                    |
| `NODE_ENV`              |
| `NODE_NO_WARNINGS`      |
| `NODE_OPTIONS`          |
| `VITEST_SEGFAULT_RETRY` |
| `ZSH_DOTENV_FILE`       |

#### GitHub Actions

Variables are prefixed by `secrets.` in [workflow](.github/workflows/) files.

### Git Config

The examples in this guide contain references to custom Git aliases.

See [`.github/.gitconfig`](.github/.gitconfig) for an exhaustive list.

## Contributing Code

[Husky][4] is used to locally enforce coding and commit message standards, as
well as run tests pre-push.

Any code merged into the [trunk](#branching-model) must confront the following
criteria:

- changes should be discussed prior to implementation
- changes have been tested properly
- changes should include documentation updates if applicable
- changes have an associated ticket and pull request

### Branching Model

This project follows a [Trunk Based Development][5] workflow, specifically the
[short-lived branch style][6].

- Trunk Branch: `main`
- Short-Lived Branches: `feat/*`, `hotfix/*`, `release/*`

#### Branch Naming Conventions

When creating a new branch, the name should match the following format:

```zsh
[prefix]/<issue-number>-<branch_name>
 │        │              │
 │        │              │
 │        │              │
 │        │              └─⫸ a short, memorable name
 │        │
 │        └─⫸ check github issue
 │
 └─⫸ feat|feat/fix|hotfix|release
```

### Commit Messages

This project follows [Conventional Commit][7] standards and uses [commitlint][8]
to enforce those standards.

This means every commit must conform to the following format:

```zsh
<type>[scope][!]: <description>
 │     │      │    │
 │     │      │    │
 │     │      │    └─⫸ summary in present tense (lowercase without punctuation)
 │     │      │
 │     │      └─⫸ optional breaking change flag
 │     │
 │     └─⫸ see commitlintrc.json
 │
 └─⫸ build|ci|chore|docs|feat|fix|perf|refactor|revert|style|test|wip

[body]

[BREAKING CHANGE: <change>]

[footer(s)]
```

`<type>` must be one of the following values:

- `build`: Changes that affect the build system or external dependencies
- `ci`: Changes to our CI/CD configuration files and scripts
- `chore`: Housekeeping tasks / changes that don't impact external users
- `docs`: Documentation improvements
- `feat`: New features
- `fix`: Bug fixes
- `perf`: Performance improvements
- `refactor`: Code improvements
- `revert`: Revert past changes
- `style`: Changes that do not affect the meaning of the code
- `test`: Change that impact the test suite
- `wip`: Working on changes, but you need to go to bed :wink:

e.g:

- `build(deps-dev): bump cspell from 6.7.0 to 6.8.0`
- `perf: lighten initial load`

See [`.commitlintrc.json`](.commitlintrc.json) to view all commit guidelines.

### Code Style

[Prettier][9] is used to format code and [ESLint][10] to lint files.

#### ESLint Configuration

- [`.eslintrc.base.cjs`](.eslintrc.base.cjs)
- [`.eslintrc.cjs`](.eslintrc.cjs)
- [`.eslintignore`](.eslintignore)

#### Prettier Configuration

- [`.prettierrc.json`](.prettierrc.json)
- [`.prettierignore`](.prettierignore)

### Making Changes

Source code is located in [`src`](src) directory.

### Documentation

- JavaScript & TypeScript: [JSDoc][11]; linted with [`eslint-plugin-jsdoc`][12]

Before making a pull request, be sure your code is well documented, as it will
be part of your code review.

### Testing

This project uses [Vitest][13] to run tests.

[Husky](#contributing-code) is configured to run tests against changed files.

Be sure to use [`it.skip`][14] or [`it.todo`][15] where appropriate.

#### Running Tests

- `yarn test`
- `yarn test:cov`
  - See terminal for coverage output

### Getting Help

If you need help, make note of any issues in their respective files in the form
of a [JSDoc comment][11]. If you need help with a test, don't forget to use
[`it.skip`][14] and/or [`it.todo`][15]. Afterwards, [start a discussion in the
Q&A category][16].

## Labels

This project uses a well-defined list of labels to organize issues and pull
requests. Most labels are scoped (i.e: `status:`).

A list of labels can be found in [`.github/labels.yml`](.github/labels.yml).

## Opening Issues

Before opening an issue, make sure you have:

- read the documentation
- checked that the issue hasn't already been filed by searching open issues
- searched closed issues for solution(s) or feedback

If you haven't found a related open issue, or feel that a closed issue should be
re-visited, open a new issue.

A well-written issue

- contains a well-written summary of the bug, feature, or improvement
- contains a [minimal, reproducible example][17] (if applicable)
- includes links to related articles and documentation (if any)
- includes an emoji in the title :wink:

## Pull Requests

When you're ready to submit your changes, open a pull request (PR) against
`main`:

```sh
https://github.com/flex-development/dist-tag-action/compare/main...$branch
```

where `$branch` is the name of the branch you'd like to merge into `main`.

All PRs are subject to review before being merged into `main`.

Before submitting a PR, be sure you have:

- performed a self-review of your changes
- added and/or updated relevant tests
- added and/or updated relevant documentation

Every PR you open should:

- [follow this template](.github/PULL_REQUEST_TEMPLATE.md)
- [be titled appropriately](#pull-request-titles)

### Pull Request Titles

To keep in line with [commit message standards](#commit-messages) after PRs are
merged, PR titles are expected to adhere to the same rules.

## Merge Strategies

In every repository, the `rebase and merge` and `squash and merge` options are
enabled.

- **rebase and merge**: PR has one commit or commits that are not grouped
- **squash and merge**: PR has one commit or a group of commits

When squashing, be sure to follow [commit message standards](#commit-messages):

```zsh
<type>[scope][!]:<pull-request-title> (#pull-request-n)
 │     │      │   │                    │
 │     │      │   │                    │
 │     │      │   │                    └─⫸ check pull request
 │     │      │   │
 │     │      │   └─⫸ lowercase title
 │     │      │
 │     │      └─⫸ optional breaking change flag
 │     │
 │     └─⫸ see .commitlintrc.json
 │
 └─⫸ build|ci|chore|docs|feat|fix|perf|refactor|release|revert|style|test
```

e.g:

- `ci(workflows): simplify release workflow #24`
- `refactor: project architecture #21`
- `release: @flex-development/dist-tag-action@1.0.0 #13`

## Releasing

> Note: Release publication is executed via GitHub workflow.\
> This is so invalid or malicious versions cannot be published without merging those
> changes into `main` first.

Before releasing, the following steps must be completed:

1. Schedule a code freeze
2. Decide what type of version bump the package needs
   - `yarn recommended-bump`
3. Bump version
   - `bump <new-version>`
   - `bump major`
   - `bump minor`
   - `bump patch`
   - `bump premajor --preid <dist-tag>`
   - `bump preminor --preid <dist-tag>`
   - `bump prepatch --preid <dist-tag>`
   - `bump prerelease --preid <dist-tag>`
4. `yarn conventional-changelog -i CHANGELOG.md -s`
5. `yarn release`
6. Open PR from `release/*` into `main`
   - PR title should match `release: <release-tag>`
     - e.g: `release: 1.1.0`
   - link all issues being released
   - after review, `squash and merge` PR
     - `release: <release-tag> (#pull-request-n)`
       - e.g: `release: 1.1.0 (#3)`
   - on PR merge, [release workflow](.github/workflows/release.yml) will fire
     - if successful, the workflow will:
       - pack project
       - create and push new tag
       - create and publish github release
       - make sure all prereleased or released issues are closed
       - delete release branch

[1]: https://brew.sh
[2]:
  https://docs.github.com/authentication/managing-commit-signature-verification/about-commit-signature-verification#gpg-commit-signature-verification
[3]: https://yarnpkg.com/getting-started
[4]: https://github.com/typicode/husky
[5]: https://trunkbaseddevelopment.com
[6]: https://trunkbaseddevelopment.com/styles/#short-lived-feature-branches
[7]: https://conventionalcommits.org
[8]: https://github.com/conventional-changelog/commitlint
[9]: https://prettier.io
[10]: https://eslint.org
[11]: https://jsdoc.app
[12]: https://github.com/gajus/eslint-plugin-jsdoc
[13]: https://vitest.dev
[14]: https://vitest.dev/api/#test-skip
[15]: https://vitest.dev/api/#test-todo
[16]: https://github.com/flex-development/dist-tag/discussions/new?category=q-a
[17]: https://stackoverflow.com/help/minimal-reproducible-example
