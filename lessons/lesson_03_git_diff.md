# Lesson 3: Seeing What Changed (`git diff`)

**Scenario:** You edit a prompt and want to see exactly which lines changed before committing.

## Demo (Claude, throwaway repo)

1. Committed `prompt.txt` with two lines.
2. Edited it: changed line 2, added line 3. `git status -s` showed ` M prompt.txt` (M = modified).
3. `git diff` showed:
   ```
   -Answer briefly.
   +Answer in detail.
   +Cite sources.
   ```
4. After `git add`, `git diff` printed nothing: the change is now staged, no longer "unstaged".
5. `git diff --staged` showed the same change: it is what the next commit will contain.

## How to read a diff

- Line starting with `-` (red) = removed. Line starting with `+` (green) = added. No sign = unchanged context.
- `@@ -1,2 +1,3 @@` = old file lines 1-2 became new file lines 1-3.
- `a/` is the old version, `b/` is the new one.

## Rule of thumb

| Command | Compares |
|---|---|
| `git diff` | working directory vs staging area (unstaged changes) |
| `git diff --staged` | staging area vs last commit (what you are about to commit) |

Habit: run `git diff --staged` before every commit to review your own work.

## Exercise 3 (in `github_learning`)

```bash
echo "" >> README.md
echo "Learning Git and GitHub step by step." >> README.md
git status
git diff
git add README.md
git diff
git diff --staged
git commit -m "Describe the project in README"
git log --oneline
```

Note: `>>` appends to a file. `>` would overwrite it.

Paste the output and answer:
1. Why was the second `git diff` (after `git add`) empty?
2. Which command shows what the next commit will contain?
