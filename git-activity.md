# Activity

The following is meant to demonstrate a typical git workflow and practice some fundamental git commands.

## Git started

```shell
$ cd ~/Desktop

$ mkdir bootcamp

$ mv shell-lesson-data bootcamp

$ cd bootcamp

$ ls -a1
./
../
shell-lesson-data/
```

Create a new git repository

```shell
$ git init
Initialized empty Git repository in .git/

$ ls -a1
./
../
.git/
shell-lesson-data/

$ ls -F1 .git
HEAD
config
description
hooks/
index
info/
logs/
objects/
refs/
```

## Commits

Add a file

```shell
nano README.md
```

```markdown
# Bootcamp 2021

A collection of activities from MSiA Bootcamp 2021

```

> Note the trailing newline at the end of the file  
> (`^O`, `Enter`, `^X` to save and exit `nano`)

Check our git status to see new file

```shell
$ git status

On branch main

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
    README.md
    shell-lesson-data/

nothing added to commit but untracked files present (use "git add" to track)
```

Add the file to our stage

```shell
$ git add README.md

$ git status
README.md           shell-lesson-data/
On branch main

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
    new file:   README.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)
    shell-lesson-data/
```

Commit our changes

```shell
$ git commit -m "Initial Commit"

[main (root-commit) 23f6c5b] Initial Commit
 1 file changed, 1 insertion(+)
 create mode 100644 README.md
```

List our commits to see history

```shell
$ git log

commit 23f6c5b5303f8a684ef60ec2e89c9943df9b1f8e (HEAD -> main)
Author: Michael Fedell <michaelfedell14@gmail.com>
Date:   Tue Sep 14 10:42:22 2021 -0700

    Initial Commit

$ git log --oneline

23f6c5b (HEAD -> main) Initial Commit

```

Inspect our commit further

```shell
$ git show 23f6c5b

commit 23f6c5b5303f8a684ef60ec2e89c9943df9b1f8e (HEAD -> main)
Author: Michael Fedell <michaelfedell14@gmail.com>
Date:   Tue Sep 14 10:42:22 2021 -0700

    Initial Commit

diff --git a/README.md b/README.md
new file mode 100644
index 0000000..01ef7ca
--- /dev/null
+++ b/README.md
@@ -0,0 +1 @@
+git-demo
(END)
```

## Git Ignore

```shell
$ git status -s
?? shell-lesson-data/

$ echo "shell-lesson-data" > .gitignore

$ git add .gitignore
$ git status -s
A  .gitignore

$ git commit -m "Add gitignore to project"
[main 7502890] Add gitignore to project
 1 file changed, 1 insertion(+)
 create mode 100644 .gitignore
```

## Branches

```shell
$ git branch dev

$ git checkout dev

Switched to branch 'dev'
```

> These can be combined in a single command: `git checkout -b dev`

```shell
$ echo "## Git\n" >> README.md

$ cat README.md

# Bootcamp 2021

A collection of activities from MSiA Bootcamp 2021

## Git

$ git status -s

 M README.md

$ git commit -a -m "Add git section to readme"

[dev 6b407d0] Add git section to readme
 1 file changed, 2 insertions(+)
```

```shell
$ git checkout main

Switched to branch 'main'
(base) [10:58:14] ➜  northwestern/tutorials/git-demo git:(main)
$ cat README.md

# Bootcamp 2021

A collection of activities from MSiA Bootcamp 2021
```

## Stash

Add a change to our README

```shell
$ echo "## Shell\n" >> README.md

$ cat README.md

# Bootcamp 2021

A collection of activities from MSiA Bootcamp 2021

## Shell

$ git status -s

 M README.md
```

"Stash" this change for later

```shell
$ git stash

Saved working directory and index state WIP on main: 7502890 Add gitignore to project

$ cat README.md

# Bootcamp 2021

A collection of activities from MSiA Bootcamp 2021

$ git status

On branch main
nothing to commit, working tree clean

$ git stash list

stash@{0}: WIP on main: 7502890 Add gitignore to project
(END)

```

```shell
$ git stash pop

On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
    modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
Dropped refs/stash@{0} (72c5e7f509cfea99faec4eadd835fbc6d5f9741c)

$ cat README.md

# Bootcamp 2021

A collection of activities from MSiA Bootcamp 2021

## Shell


$ git status -s

 M README.md
```

Let's go ahead adn commit this change

```shell
$ git add README.md

(base) [11:03:20] ➜  northwestern/tutorials/git-demo git:(main) ✗
$ git commit -m "Add shell section to readme"
commit       -- record changes to repository
[main 2dab5a4] Add shell section to readme
 1 file changed, 2 insertions(+)

 ```

## Merges

Let's review the state of our repo

```shell
$ git status

On branch main
nothing to commit, working tree clean

$ git log --oneline

2dab5a4 (HEAD -> main) Add shell section to readme
7502890 Add gitignore to project
23f6c5b Initial Commit
```

But remember that commit we made on `dev` branch?

Here's a cool command to show commits from multiple branches

```shell
$ git log --oneline --decorate --graph --all

* 2dab5a4 (HEAD -> main) Add shell section to readme
| * 6b407d0 (dev) Add git section to readme
|/
* 7502890 Add gitignore to project
* 23f6c5b Initial Commit
```

Let's incorporate the changes on our `dev` branch; e.g. `merge` them into `main`

```shell
$ git merge dev

Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
Automatic merge failed; fix conflicts and then commit the result.
```

Uh-oh... looks like this failed; let's inspect

```shell
$ git status

On branch main
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
    both modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
```

Looks like we'll have to fix this ourselves

```shell
$ nano README.md
```

```markdown
# Bootcamp 2021

A collection of activities from MSiA Bootcamp 2021

<<<<<<< HEAD
## Shell
=======
## Git
>>>>>>> dev
```

The `<<<<<<<`, `=======`, and `>>>>>>>` markers tell us which parts of the file have been modified by the two sources (denoted by the `HEAD` and `dev` labels)

In this case, we can fix this conflict by  editing the file to allow both changes to stay. Simply remove the lines containing `<<<<<<<`, `=======`, and `>>>>>>>` so that your file looks like:

```markdown
# Bootcamp 2021

A collection of activities from MSiA Bootcamp 2021

## Shell

## Git
```

As always, our best course of action is to check status

```shell
$ git status

On branch main
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
    both modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
```

Git doesn't yet know that we're done fixing the file, so we'll have to tell it we're done.

```shell
$ git add README.md

(base) [11:13:39] ➜  northwestern/tutorials/git-demo git:(main) ✗
$ git status

On branch main
All conflicts fixed but you are still merging.
  (use "git commit" to conclude merge)

Changes to be committed:
    modified:   README.md

(base) [11:13:42] ➜  northwestern/tutorials/git-demo git:(main) ✗
$ git commit
```

If git throws you into a `vim` window (this is the default editor if you didn't configure it to use `nano`), simply type `:wq` to save and exit vim (using the default commit message of `Merge branch 'dev'`)

Finally, let's see the results of our merge

```shell
$ git log --oneline --decorate --graph --all

*   8741c3e (HEAD -> main) Merge branch 'dev'
|\
| * 6b407d0 (dev) Add git section to readme
* | 2dab5a4 Add shell section to readme
|/
* 7502890 Add gitignore to project
* 23f6c5b Initial Commit
```

Congrats, you've successfully resolved your first Merge Conflict!
