# Git Cheatsheet

## Table Of Contents

- [Git Cheatsheet](#git-cheatsheet)
  - [Table Of Contents](#table-of-contents)
  - [Handwrite Notes](#handwrite-notes)
    - [How to undo the last commit?](#how-to-undo-the-last-commit)

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
