# Lesson 5: Undoing Mistakes (`git restore`, `git reset`)

## Safety table

| Command | What it does | Safe? |
|---|---|---|
| `git restore --staged <file>` | Unstage, keep edits | Safe |
| `git reset --soft HEAD~1` | Undo last commit, keep changes staged | Safe |
| `git reset HEAD~1` (mixed, default) | Undo last commit, keep changes unstaged | Safe |
| `git restore <file>` | Discard unstaged edits in file | DESTRUCTIVE |
| `git reset --hard HEAD~1` | Undo last commit AND discard all changes | DESTRUCTIVE |

`HEAD~1` = one commit before the current one.

## Demo (throwaway repo)

A. Edited `config.txt` (temp 9.9), ran `git restore config.txt`: edit gone, file back to last commit.
B. Committed "Add top_p", ran `git reset --soft HEAD~1`: commit gone, change still staged.
C. Recommitted, ran `git reset --hard HEAD~1`: commit and change both gone.

## Exercise 5 (run in `github_learning`, safe only)

Do NOT use `restore` without `--staged` or `reset --hard` here.

```bash
echo "" >> README.md
echo "Temporary line." >> README.md
git commit -am "Temp commit"
git log --oneline
git reset --soft HEAD~1
git log --oneline
git status -s
git restore --staged README.md
git status -s
```
`-am` = `-a` (stage all modified tracked files) + `-m` (message). It does not include untracked files.

Expected: the "Temp commit" disappears from the log, `README.md` shows `M ` after reset --soft, then ` M` after unstaging.

Then undo your temp line safely. Tell me the output of the above first and I will guide the cleanup (it needs `git restore README.md`, which is destructive, so we do it together).

Questions:
1. What is the difference between `reset --soft` and `reset --hard`?
2. Why is `git restore README.md` dangerous but `git restore --staged README.md` is not?
