---
layout: front
title: Git Basics
---

# Git Basics

[Git](https://git-scm.com/book/en/v2) is a version control system (VCS). As such, it provides a persistent, ordered history of modifications to files under source control. For the purposes of this work, that means text documents for Internet Drafts. This can be, for example, [Markdown](https://www.ietf.org/about/participate/tutorials/process/writing-rfcs-and-internet-drafts-markdown-and-bit-yaml/) or [XML](https://tools.ietf.org/html/rfc2629). Markdown is the preferred medium for documents tracked and managed with Git. 

Notably, Git is a distributed VCS. That means that a set of files can
be worked on in different locations at the same time, and edits can be
merged when desired. It also means that changes across multiple instances
can be shared and integrated very flexibly. For example, within an IETF
working group, there is likely an repository on a hosting service,
individual authors and design teams can have their own, and changes
can be handled across all interactions.

There are a few concepts that are useful to know when working with Git, described below.

## Repository

A Git repository is a collection of files under source control.

Repositories can be cloned, which creates a copy of the repository, including all commits.

## Commit

A commit (or revision) is effectively a snapshot of all tracked files at a particular point in time. Each snapshot of a file is stored by hash. Each commit is a collection of file snapshots that capture the current state of the repository. Each commit has at least one parent commit, as shown below.

```
... ====[commit 1]====[commit 2]====...===[commit N]===[commit N+1]===>
```

New commits can be added by making changes, staging changes with `git add` and then creating a new commit with `git commit`.

## [Branch](https://help.github.com/en/articles/github-glossary#branch) {#branch}

Git uses the concept of "branches" to track repository snapshots. A branch points to an ordered sequence of commits. They are cheap to create and dispose. 

Minimally, each repository has a branch called "master," shown below. 

```
master: ====[commit 1]====[commit 2]====...===[commit N]===[commit N+1]===>
```

Many branches can exist in parallel. Branches will often "stem" from some commit in the "master" branch.

```
master: ====[commit 1]====[commit 2]====...===[commit N]===[commit N+1]===>
issue-1:                                            \====[commit 1]====[commit 2]====>
```

The base of a branch is the commit from which it originated. The tip of a branch is the latest commit in the sequence. In the example above, `commit 1` for the "issue-1" branch is *based* on `commit N` of the "master" branch.

A branch is created with `git branch`.  A branch is activated by checking it out using `git checkout <branch>`.

Alternatively, a branch can be thought of as a pointer to a commit.  When new commits are added, the active branch moves to point to the new commit.

Typically, individual features or document changes happen in branches through a series of one or more commits to said branches. 

## Merge

Upon completion of changes on a feature branch, the results are typically then added to a target branch. This procedure is referred to as *merging*, and is shown below.

```
master:  ...===[commit N]===[commit N+1]=========[commit N+2]===>
issue-1:            \====[commit 1]====[commit 2]====/
```

In this example, the "issue-1" branch is merged into master by creating a single commit -- `commit N+2` -- that combines `commit N+1` from "master" and `commit 2` from "issue-1". 

This type of merge is *not recommended*, as it makes the history of a particular repository difficult to read. Moreover, if the same parts of a version-controlled file were modified in the branches being merged, "conflicts" will occur. (Git cannot know whether changes from the source or target branch are correct.) In these cases, the authors needs to resolve the merge conflict, which is an error prone process.

## Re-Base and Merge

Authors can avoid merge conflicts by ensuring that their feature branches are always extensions of the target branch. Put differently, changing the base of a feature branch to be the tip of the target branch ensures that a merge to said target branch will (a) never conflict and (b) result in a *linear* history. This is shown below.

```
master:  ...===[commit N]===> (tip)             ====[commit N+1]===>
issue-1:              \====[commit 1]====[commit 2]====/
```

In this example, the "issue-1" branch was re-based on "master" from its tip (`commit N`). It then merges cleanly into "master" since all changes in "issue-1" are built on those in "master". (In other words, the sequence of commits is linear, with one path.)

## Tags

Just like a branch, tags point to a commit.  Unlike branches, tags don't move when new commits are added.  Tags are created with `git tag`.

Tags are usually used to mark particular commits as being special.  For instance, tags are often used to mark release versions.

## Pull and Push

Git is a distributed system, so all of the previous operations happen only locally.  Changes are moved between repositories by copying commits (and branches, and sometimes tags).

Git maintains a set of "remotes", which are the location of other copies of the repository.  When a repository is cloned, git creates a remote called "origin" pointing to the original repository.  Remotes can be managed using `git remote`.

Commits in the local repository can be copied to another remote with `git push`.  Commits in the remote repository can be copied into the local repository with `git fetch` and `git pull`.  A pull also modifies the current branch to include any remote changes, whereas a fetch only takes a copy of the remote commits without making any changes to the local branch.
