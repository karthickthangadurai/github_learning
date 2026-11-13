# Lesson 6: Connecting Your Repo to GitHub

## Concepts

- **Remote**: a copy of your repo hosted somewhere else (GitHub). Your local repo and the remote are separate; they only sync when you tell them to.
- **origin**: the conventional nickname for your main remote. It is just a name for a URL.
- **Upstream/tracking branch**: the remote branch your local branch is linked to (`origin/main`).
- **Push** = upload local commits to the remote. **Pull** = download and merge. **Fetch** = download only.

```
Local repo  --git push-->  GitHub (origin)
Local repo  <--git pull--  GitHub (origin)
```

## Steps (in order)

1. Create an EMPTY repo on GitHub (no README, no .gitignore, no license), because your local repo already has history. A non-empty remote causes a history mismatch on the first push.
2. `git remote add origin <url>` links the local repo to it.
3. `git remote -v` shows the link.
4. `git push -u origin main` uploads `main` and sets tracking (`-u`).
5. Refresh the GitHub page: your commits appear.
6. Future pushes are just `git push`.

## HTTPS vs SSH

| | HTTPS | SSH |
|---|---|---|
| URL | `https://github.com/user/repo.git` | `git@github.com:user/repo.git` |
| Auth | Token via credential helper (`gh` handles it) | SSH key pair |
| Setup | Easy | One-time key setup |

You are using HTTPS via `gh`. We will compare SSH later.

## Before pushing: safety checklist

- Public or private? Private is safest while learning.
- Anything secret in history (keys, tokens)? Once pushed, assume it is exposed even if deleted later.
- Check: `git log --stat` and `git ls-files`.

## Exercise 6

Answer the repo name and visibility first (see chat), then we run the steps together.

## Commands explained

| Command | What it does | Why you run it |
|---|---|---|
| `git remote add origin <url>` | Saves a nickname (`origin`) for the GitHub repo URL | Tell your local repo where GitHub is |
| `git remote -v` | Lists remotes with their URLs (`-v` = verbose) | Verify the link |
| `git push -u origin main` | Uploads `main` to `origin`. `-u` links your local `main` to `origin/main` | First push; later `git push` is enough |
| `git push` | Uploads new local commits to the linked remote branch | Share or back up your work |
| `git status -sb` | Short status plus branch line, e.g. `## main...origin/main` (`[ahead 1]` if unpushed) | Check sync with GitHub |
| `git ls-files` | Lists files Git is tracking | Check what would be pushed |
| `git log --stat` | History with files changed per commit | Review before publishing |
