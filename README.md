# Standard Version

[![Build Status](https://travis-ci.org/conventional-changelog/standard-version.svg)](https://travis-ci.org/conventional-changelog/standard-version)
[![NPM version](https://img.shields.io/npm/v/standard-version.svg)](https://www.npmjs.com/package/standard-version)
[![Coverage Status](https://coveralls.io/repos/conventional-changelog/standard-version/badge.svg?branch=)](https://coveralls.io/r/conventional-changelog/standard-version?branch=master)
[![Standard Version](https://img.shields.io/badge/release-standard%20version-brightgreen.svg)](https://github.com/conventional-changelog/standard-version)

> stop using `npm version`, use `standard-version` it rocks!

NPM and Cordova Automatic versioning and CHANGELOG management, using GitHub's new squash button and
the [recommended workflow](https://github.com/conventional-changelog/conventional-changelog-cli#recommended-workflow) for `conventional-changelog`.

_how it works:_

1. when you land commits on your `master` branch, select the _Squash and Merge_ option.
2. add a title and body that follows the [conventional-changelog-standard conventions](https://github.com/bcoe/conventional-changelog-standard/blob/master/convention.md).
3. when you're ready to release to npm:
  1. `git checkout master; git pull origin master`
  2. run `standard-version`
  3. `git push --follow-tags origin master; npm publish`

`standard-version` does the following:

1. bumps the version in _package.json_ (based on your commit history)
2. bumps the _version_ and _android-versionCode_ in _config.xml_ (based on your commit history)
3. uses [conventional-changelog](https://github.com/conventional-changelog/conventional-changelog) to update _CHANGELOG.md_
4. commits _package.json_ and _CHANGELOG.md_ and optionally _config.xml_
5. tags a new release

## Installation

### As `npm run` script

Install and add to `devDependencies`:

```
npm i --save-dev azachar/standard-version
```

Add an [`npm run` script](https://docs.npmjs.com/cli/run-script) to your _package.json_:

```json
{
  "scripts": {
    "release": "standard-version"
  }
}
```

Now you can use `npm run release` in place of `npm version`.

This has the benefit of making your repo/package more portable, so that other developers can cut releases without having to globally install `standard-version` on their machine.

### As global bin

Install globally (add to your `PATH`):

```
npm i -g azachar/standard-version
```

Now you can use `standard-version` in place of `npm version`.

This has the benefit of allowing you to use `standard-version` on any repo/package without adding a dev dependency to each one.

## Usage

### First Release

To generate your changelog for your first release, simply do:

```sh
# npm run script
npm run release -- --first-release
# or global bin
standard-version --first-release
```

This will tag a release **without bumping the version in package.json**.

When ready, push the git tag and `npm publish` your first release. \o/

### Cut a Release

If you typically use `npm version` to cut a new release, do this instead:

```sh
# npm run script
npm run release
# or global bin
standard-version
```

As long as your git commit messages are conventional and accurate, you no longer need to specify the semver type - and you get CHANGELOG generation for free! \o/

After you cut a release, you can push the new git tag and `npm publish` (or `npm publish --tag next`) when you're ready.

### Prevent Git Hooks

If you use git hooks, like pre-commit, to test your code before committing, you can prevent hooks from being verified during the commit step by passing the `--no-verify` option:

```sh
# npm run script
npm run release -- --no-verify
# or global bin
standard-version --no-verify
```

### CLI Help

```sh
# npm run script
npm run release -- --help
# or global bin
standard-version --help
```

## Commit Message Convention, at a Glance

_patches:_

```sh
git commit -a -m "fix(parsing): fixed a bug in our parser"
```

_features:_

```sh
git commit -a -m "feat(parser): we now have a parser \o/"
```

_breaking changes:_

```sh
git commit -a -m "feat(new-parser): introduces a new parsing library
BREAKING CHANGE: new library does not support foo-construct"
```

_other changes:_

You decide, e.g., docs, chore, etc.

```sh
git commit -a -m "docs: fixed up the docs a bit"
```

_but wait, there's more!_

Github usernames (`@bcoe`) and issue references (#133) will be swapped out for the
appropriate URLs in your CHANGELOG.

## Badges!

Tell your users that you adhere to the `standard-version` commit guidelines:

```markdown
[![Standard Version](https://img.shields.io/badge/release-standard%20version-brightgreen.svg)](https://github.com/conventional-changelog/standard-version)
```

## FAQ

### How is `standard-version` different from `semantic-release`?

[`semantic-release`](https://github.com/semantic-release/semantic-release) is a fully automated library/system for versioning, changelog generation, git tagging, and publishing to the npm registry.

`standard-version` is different because it handles the versioning, changelog generation, and git tagging for you **without** automatic pushing (to GitHub) or publishing (to an npm registry). Use of `standard-version` only affects your local git repo - it doesn't affect remote resources at all. After you run `standard-version`, you still have to ability to review things and correct mistakes if you want to.

They are both based on the same foundation of structured commit messages (using [Angular format](https://github.com/bcoe/conventional-changelog-standard/blob/master/convention.md)), but `standard-version` is a good choice for folks who are not yet comfortable letting publishes go out automatically. In this way, you can view `standard-version` as an incremental step to adopting `semantic-release`.

We think they are both fantastic tools, and we encourage folks to use `semantic-release` instead of `standard-version` if it makes sense for them.

### Should I always squash commits when merging PRs?

The instructions to squash commits when merging pull requests assumes that **one PR equals, at most, one feature or fix**.

If you have multiple features or fixes landing in a single PR and each commit uses a structured message, then you can do a standard merge when accepting the PR. This will preserve the commit history from your branch after the merge.

Although this will allow each commit to be included as separate entries in your CHANGELOG, the entries will **not** be able to reference the PR that pulled the changes in because the preserved commit messages do not include the PR number.

For this reason, we recommend keeping the scope of each PR to one general feature or fix. In practice, this allows you to use unstructured commit messages when committing each little change and then squash them into a single commit with a structured message (referencing the PR number) once they have been reviewed and accepted.

## License

ISC
