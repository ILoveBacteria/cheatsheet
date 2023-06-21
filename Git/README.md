# Git Cheatsheet

## Table Of Contents

- [Git Cheatsheet](#git-cheatsheet)
  - [Table Of Contents](#table-of-contents)
  - [Github Workflows](#github-workflows)
  - [Handwrite Notes](#handwrite-notes)
    - [How to undo the last commit?](#how-to-undo-the-last-commit)

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
