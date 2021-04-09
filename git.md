# Git and GitHub

## What (and why) is Git?

Git is a version control software (VCS) that has become the standard for managing development projects. It serves two main purposes:

1. Version Control: manage the evolution of a project as it grows in complexity, easily rollback or revert breaking changes, inspect history, and maintain separate versions for development, integration, release, etc.
2. Collaboration: allow many users to collaborate on a single codebase remotely in an effective and managed manner; provides tools for integrating changes from multiple developers, resolving conflicts, and tracking the owner of certain features/changes.

You should use git for every software project you work on, regardless of whether you share that project with others.
You do not need to use GitHub with git.
You should be comfortable with using git from the command line (without a GUI).
You do not need to memorize all the git commands, but we'll cover an important set of them.
You do not need to be a pro on resolving merge conflicts, but should be comfortable if one pops up

## Fundamentals (git)

We'll cover the core concepts and terms that are fundamental to understanding git

Repository: A "repo" is a collection of managed code. It may represent a complete project, a part of a larger project, or really logical unit of code.
Remote: Repositories can be set up to keep in sync with a "remote" repository that is kept in a central place like GitHub, GitLab, BitBucket, etc. allowing for effective collaboration.
Branch: A branch is a tool to keep some collection of changes separated from the rest of the project. Commonly used to work on developing new features while the "main" branch is kept stable.
Commit: A commit marks a set of changes submitted to the code's history. It's up to the developer to decide what should go in a commit, but best practice is to keep commits small and "atomic", meaning that they can stepped through one-by-one without breaking the project
Working Directory: This represents the state of the files on your local computer. This may include new changes, ignored files, etc. and thus not fully match the state of the repository.
Stage: The stage is the collection of changes that are about to be committed. This allows you to specify which changes make it into a commit when you have a lot of unrelated changes in your working directory.

## Working with Remotes

Remotes

Forks

Conflicts

Pull Requests (or Merge Requests)



## Flow

![Basic Git Workflow (how changes progress)](https://www.cloudturbine.com/wordpress/wp-content/uploads/2016/08/git_workflow.png)

![GitFlow Workflow (one strategy for remote collaboration)](https://wac-cdn.atlassian.com/dam/jcr:b5259cce-6245-49f2-b89b-9871f9ee3fa4/03%20(2).svg?cdnVersion=1548)

## Common Commands

### Local

status
log
show
diff

### Remotes

remote
pull
rebase
merge

## Configuration

config
gitignore
global gitignore

## Managing Releases

tags
releases
wiki's

## GitHub Features

pages
ssh keys
gpg keys
repo settings

## What if I?

> Have made a ton of changes since my last commit...
add/commit changes incrementally

> Can't push because someone else has made changes...
stash/commit, pull (merge/rebase), push

> Can't pull because I've made changes...
stash

> Can't find the changes I was working on...
check your stash, check your current branch, etc.

> Need to "go back in time"...
git checkout <some commit>

> Mess up some file and need to reset it
git checkout -- <some file>

> Forgot to add a file to my last commit
git commit --amend --no-edit (`oops`)

> Accidentally committed something secret (api key, passwords, etc...)
It's in history forever...
Expire the secret, purge from history (bfg)

> Committed something that shouldn't be in git (not sensitive)
git reset <some file> or git remove --cached

> Have made a ton of commits on my branch that I don't want to show up in main branch
git rebase --interactive

