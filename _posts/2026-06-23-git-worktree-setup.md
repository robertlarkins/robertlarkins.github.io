---
layout: post
title: "Git Worktrees"
date: 2026-06-23
categories: ['tools']
tags: ['git']
---

Soul: Active voice, concise, provides context to aid understanding. Consistent naming. Avoid using you or your - pros and cons.

Should it be directory or directory (use consistently)
terms and syntax should match the original documentation where applicable.

 Focus on active voice, being concise (but without losing meaning or introducing ambiguity), provides context to aid understanding. Consistent naming. I also avoid using you or your (but could be convinced with good enough reason). Terms and syntax should match the original documentation where applicable. No acronyms or jargon that is not defined. The blog post should be self contained. Include links to appropriate references.


We all know the pain of context switching in git. It is some version of: `git stash`, switch branches, make the change, commit, switch back, and `git stash pop`. _Git Worktrees_ alleviates this friction. Instead of a single working tree, the worktree feature enables **multiple branches checked out simultaneously, each in its own directory**, all attached to a single underlying local repository.

Additional benefits of multiple worktrees include:

- **Instant Context Switching** \
Keep a feature branch running in one directory while opening a separate directory to handle a hotfix or review a pull request. No stashing, no rebuilding.

- **Shared Local Cache** \
Because all worktrees share a single `.git` directory, operations like `git fetch` and the underlying object database are shared. The repository is not cloned multiple times.

- **Isolate Build Artifacts** \
Switching branches in a single directory often forces the IDE or compiler to rebuild the entire project. Separate directories keep build artifacts intact.


## Setting Up Multiple Worktrees

Worktrees can be organised in different ways, but the following approach creates a clean, self-contained project structure:

```text
my-project/
├── .git/          # The actual bare repository (in a hidden directory)
├── main/          # Worktree for the main branch
├── feature-abc/   # Worktree for a specific feature
└── hotfix-123/    # Worktree for an urgent fix
```

The root project directory (`my-project` in the example) contains the hidden .git directory for the git database. This leaves the root clean for worktree sub-directories.

Establish this structure using the following steps.


## 1. Clone a repository as bare:

Clone the repository as bare by running:

```bash
# Creates a directory for the repo and clones the repo's git database into it
git clone --bare <repo-url/repo-name.git> <repo-name>/.git
```
This creates `<repo-name>` as the root project directory, with the .git directory as a sub-directory. Running `git status` will show the root has no working tree with the error `fatal: this operation must be run in a work tree`. 


## 2. Configure the refspec

Configure the default remote fetch reference specification (refspec) in the project directory:

```bash
cd <repo-name>
git config remote.origin.fetch "+refs/heads/*:refs/remotes/origin/*"
```
A bare repository is typically used to setup a central Git repository on a server - to act as the remote, not to track one. Consequently, it lacks the default refspec set by the standard `git clone`. This refspec dictates what Git downloads from the remote and where it maps locally, ensuring the repository properly updates its remote-tracking branches. This enables commands like `git fetch` or `git rebase origin/main` to function from any worktree.


## 3. Adding a Worktree

The syntax for adding a worktree depends on which branch will be checked out in the worktree:
- A new branch.
- A remote branch.
- An existing local branch.

The following examples outline common syntax for adding a worktree:

### Add a worktree and a new branch with the same name

```bash
# Creates a worktree directory named 'my_feature'
# and a new branch named 'my_feature', branching from the current HEAD.
# Syntax: `git worktree add <path>`
git worktree add my_feature
```
If a branch named `my_feature` already exists, Git automatically checks the branch out into this worktree.


### Add a worktree and a new branch with different names

```bash
# Creates a new branch named 'my_feature' and a worktree named 'myf'.
# Syntax: `git worktree add -b <new-branch> <path>`
git worktree add -b my_feature myf
```

### Checkout a remote branch in a new worktree

If a branch exists on the remote but not locally, it can be checked out into a new worktree. Ensure origin is fetched first.
```bash
git fetch origin
# Creates a directory 'main' and creates a local 'main' branch tracking 'origin/main'
# Syntax: `git worktree add <path>`
git worktree add main 
```

### Checkout an existing local branch into a new worktree

If a local branch exists but isn't checked out into its own worktree:
```bash
# Syntax: `git worktree add <path> <commit-ish>`
git worktree add bugfix-directory existing-bugfix-branch
```


### Explicitly create a new branch from a specific start point

To create a new branch based on a remote branch (or another commit) and check it out into a new worktree:
```bash
# Syntax: `git worktree add -b <new-branch> <path> <commit-ish>`
git worktree add -b feature-xyz feature-xyz origin/main
```
The `<path>` and `<commit-ish>` are _positional arguments_, so their order matters. The `<new-branch>` is an _option argument_ anchored to the `-b` _option_ (or _flag_), meaning its position in the arguments can move.


## 4. Removing a Worktree and Branch

Once a feature is merged and the worktree is no longer needed, it is best practice to clean up both the worktree and the local branch. 

Note that Git prevents a branch deletion if it remains checked out in an active worktree.

### 1. Remove the Worktree Directory

First, instruct Git to remove the worktree. This deletes the directory and unlinks it from the bare repository.

```bash
# Syntax: git worktree remove <path>
git worktree remove my_feature
```
If the directory has uncommitted changes, Git blocks the removal. Adding `-f` forces the removal.

### 2. Delete the Associated Branch

Removing the worktree does **not** delete the Git branch. To keep the repository clean, delete the local branch as well:

```bash
# Syntax: git branch -d <branch-name>
git branch -d my_feature
```
Using `-D` will force branch deletion, which is necessary if the branch has not been merged.


## Further Tips

### List all worktrees

```bash
git worktree list
```


### Move a worktree

```bash
git worktree move <old-path> <new-path>
```
Never use the OS file explorer to move a worktree, as it will break the hardcoded `.git` file link.


### Repair broken links

```bash
git worktree repair
```
Use this command to restore connectivity if a directory is accidentally moved or deleted manually.


### Shifting changes between worktrees

Uncommitted changes can be shifted between worktrees using `git stash`:
1. Run `git stash` to store desired changes in the current worktree.
1. Navigate to the target worktree.
1. Run `git stash pop` to apply the changes to the target worktree.