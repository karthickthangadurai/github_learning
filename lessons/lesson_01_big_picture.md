# Lesson 1: The Big Picture

## 1. Roadmap

| Phase | Topic | Outcome |
|---|---|---|
| 1 | Git fundamentals | Understand the mental model, create a first repo |
| 2 | Daily commands | `add`, `commit`, `log`, `diff`, `restore`, `reset`… |
| 3 | GitHub basics | Push/pull, SSH vs HTTPS, README, `.gitignore` |
| 4 | Branching and merging | Branches, merges, resolving conflicts |
| 5 | Professional workflow | PRs, reviews, branch protection, commit messages |
| 6 | Advanced Git | Rebase, cherry-pick, stash, reflog, recovering work |
| 7 | GitHub for AI engineers | Secrets, datasets, model files, LFS, notebooks |
| 8 | Real-world project | A Python AI app from the first commit to a release |

## 2. Git vs. GitHub

- **Git** is a tool that runs on your machine. It tracks changes to your files and keeps a full history. It works offline.
- **GitHub** is a website that hosts Git repositories online. It adds collaboration features: pull requests, code review, issues and CI.

Git is like the Word track-changes engine. GitHub is like a shared drive that adds comments and approval flows. Other hosts such as GitLab and Bitbucket do the same job as GitHub, and the Git commands are identical on all of them.

## 3. Why professionals can't work without Git

- **History:** you can see who changed what, when, and why, and go back to any earlier state.
- **Safe experiments:** you try an idea on a separate branch and throw it away if it fails.
- **Teamwork:** ten people edit the same codebase without overwriting each other.
- **Review:** nothing reaches production without another person reading it (pull requests).
- **For AI engineers:** you can find which commit broke your eval score or changed a prompt.

## 4. The core mental model

```
Working directory  --git add-->  Staging area  --git commit-->  Local repository
 (files you edit)                (the next snapshot)             (permanent history)
```

Think of taking a group photo:
- The **working directory** is people milling around.
- The **staging area** is people you've arranged in the frame.
- A **commit** is pressing the shutter. The photo is saved permanently.

Staging lets you commit only some of your changes, for example a bug fix separately from an unrelated experiment.

## 5. Real-world example

Tuning a RAG pipeline:
1. Edit `prompt.py`. The working directory now differs from the last commit.
2. `git add prompt.py` stages it.
3. `git commit -m "Tighten retrieval prompt"` saves a snapshot.
4. Two days later accuracy drops. `git log` shows the history, and you can inspect or undo the exact change.

## 6. Exercise 1 (safe, read-only)

```bash
git --version
git config --global user.name
git config --global user.email
```

Answer in your own words:
1. What is the difference between Git and GitHub?
2. Name the three areas a change passes through before it becomes permanent history.

Also tell me which email your GitHub account uses. If it differs from your global Git email, commits won't link to your profile.

## Recap

- Git = local version control. GitHub = online hosting and collaboration.
- Changes flow: working directory → staging area → local repository.

## Commands introduced

`git --version`, `git config` (see [git_command_reference.md](../git_command_reference.md))

## Commands explained

| Command | What it does | Why you run it |
|---|---|---|
| `git --version` | Prints the installed Git version | Confirms Git is installed and tells you which version |
| `git config --global user.name` | Reads (no value given) the name Git stamps on your commits | Check your identity is set |
| `git config --global user.email` | Reads the email Git stamps on your commits | Must match your GitHub email so commits link to your profile |
| `git config --global user.name "Name"` | Sets (value given) the name. `--global` = for all your repos | One-time setup on a new machine |
