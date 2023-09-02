# Git Cheatsheet

[Good Cheatsheet](https://cs.fyi/guide/git-cheatsheet)

## Table Of Contents

- [Git Cheatsheet](#git-cheatsheet)
  - [Table Of Contents](#table-of-contents)
  - [Git Commands](#git-commands)
    - [Config](#config)
    - [Stage and Commit](#stage-and-commit)
    - [Branch](#branch)
    - [Tag](#tag)
    - [Signing](#signing)
    - [Blame](#blame)
    - [Bisect](#bisect)
  - [Github Workflows](#github-workflows)
  - [Handwrite Notes](#handwrite-notes)
    - [How to undo the last commit?](#how-to-undo-the-last-commit)
    - [How to undo the reset?](#how-to-undo-the-reset)

## Git Commands

`git help <command>`: Show help for a command.

### Config

- `git config --global user.name`: Show current user name.
- `git config --global user.name <name>`: Set user name.
- `git config --global user.email`: Show current user email.

### Stage and Commit

- `git init`
- `git add -A`: Add stage all files
- `git status`
- `git log`
- `git diff HEAD`: Show difference between current changes and last commit.
- `git diff --staged`: Show changes in staged files
- `git reset <file>`: Unstage a file
- `git checkout -- <file>`: Discard changes in working directory. Restore file from last commit.
- `git rm <file>`: Delete a file from both git and filesystem.
- `git show <commit>`: Show changes in commit.

### Branch

- `git branch`: List all branches.
- `git branch <branch>`: Create a new branch.
- `git checkout <branch>`: Switch to a branch.
- `git merge <branch>`: Merge a branch into the active branch.
- `git branch -d <branch>`: Delete a branch.
- `git remote add origin <server>`: Connect a local repository to a remote server.
- `git remote -v`: List all currently configured remote repositories.

### Tag

- `git tag`: List all tags.
- `git tag -a <tag> -m <message>`: Create a new tag at HEAD.
- `git tag -a <tag> <commit>`: Create a new tag at a specific commit.
- `git show <tag>`: Show changes in tag.
- `git push origin <tag>`: Push a tag to remote repository. By default, the git push command doesn’t transfer tags to remote servers.
- `git checkout <tag>`: Checkout a tag.

### Signing

pgp: pretty good privacy

gpg: gnu privacy guard

- `gpg --list-keys`: List all keys.
- `gpg --gen-key`: Generate a new key.
- `gpg --list-secret-keys --keyid-format LONG`: List all my secret keys. We get the public key from the beginning of the `/`.
- `git config --global user.signingkey`: Show current signing key.
- `git tag -s <tag> -m <message>`: Create a new signed tag at HEAD. This will add a secret message.
- `git tag -v <tag>`: Verify a signed tag.
- `git commit -S -m <message>`: Create a signed commit.

### Blame

- `git blame <file>`: Show who changed what and when in a file.
- `git blame -L <start>,<end> <file>`: Show who changed what and when in a file in a specific range.

### Bisect

Debugging tool to find the commit that introduced a bug.

bisect: binary search commit history

- `git bisect start`: Start a bisect session.
- `git bisect good <commit>`: Mark a commit as good.
- `git bisect bad <commit>`: Mark a commit as bad.
- `git bisect bad`: Mark the current commit as bad.

## Github Workflows

A workflow is a configurable automated process made up of one or more jobs. You must create a workflow file to use the workflow. The file must be located in the `.github/workflows` directory of your repository.

A sample workflow file:

```yaml
# This workflow will install Python dependencies and run tests.
# For more information see: https://docs.github.com/en/actions/automating-builds-and-tests/building-and-testing-python

name: Unit Tests

on:
  push:
    branches: [ "master" ]
  pull_request:
    branches: [ "master" ]

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3
      - name: Set up Python 3.10
        uses: actions/setup-python@v3
        with:
          python-version: "3.10"

      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install pipenv

      - name: Run tests
        run: pipenv run test
```

## Handwrite Notes

- `git clone --branch master --depth 1`: Just clone the last commit and not the whole history.

### How to undo the last commit?

- Option1 `git reset --hard`: You want to destroy the last commit C and also throw away any uncommitted changes.
```bash
git reset --hard HEAD~1
```

- Option 2 `git reset`: Maybe the last commit wasn't a disaster, but just a bit off. You want to undo the commit but keep your changes
```bash
git reset HEAD~1
```

### How to undo the reset?

 - `git reflog`: Find the commit you want to go back to.
 - then for example: `git reset "HEAD@{1}"`
