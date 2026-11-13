# Lesson 8: `.gitignore` (keep secrets and junk out of Git)

**Scenario:** You add a `.env` file with an API key and a `venv/` folder. A careless `git add .` would commit both and the key would end up on GitHub. `.gitignore` tells Git which files to skip.

## Demo (throwaway repo)

```bash
git init -b main
echo 'print("hi")' > app.py
echo 'API_KEY=fake-not-real' > .env
mkdir venv data && echo x > venv/lib.py && echo "a,b" > data/train.csv
git status -s                      # 1. before: .env, app.py, data/, venv/ all listed as ??
printf '.env\nvenv/\ndata/*.csv\n__pycache__/\n' > .gitignore
git status -s                      # 2. after: only .gitignore and app.py are listed
git check-ignore -v .env           # 3. which rule ignores .env?
git add .
git status -s                      # 4. only .gitignore and app.py staged
```

## `.gitignore` pattern rules

| Line | Meaning |
|---|---|
| `.env` | ignore any file with this name |
| `venv/` | ignore a folder (trailing `/`) |
| `data/*.csv` | `*` wildcard: all `.csv` files in `data/` |
| `__pycache__/` | Python bytecode folders |
| `.DS_Store` | macOS junk file |
| `# text` | comment |
| `!keep.csv` | exception: do NOT ignore this file |

Put `.gitignore` at the repo root. Commit it, so the whole team shares the rules.

## New command: `git check-ignore -v <file>`

- Prints the rule that ignores the file. `-v` = verbose.
- Output `.gitignore:1:.env	.env` = file `.gitignore`, line 1, pattern `.env`, path `.env`.
- No output = the file is NOT ignored.

## The big gotcha

`.gitignore` only affects files Git is **not tracking yet**.
- If `.env` was already committed, adding it to `.gitignore` won't stop tracking. Stop tracking with `git rm --cached .env` (keeps the file on disk).
- If a secret was pushed, treat it as leaked: rotate/revoke the key. Deleting it later does not remove it from history.
- So: create `.gitignore` BEFORE the first `git add .`.

## Exercise 8 (in `github_learning`, fake secrets only)

```bash
echo 'API_KEY=fake-not-real' > .env
mkdir venv && echo x > venv/lib.py
git status -s                                  # .env and venv/ show as ??
printf '.env\nvenv/\n__pycache__/\n.DS_Store\n' > .gitignore
git status -s                                  # only .gitignore shows as ??
git check-ignore -v .env                       # prints the matching rule
git check-ignore -v README.md                  # prints nothing
git add .gitignore
git commit -m "Add .gitignore"
git push
git status -sb
```

## Questions
1. Why create `.gitignore` before the first commit of a secret file?
2. After `.gitignore` was created, why did `git status -s` stop showing `.env`?
3. A file is already tracked and you add it to `.gitignore`. What happens, and how do you fix it?

## Recap
`.gitignore` = list of patterns Git skips. Create it first, commit it, check with `git check-ignore -v`. Ignoring does not remove already-tracked files or already-pushed secrets.

## AI project `.gitignore` starter (preview of Phase 7)
```
.env
venv/
.venv/
__pycache__/
*.pyc
.ipynb_checkpoints/
data/
models/
*.pt
*.ckpt
.DS_Store
```
