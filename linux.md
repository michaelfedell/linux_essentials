# Command Line Basics

## Servers

The term "server" is a bit flexible, but often refers to a remotely-accessible computer.

Servers can be "bare-metal" machines running in a managed data center (deepdish, wolf, rstudio/jupyterhub, msia423), cloud resources provided by a vendor (AWS, Microsoft Azure, Google Cloud Platform, etc.), or virtual machines running on your own laptop (virtualbox, docker, etc.).

> "server" may also simply refer to a process that is listening to and "serving" connections; e.g. `jupyter notebook` runs a simple web server on your laptop so you can connect to the process via your browser. We will refer to these processes as "web servers" or "app servers" and user "Server" to refer to actual machines (virtual or physical) which users can log into and interact with.

Important parts of a Server:
- operating system
- file hierarchy
- users
- groups
- software, etc.
- ports

### Operating Systems

Operating Systems define how the machine runs and how users interact with its contents. You don't need to know much here but should be familiar with the "families" and some of the most common ones.

- Posix/Unix
    - Linux
        - Ubuntu
        - Debian
        - Red Hat
    - MacOS
- Windows

### Typical Directory Structure

Essentials:

More Info: https://en.wikipedia.org/wiki/Filesystem_Hierarchy_Standard

Binaries vs Libraries

### Users

Users are created on linux machines and given specific permissions to access certain files or commands.

Users typically have a `$HOME` directory where they can keep files and user-specific configuration. Sometimes a user will want to run a command for which they don't typically have permissions. In this case, they _may_ be able to run the command via `sudo`...

#### Super User and sudo

The `superuser` or `root` user is a special account that has permission to _everything_ on the server (files, commands, config, etc.). It should be used with extreme caution!

You can "become" a different user with the command `su`. This requires you to have the credentials for that user; you will be logged in as that user until you log out.

Alternatively, you can run a single command as `superuser` / `root` by prepending your command with `sudo` ("super user do ..."). This only requires that your current user is on the "sudoers list" (`/etc/sudoers`)

> _PLEASE_ never copy/paste something from the internet that starts with `sudo` unless you understand what the command is doing and why it needs to be run as sudo. As extra motivation, everything you run as `sudo` is logged and sent to the system administrators of shared systems; on your local system, you can delete every single thing on your computer (and thus completely break it) with a single command (sudo + 8 very dangerous characters: ~rm -rf /~)

### Groups

To more easily manage permissions, administrators often create "groups" of users.

Every user typically belongs to their own group (e.g. there is a `root` group consisting of just the `root` user). More helpful are additional groups which are created manually such as `msia-students` or `admin`. These groups can be given permissions over shared resources or to execute more system-level commands. When a new user is created, they can simply be added to the group and will have all the permissions they need.

### Common ports

Reserved (anything under 1024):
- `22`: SSH (secure shell connections)
- `80`: HTTP (simple web traffic)
- `443`: HTTPS (secure web traffic)

Developers typically run internal applications on 4-digit port numbers like:
- `3306`: MySQL default
- `5432`: Postgres default
- `3000`: many javascript-based web frameworks like express.js, react.js, etc.
- `5000`: Flask default
- `8000`: Django default
- `8080`: Python http server default
- `8888`: Jupyter Notebook default

## Shells (and terminals)

what's the difference

### Common Shells

- sh
- bash
- zsh
- powershell
- git-bash
- WSL !!

### Commands

Structure
Environment Command Options Arguments

```shell
MY_SECRET="hunter7" python -c "import os; print(os.environ[MY_SECRET])"
```

- Environment: Set environment variables specific to the execution of command
- Command: Run some executable
- Options: Specify options defined by the executable (optional modifiers)
    - longhand like `--verbose`
    - shorthand like `-v`
- Arguments: Pass arguments to the executable (required parameters)

#### Redirects



#### Environment

Your `ENV`ironment contains a set of variables available to commands being run within that session.

Many of these are set by default (`$USER`, `$HOME`), some are set by your user-profile (like in `~/.bashrc` or `~/.bash_profile`), others are set interactively (`export FOO=bar`), and few are set to a specific command (`FOO=bar echo $FOO`).

From the command line, these can be accessed via the `$` symbol; e.g. `ls $HOME` or `echo $USER`. Programming languages frequently make user of environment variables as well. In python you do this by `import os; print(os.environ)` which returns a `dict` object with available environment variables (keys and values are strings).

Environment variables are a great place to set and source configuration and secrets for your applications as it is part of the _execution environment_ and not your code. This means that the same code can run differently on your laptop than it will on some production server, or that code on some shared repository will not include sensitive secrets like your personal database password.

#### PATH

Your `PATH` is the "search path" for finding commands to execute. You can check your current `PATH` by executing `echo $PATH`. This will look something like:

```shell
/home/mfedell/.local/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:
```

When you type `python` in your shell, it will go look in these places (in order) for an executable by that name. The first one found will be run as can be checked by `which python`.

#### Common Commands

Navigation:
- `which`: what does this thing point to?
- `man`: manual for this thing?
- `whoami`: what user am I currently?
- `pwd`: where am I? (Path to Working Directory)
- `ls`: list the contents of current directory
- `echo`: repeat some string back to me

File Interaction:
- `cat`: print the contents of this file (conCATenate)
- `head` / `tail`: show me the start (head) or end (tail) of this file
- `less` / `more`: show me this files contents in pages
- `grep`: search for some "pattern" in this file (Global Regular Expression Print)
- `file`: what kind of file is this?
- `wc`: how many words/lines/bytes are in this file? (Word Count)
- `stat`: show information for file (file STATus)
- `touch`: create some file
- `open`: open some file with default or specified application
- `vim` / `nano`: common terminal-based text editors

> Vim is not scary. Vim is a great tool. Vim is your friend.
> (if you want to learn more about vim, I'm happy to hold a crash-course or link you to some good tutorials online...)

User/Group/Permission Management:
- `su`: become superuser or some specified user
- `sudo`: run this command as superuser
- `useradd`: create a new user
- `usermod`: modify a given user
- `groupadd`: create a new group
- `groups`: list the groups that a user belongs to
- `chmod`: edit the permission for a file (CHange file MODe)
- `chown`: edit the owner/group for a file (CHange file OWNer)

> `^C`, `^D`, and other helpful keystrokes

##### Vim in a Nutshell

`vim` is a text editor and is based on the more basic version, `vi` (`vim` = `Vi IMproved`).

`vim` has operating **modes**. There are 6 basic modes, we will just cover 4: **Normal Mode** (default), **Visual Mode**, **Insert Mode**, and **Command Mode**

**Normal Mode** (`[esc]`)is active by default when `vim` starts up. This mode allows you to issue commands to vim itself (such as changing modes or exiting).

**Visual Mode** (`v`) allows you to select characters, lines, or blocks of text for manipulation (like cut/copy, delete, indent, etc.).

**Insert Mode** (`i`) is how you actually change the contents of the file (normal editing).

**Command-line Mode** (`:`) is how you execute commands, and most commonly, exit vim (`:q` or `:wq` to **write** and **quit** or `:q!` to force-quit (discard changes)).

Vim is a very powerful editor and many developers use this as their primary IDE! You certainly don't have to know this much about vim, but you should at least know how to make some simple file edits and exit properly as it is the default editor on many systems and used very widely.
