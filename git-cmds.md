# Git Commands

## 1. Terminal Basics
**- `ls`:** lists all the files/folders inside the current path

**- `ls -a`:** lists all the files/folders inside the current path, including hidden files

**- `cd ..`:** go back one folder

**- `cd ../..`:** go back two folders

**- `cd ../../<folder name>`:** go back twice, then into a folder

**- 'touch <file name>':** Simple way (Git Bash) to create a file inside the current path

**- 'cat <file name>':** used to display the contents (even codes) of any file and it works with any .txt, .js, .py, .java, etc. file

**- 'mv <oldfile.txt> <newfile.txt>':** renames the file on your system

**- `rm -rf <file name>`:** Deletes the file from the directory

*Observation:*

- Hidden folders/files usually start with a `.`

- `.` refers to the current directory

- `..` refers to the parent directory (one level up)

- That's why **`cd ..`** takes you back - you're literally telling it "Go to the parent directory"!

**To run Git commands, we are going to first install Git.**

**But how to configure Git after installing it?**

## 2. Git Configuration

Before committing, Git needs to know who you are.

```bash
git config --global user.name "Hamzah"
git config --global user.email "hamzah@example.com"
git config --list
```

This sets your name and email (will show up in commits). The `--global` makes it apply everywhere. Without this, Git doesn’t know who made the changes.

---

## 3. Creating a Repository
There are two ways to start working with Git:

1. Create a new repo from scratch (`git init`).
2. Copy an existing repo from GitHub (`git clone`).

### Case 1: Start a new repo locally

- ``: Initializes the folder into a Git repository. From now on, Git tracks changes here.

| What happens when you run `git init`     |
| ---------------------------------------- |
| Creates a hidden `.git/` folder          |
| Sets up basic Git configuration          |
| Enables version tracking for this folder |

#### Inside the `.git/` folder:

| File/Folder | Purpose                                                |
| ----------- | ------------------------------------------------------ |
| `HEAD`      | Points to the current branch                           |
| `config`    | Repository-specific settings                           |
| `objects/`  | Stores all file snapshots (commits, blobs, trees)      |
| `refs/`     | References to branches and tags                        |
| `hooks/`    | Custom scripts triggered on actions (like commit/push) |

#### Before vs After

| Before `git init`      | After `git init`                           |
| ---------------------- | ------------------------------------------ |
| Just a normal folder   | Folder becomes a Git repository            |
| No version control     | Can now track changes                      |
| Can’t use Git commands | Can run all Git commands                   |
| No branching           | Can create branches and connect to remotes |

Example:

```bash
mkdir project
cd project
git init
```
Now let’s connect this local repository to GitHub so we can push our commits:

```bash
git remote add origin <url>
git branch -M main
git push -u origin main
```

