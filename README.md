# Introduction

GitHub and similar version control sysems used for developing IETF Internet Drafts and managing Working Group process in a transparent fashion can be highly effective. GitHub in particular enables several different modes of interaction for Working Groups and its participants, as discussed in the [Using GitHub draft](https://ietf-gitwg.github.io/using-github/draft-ietf-git-using-github.html). However, using these systems without any prior experience may be challenging. 

This document serves as a tutorial for GitHub and its underlying concepts. It provides instructions for interacting with GitHub for the purposes of managing, developing, and advancing Internet Drafts.

# Technology Overview

This section presents an overview of technologies related to the use of GitHub for IETF draft development and working group activities. It assumes familiarity with the general concept of version control systems. Readers familiar with this information may skip ahead without loss of continuity.

## Git

[Git](https://git-scm.com/book/en/v2) is a distributed version control system (VCS). As such, it provides a persistent, ordered history of modifications to files under source control. For the purposes of this work, that means text documents for Internet Drafts. This can be, for example, [Markdown](https://www.ietf.org/about/participate/tutorials/process/writing-rfcs-and-internet-drafts-markdown-and-bit-yaml/) or [XML](https://tools.ietf.org/html/rfc2629). Markdown is the preferred medium for documents tracked and managed with Git. 

There are a few concepts that are useful to know when working with Git, described below.

### Repository

A Git repository is a collection of files under source control.

### Commit

A commit is effectively a snapshot of all tracked files at a particular point in time. Each snapshot of a file is stored by hash. Each commit is a collection of file snapshots that capture the current state of the repository. Each commit has at least one parents to track history, as shown below.

~~~
... ====[commit 1]====[commit 2]====...===[commit N]===[commit N+1]===>
~~~

### Branch

Git uses the concept of "branches" to track repository snapshots. A branch points to an ordered sequence of commits. They are cheap to create and dispose. 

Minimally, each repository has a branch called "master," shown below. 

~~~
master: ====[commit 1]====[commit 2]====...===[commit N]===[commit N+1]===>
~~~

Many branches can exist in parallel. Ultimately, each of these branches "stem" from some commit in the "master" branch.

~~~
master: ====[commit 1]====[commit 2]====...===[commit N]===[commit N+1]===>
issue-1:                                            \====[commit 1]====[commit 2]====>
~~~

The base of a branch is the commit from which it originated. The tip of a branch is the latest commit in the sequence. In the example above, `commit 1` for the "issue-1" branch is *based* on `commit N` of the "master" branch.

Typically, individual features or document changes happen in branches through a series of one or more commits to said branches. 

### Merge

Upon completion of changes on a feature branch, the results are typically then added to a target branch. This procedure is referred to as *merging*, and is shown below.

~~~
master:  ...===[commit N]===[commit N+1]=========[commit N+2]===>
issue-1:            \====[commit 1]====[commit 2]====/
~~~

In this example, the "issue-1" branch is merged into master by creating a single commit -- `commit N+2` -- that combines `commit N+1` from "master" and `commit 2` from "issue-1". 

This type of merge is *not recommended*, as it makes the history of a particular repository difficult to read. Moreover, if the same parts of a version-controlled file were modified in the branches being merged, "conflicts" will occur. (Git cannot know whether changes from the source or target branch are correct.) In these cases, the authors needs to resolve the merge conflict, which is an error prone process.

### Re-Base and Merge

Authors can avoid merge conflicts by ensuring that their feature branches are always extensions of the target branch. Put differently, changing the base of a feature branch to be the tip of the target branch ensures that a merge to said target branch will (a) never conflict and (b) result in a *linear* history. This is shown below.

~~~
master:  ...===[commit N]===> (tip)             ====[commit N+1]===>
issue-1:              \====[commit 1]====[commit 2]====/
~~~

In this example, the "issue-1" branch was re-based on "master" from its tip (`commit N`). It then merges cleanly into "master" since all changes in "issue-1" are built on those in "master". (In other words, the sequence of commits is linear, with one path.)

### Pull Request

Informally, a Pull Request (PR) is a process by which the developer or creator of a feature branch asks to merge changes to some target branch. 

## GitHub

GitHub is a platform that hosts Git repositories and provides a web interface for interacting with and managing repositories. Beyond basic Git functionality, GitHub provides the following features that are useful for Internet Draft document management:

- [Code review](https://github.com/features/code-review/): Prepare, review, and merge PRs.
- [Issue tracking](https://help.github.com/en/articles/about-issues): A single, per-repository mechanism for collecting draft feedback, reporting issues, and organize tasks.
- [Documentation](https://github.com/features#documentation): Wiki-style documentation pages that track miscellaneous information and content inappropriate for Internet Drafts yet still relevant to the work, such as pointers to software implementations, test vectors, or other testing tools.
- [Content hosting](https://github.blog/2016-08-22-publish-your-project-documentation-with-github-pages/): Web hosting for Internet Draft papges.

## Continuous Integration (C-I)

TODO

# Mode Workflows

The following workflows assume your development machine is setup accordingly. Please see [the instructions here](https://github.com/martinthomson/i-d-template/blob/master/doc/SETUP.md) for more details.

## Document Management Mode

### Document Repository Creation

TODO: steps using Martin's tooling

### Document Modification

TODO: from fork or repository, create branch, make edits, push to branch, thens submit PR

### Document Submission

TODO

## Issue Tracking Mode

TODO

## Issue Discussion Mode

TODO

# Additional Resources

- https://datatracker.ietf.org/meeting/104/materials/slides-104-edu-sessg-git-github-and-markdown-for-internet-drafts-00
