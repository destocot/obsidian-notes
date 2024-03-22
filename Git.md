# Trunk-Based Development

Overview
1. You create a new branch.
2. You write and commit your code.
3. You push the commits to the remote repository on GitHub.
4. You open a Pull Request (PR).
5. You wait for the Continuous Integration pipeline to pass.
6. You request a review from other developers and discuss the code.
7. Once the PR is approved you merge it to the main branch.

# Clone repository
```bash
git clone <REPO_URL>
```

# Create a branch
```bash
git checkout -b <BRANCH_NAME>
```

# Stage changes

> Using the `.` stages all changes (use with caution)
```bash
git add .
```

# Check staged files
```bash
git status
```

# Commit changes
```bash
git commit -m "<COMMIT_MESSAGE>"
```

# Push to remote repository
```bash
git push origin <BRANCH_NAME>
```

> Pushing to the main branch
```bash
git push origin main
```

## (Theory) Pull Request / Merge Request

- With a "Pull Request" you *request* your branch to be *merged* with the main branch (or any other branch).

> *Hey folks, I wrote a bunch of code. I want to merge it so our users can benefit from it. Could you have a look and tell me if the code looks ok?*

- a description to explain the context and the code changes
- a place to discuss and comment the code
- a mechanism to ensure that only reviewed code can be merged

# (Optional) Request Reviewers

# Squash and Merge
- Fill out form that asks for title and a description
- clean up by deleting the branch

> Delete branches can be recovered

> By using the squash merge the history of the main branch contains only one commit per PR

# Commit history
```bash
git log --oneline
```

#### log
```bash
git log --graph --decorate
```

# Pull and sync from remote repository
```bash
git pull origin main
```

# Commit changes directly
```bash
git commit -a -m "Flag fields with mines"
```

---
# FEM - Git

- open git manual
```bash
man git -
```

#### What is git?
- Git is a distributed version control system, VCS.

General Flow:
**untracked** -> **staging area** -> **tracked**

- All git config keys are in the following shape: `<section>.<key>`
- --global flag will ensure you set this key value for all future uses of git and repos
- user.name and user.email are the key's used in creating a commit tied to you
- to add a key value, execute git config --add --global `<key> "<value>"`
- you can view any value of git config by executing git config --get `<key>`

**Basics**
- git add <path-to-file | pattern> will add zero or more files to the index (staging area)
- git commit -m `'<message>'` will commit what changes are present in the index.
- git status will describe the state of your git repo which will include tracked, staged, and untracked changes.

> **SHA** stands for Secure Hashing Algorithm. SHA is a modified version of MD5

#### Inspect files within the git's data store.
```bash
git cat-file -p <some-sha>
```

- each commit will have a tree and parent as a **pointer** to the **ENTIRE** worktree
```bash
➜  my-first-git git:(master) git cat-file -p 48d06ff
tree 6282551fedc655bc5ee9180ad67021c22245fdae
parent 5ba786fcc93e8092831c01e71444b9baa2228a4f
author ThePrimeagen <the.primeagen@gmail.com> 1706387467 -0700
committer ThePrimeagen <the.primeagen@gmail.com> 1706387467 -0700

second
```

#### Add config values
```bash
git config --add <section>.<keyname> <value>
```

```bash
git config --get-regexp fem
git config --list | grep <thing>
```

## Branches
```bash
git branch foo
git switch <branch name>
git checkout <branch name>
git branch -D <branch name> 
```

## Merge
A merge is attempting to combine two histories together that have diverged at some point in the past. There is a common commit point between the two, this is referred to as the best common ancestor ("merge base" is often the term used in the docs).

The branch your on is the target branch and the branch you name in `<branchname>` will be the source branch.

```bash
git merge <branchname>
```


- fast forward merge
	- only one branch diverges
- 3 way merge
	- both branches diverge

## Rebase
-  rebase updates the commit where the branch originally points to

The basic steps of rebase is the following:
1. execute git rebase `<targetbranch>`. I will refer to the current branch as `<currentbranch> `later on
2. checkout the latest commit on `<targetbranch>`
3. play one commit at a time from `<currentbranch>`
4. once finished will update `<currentbranch>` to the current commit sha

## Reflog
```bash
git reflog
```

> can find files that have been committed even if the branch is deleted

## Cherry Pick
```
Given one or more existing commits, apply the change each one introduces,
recording a new commit for each. This requires your working tree to be clean
(no modifications from the HEAD commit).
```

## Remote Git
```bash
git remote add <name> <uri>
git remote add origin ../hello-git
git remote -v
git fetch
git branch -a
git merge origin/trunk
git pull <remote> <branch>
git branch --set-upstream-to=origin/<branch> trunk

```

If you wish take your changes and move the remote repo, you can do this by using git push. Much like pull, if you are not "tracking" then you cannot simply git push but instead you will have to specify the remote and branch name.
```bash
git push
git push <remote> <local_name>:<remote_name>
git push <remote> :<remote_name>
```

## Stash
Use git stash when you want to record the current state of the working directory and the index, but want to go back to a clean working directory. The command saves your local modifications away and reverts the working directory to match the HEAD commit.

```bash
git stash -m "my lovely message here"
git stash list
git stash show [--index <index>]
git stash pop
git stash pop --index <index> # works well with git stash list
```