(We'll Learn about this remote command with branches and commits later on)
---

### Case 2: Work on an existing repo (GitHub → local)

- ``: Downloads an existing repository (like copying someone’s folder from GitHub to your computer).

```bash
git clone <repo-url>
cd repo
```

| What happens when you run `git clone`                     |
| --------------------------------------------------------- |
| Creates a new folder with the repo name                   |
| Automatically initializes `.git/` inside it               |
| Downloads the full history of commits, branches, and tags |
| Sets up a remote called `origin` pointing to GitHub       |

#### Key difference from `git init`:

| `git init`                             | `git clone`                        |
| -------------------------------------- | ---------------------------------- |
| Use when starting fresh                | Use when repo already exists       |
| Empty history, you create first commit | Full history is downloaded         |
| You must connect remote manually       | Remote `origin` is auto-configured |

---

## 4. Staging & Committing
Before moving to commands, let’s understand what staging and committing really mean.

**What is Staging Area?**

| Think of the staging area as a "preparation zone" or "packing area" |
| ------------------------------------------------------------------- |
| Files you want to commit must go through staging first.             |

**Why use a Staging Area?**

| Example                                                             |
| ------------------------------------------------------------------- |
| - You modified 3 files: A.txt, B.txt, C.txt                         |
| - You want to commit A.txt and B.txt together, but C.txt separately |
| - Staging lets you:                                                 |
| 1. Stage and commit A.txt + B.txt                                   |
| 2. Stage and commit C.txt later                                     |

**What is a Commit?**

| A commit is like taking a snapshot of your project at a specific point in time. |
| ------------------------------------------------------------------------------- |
| Each commit has:                                                                |
| - A unique ID (hash)                                                            |
| - A message describing what changed                                             |
| - The actual changes made to files                                              |
| - Information about when and who made the changes                               |

**Think of commits like saving a video game:**

| Each save is a checkpoint in our progress.         |
| -------------------------------------------------- |
| We can go back to any save point if needed         |
| Each save has a description of what we achieved    |
| We can see our whole journey through all our saves |

**Why do we make commits?**

| Reason                                                           |
| ---------------------------------------------------------------- |
| Track history: See what changed, when, and why                   |
| Backup points: Can return to earlier commits if something breaks |
| Collaboration: Others can understand our changes                 |

---
So, it's clear that after modifying a file/folder we need to add → commit the changes so as to reflect those changes in the GitHub repo.
The workflowcommands for adding → committing are here below:

```bash
git status            # shows what changed
git add <file>        # stage that file
git add .             # stage everything
git reset <file>      # remove from suitcase (unstage)
git commit -m "msg"   # seal it with a message
```

Example:

```bash
echo "Hello" > a.txt
git add a.txt
git commit -m "added hello file"
```

**- `git status`:** Tracks the history of changes made in the repository. Shows which files have been added, modified, or deleted.

| So, the typical workflow looks like: |
|-----------------------------------|
| - Firstly, make some changes to your files |
| **- `git status`** (check what's changed) |
| **- `git add <file name>`** (stage specific files or all files) |
| **- `git status`** (verify what's staged) |
| **- `git commit -m "message"`** (commit the staged changes) |

---

## 5. Checking Changes

```bash
git diff            # see what you have changed but not staged
git diff --staged   # see what you staged but not committed
git status          # see a summary of all changes and their state
```

**NOTE:**

We can always rely on `git status` to see what's staged (green) and what's not (red).

---

## 6. Undo & Recovery

Sometimes mistakes happen.

```bash
git restore <file>              # discard changes in a file
git checkout <commit> -- <file> # take version of file from old commit
```

### Reset Types (⚠️ careful here)

```bash
git reset --soft <commit>   -> go back, keep staged
git reset <commit>          -> go back, keep changes unstaged
git reset --hard <commit>   -> go back, throw everything away
```

### Case Study: `git reset --mixed` (default)

1. You create `file.txt` with content **Hello** (First commit: `abc123`)  
2. You add **World** to `file.txt` (Second commit: `def456`)  
3. You add **!!!** to `file.txt` (Third commit: `ghi789`)  

When you run:
```bash
git reset abc123
```

### Revert (safe undo)

```
git revert <commit>
```

**What happens:**  
- Commit history is moved back to **abc123**  
- The later commits (`def456`, `ghi789`) are gone from history  
- But `file.txt` on disk still contains **Hello World!!!**  
- Those changes are now **unstaged** (shown by `git status`)  

This is the default `git reset` behavior, also called **`--mixed` reset**.  
It resets the commit history and staging area, but keeps the working directory files unchanged.

---

## 7. Branching

Branches are like different workspaces.

```bash
git branch              -> list branches
git branch exp          -> new branch called exp
git switch exp          -> move to exp branch
git switch -c fixbug    -> make and switch
```

Example:

```bash
git branch feature1
git switch feature1
echo "new line" >> a.txt
git commit -am "feature added"
```

---

## 8. Remote and Local Repo

*Remote = GitHub (or similar). Local = your laptop.*

---

### What is a Remote?
The remote is the central copy on GitHub.  
Your local repo is the working copy on your machine.  
Remotes let you share commits with others and keep a central source of truth.

---

### Common Commands
```bash
git remote add origin <url>   # add a remote named "origin" (one-time)
git remote -v                 # verify configured remotes and their URLs

# First time: link local -> GitHub and push initial commit
git branch -M main
git remote add origin <url>
git push -u origin main       # first push; sets upstream tracking

# Normal workflow after that
git add .
git commit -m "message"
git pull origin main          # fetch remote changes and merge into current branch
git push                      # push your committed changes
```

**Why git pull before git push?**

If someone else pushed changes to the remote since your last update, your push can be rejected (non-fast-forward).
Pulling first brings remote changes into your local branch so you can resolve any differences and then push cleanly.

*Simple Practical Sequence (Avoid Push Rejection)*
```bash
git status # check your state.

git add <files> → git commit -m "..." # commit your local work.

git pull origin main # integrate any remote changes.
```

*If conflicts appear: fix files, git add the resolved files, then git commit.*

```bash
git push # now push will succeed.
```

**Common Error and Fix**

*Error:*

```bash
! [rejected] main -> main (non-fast-forward)
```

*Fix:*

```bash
git pull origin main
# resolve conflicts if any
git add <resolved-files>
git commit   # if a merge commit is required
git push
```
**Advanced Note (Optional)**

There is a way to keep a linear history using:
```bash
git pull --rebase
```

## 9. Forks & Upstream

## Working with Forks (Keeping Your Fork Updated)

**What is a Fork?**  
A fork is your own copy of someone else’s repository on GitHub.  
- You fork when you don’t have direct write access to the original repo.  
- You can make changes safely in your fork without affecting the original project.  
- Later, you can suggest your changes back using a Pull Request.  

So the setup looks like this:
- **origin** → your fork (your GitHub copy)  
- **upstream** → the original project (the main repo you forked from)  

---

### Why add `upstream`?
By default, when you clone your fork, Git only knows about **origin** (your copy).  
But you also want to stay in sync with the original repo. That’s why we add **upstream**.

---

### Commands
```bash
git remote add upstream <url-of-original>   # add original repo as upstream
git fetch upstream                          # fetch latest commits from upstream
git checkout main                           # switch to your main branch
git merge upstream/main                     # merge changes from upstream into your fork
git push origin main                        # push the merged changes to your fork
```

**Practical Flow**

*Work on your fork (origin).*
*Before starting new work, sync your fork with upstream:*
```bash
git fetch upstream
git merge upstream/main
```
*Push the updates back to your fork (origin).*
*Continue with your changes.*

**Common Pitfall**

*If you don’t sync regularly, your fork can go “out of date” compared to the original.
Then your pull requests may conflict with upstream changes.*

**⚠️ Force Reset (Dangerous)**

If you want your fork’s main branch to exactly match the upstream main (discarding all local changes):
```bash
git fetch --all --prune
git reset --hard upstream/main
```


**⚠️ Use this only if you’re sure you don’t need your local commits anymore. It wipes them out.**

Analogy:
Think of a fork like having your own copy of a recipe.

The chef (original repo) keeps updating the recipe.

You copy it (fork) and try your variations.

To avoid cooking from an outdated recipe, you regularly check the chef’s latest version (upstream) and sync it into your copy (origin).

---

## 10. Stash (Temporary Shelf)

*How to temporarily save changes made to the working directory that are not yet ready to be committed?*

**-'git stash <file name>':** Files are REMOVED from our working directory (they disappear from file manager) and the changes are saved in a hidden Git area so we can switch to other work and come back to them later.

**-'git stash pop':** Brings back the most recent stash

**-'git stash clear':** permanently deletes ALL our stashed changes.

---

## 11. Removing Files

```
git rm <file>
git commit -m "removed file"
```

This tells Git that file is gone.

---

## 12. Conflict Resolution

When two people edit the same file:

1. Run `git pull` → conflict appears.
2. Open file → see markers (<<<<<<<, ======, >>>>>>>).
3. Edit to final version.
4. `git add file`
5. `git commit`

---

## 13. .gitignore

To stop tracking certain files:

```
node_modules/
*.log
.env
```

---

## 14. Useful Logs

```
git log --oneline --graph --decorate --all
```

Shows a clear branch diagram.

---

# Quick Workflows (Summary)

### A. New repo → push to GitHub

```
git init
git add .
git commit -m "first commit"
git remote add origin <url>
git push -u origin main
```

If push errors:

```
git pull origin main --rebase
git push
```

### B. Clone existing → push updates

```
git clone <url>
git add .
git commit -m "update"
git pull origin main --rebase
git push
```

### C. Sync fork

```
git fetch upstream
git checkout main
git merge upstream/main
git push origin main
```
