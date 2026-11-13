# Lesson 4: Commit Only What You Choose (selective staging)

**Scenario:** You changed three things, but they belong in two different commits. Staging lets you split them.

## Your real situation right now

`git status -s` shows:
```
 M git_command_reference.md
 M progress_tracker.md
?? lessons/lesson_03_git_diff.md
```
- ` M` = modified, not staged. `??` = untracked.
- Logical split: the lesson file is one commit ("Add lesson 3 notes"); the reference and tracker are another ("Update reference and tracker").

## New commands

- `git add <file>` stages only that file.
- `git restore --staged <file>` unstages a file (moves it back from staging to working directory). Your edits are kept, only the staging is undone. Safe.

## Exercise 4

```bash
git status -s
git add lessons/lesson_03_git_diff.md
git status -s
git commit -m "Add lesson 3 notes"
git status -s
```
Expect: after the commit, the two ` M` files still remain.

Now practice unstaging:
```bash
git add git_command_reference.md progress_tracker.md
git status -s
git restore --staged progress_tracker.md
git status -s
```
Expect: `git_command_reference.md` shows `M ` (staged), `progress_tracker.md` shows ` M` (not staged).

Finish:
```bash
git add progress_tracker.md
git commit -m "Update command reference and tracker"
git log --oneline
```

Paste the output of the last `git log --oneline` and the `git status -s` outputs.

Questions:
1. What does the first column vs the second column of `git status -s` mean?
2. Did `git restore --staged` delete your edit to `progress_tracker.md`?

## Commands explained

| Command | What it does | Why you run it |
|---|---|---|
| `git status -s` | Short status. Column 1 = staged, column 2 = unstaged. `??` = untracked, ` M` = modified, `M ` = staged, `A ` = new and staged | Compact view of what is where |
| `git add lessons/lesson_03_git_diff.md` | Stages just that file | Commit one logical change alone |
| `git commit -m "Add lesson 3 notes"` | Commits only what is staged | Other modified files stay uncommitted |
| `git add a.md b.md` | Stages several named files at once | Group related files |
| `git restore --staged progress_tracker.md` | Unstages the file; your edits stay | Undo an accidental `git add` |
| `git add progress_tracker.md` | Stages it again | Finish the second commit |
| `git log --oneline` | Short history | Confirm two focused commits |
