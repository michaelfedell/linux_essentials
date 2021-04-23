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

#### Quick Aside on Package Managers

Each OS ships its own package manager which is used for installing/updating/managing extra software. These tools will handle version resolution, proper placement of files, and installation of any needed dependencies.

You can think of these tools in the same way as `pip` (the standard package manager for python).

You may see:

On various Linux distro's: `yum`, `apt` or `apt-get`, `dpkg`, `pacman`

On MacOS: `brew` (Homebrew)

On Windows... well... typically nothing... [for now anyways](https://devblogs.microsoft.com/commandline/windows-package-manager-preview/)

### Typical Directory Structure

Essentials:

- `/`: "the root" - where all things start
- `/bin`: "binaries" - essential executable files that the system relies on
- `/dev`: "devices" - linux treats connected devices like "files" for sake of management
- `/etc`: "etcetera" (pronounced like "etsy") - contains system-wide configuration
- `/home`: "home" - contains home directories for all the users on a machine
- `~`: "my home" - shorthand pointing to the `$HOME` directory of current user (like `/home/mfedell/`)
- `/lib`: "libraries" - software libraries required/used by the programs in `/bin` etc.
- `/root`: "root's home" - home directory for the special user, `root`
- `/usr`: "shared user data" - a whole secondary hierarchy for things that should be read-only but shared across users
- `/var`: "variable" - these files change often but include things like temp files, logs, etc.

More Info: <https://en.wikipedia.org/wiki/Filesystem_Hierarchy_Standard>

Binaries vs Libraries:

Binaries are compiled, purpose-built software programs that are easy for your machine to execute. These are the things that run when you type `ls` or `python` or most any other system command.

Libraries however are meant to be more flexible or reusable collections of code that can be used by various applications (they are often compiled down to binary as well; I know that's a bit confusing)

`train_model.py` may be your executable (like a "Binary"), whereas it depends on `pandas`, a "Library"

### Permissions

Every file has three permission sets and three permission types. The permission sets indicate who a particular permission applies to; the permission types indicate what level of interaction is allowed.

A permission looks like `-rwxrwxrwx` where the leading `-` indicates that it is a file (directories start with `d`), and the `rwx` clusters are permission types granted to each set:

```txt
-rw-rw-r--  1   user    group   size    timestamp   filename.ext
```

#### Permission Sets

Relevant to permission sets, every file has an owner and a group - these are covered more later on.

- owner: the single owner of the file can do these things
- group: every user in the specified group can do these things
- everyone: everyone regardless of identity can do these things

#### Permission Types

In the permission code we saw earlier (`-rwxrwxrwx`), the file has full, unprotected access. These strings will always be 7 characters long, so if execute permission is taken away, that place is filled with a `-`. The codes are as follows:

- `r`: Read
- `w`: Write
- `x`: Execute
- `-`: No permission

But 9 characters to describe permission is so wasteful (/s)... so linux has another way to denote permissions: Numerically... as three-digit octals. The permission codes can be mapped as below:

```text
rwx
421
```

And then the permission for a set is summed and presented as a single digit. If a user has read (`r`) and execute (`x`) permissions, that becomes `5` in numeric mode (`r+x = 4+1 = 5`). So a full permission string like: `-rwxrw---x` becomes (`761`)

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

Every user typically belongs to their own group (e.g. there is a `root` group consisting of just the `root` user and on my machine, an `mfedell` group with just me, `mfedell`). More helpful are additional groups which are created manually such as `students` or `admin`. These groups can be given permissions over shared resources or to execute more system-level commands. When a new user is created, they can simply be added to the group and will have all the permissions they need.

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

You may see/hear someone say: "Just go run this command in the (shell/terminal/command-line)" and depending on who you're talking to, any of those three words may be used. They are not actually the same. (Although it'd be quite pedantic to correct someone at that point...)

The **shell** is a program that acts as an interface between you and the operating system. `Bash` is a very popular shell (and the default on many systems). Most shells can interpret a common set of commands that together provide a pretty flexible scripting language (`.sh` files).

The **terminal** is an application that lets you send commands to and see results back from a shell program. Many terminals also handle pretty formatting/coloring of text, multi-pane views, etc. Every OS will have a default terminal, but you can choose to modify or use other terminal environments based on preference (I personally like iTerm2 for MacOS, Gnome Terminal for Linux desktops, and Windows Terminal for Windows)

The **command line** is basically just the line of text you enter into the terminal consisting of the command and its arguments.

### Common Shells

- sh, bash, fish, zsh: All variations on the typical, text-based shell that accepts text commands and returns text that contains info, describes other processes, or points to objects on the file system. All have a pretty similar set of commands.
- powershell: default Windows shell; inherently different to the above shells and has a totally different set of commands. This shell is object-oriented.
- git-bash: This was meant to allow Windows users to make use of `git` which itself relies on many linux-style shell commands. It is in fact a pretty complete shell that exposes Bash commands on a windows environment.
- WSL: This is a rather new development from microsoft that allows an incredibly easy and seamless development experience on Windows. This allows users to run a mostly complete linux environment right on their windows machine without worrying about typical virtualization or dual-boot setups. Can't recommend this one enough if you're on windows! ([learn more about WSL and how to install it here](https://docs.microsoft.com/en-us/windows/wsl/about))

### Profiles and "rc" Configuration Files

You may have noticed a `.bash_profile` or `.bashrc` in your home folder (`~/`), (or `.zshrc` if you're using `zsh`). These files inform your shell of any commands that should be run when a new session is initialized and is often used to set aliases, configurations, and other settings.

If you want environment variables ([as discussed below](#environment)) or changes to your `PATH` ([as discussed below](#path)) to persist across multiple sessions (e.g. closing and opening a new terminal), you will want to add those changes to your `~/.bashrc` (typically used for environment variables, aliases, and settings) or `~/.bash_profile` (typically used for `PATH` modifications).

### Commands

Structure:

`Environment` `Command` `Options` `Arguments` `?Redirect`

```shell
MY_SECRET="hunter7" python -c "import os; print(os.environ[MY_SECRET])" > output.txt
```

- Environment: Set environment variables specific to the execution of command
- Command: Run some executable
- Options: Specify options defined by the executable (optional modifiers)
  - longhand like `--verbose`
  - shorthand like `-v`
- Arguments: Pass arguments to the executable (required parameters)
- Redirect: Passes the output (text) of one command to some other destination

#### Redirects

- `|`: "Pipe" - passes the output of one command straight into the output of another
- `>`: "Overwrite" - sends the output of one command into a file, overwriting if it exists (instead of standard output)
- `>>`: "Append" - sends the output of one command into a file, appending to the end if it exists

example: `cat debug.logs info.logs | grep model > search_results.txt`

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
- `whoami`: what user am I currently? (`whyami` is not yet widely available)
- `pwd`: where am I? (Path to Working Directory)
- `ls`: list the contents of current directory
- `cd`: change directory to some specified path
- `echo`: repeat some string back to me
- `find`: search for files with filename matching some pattern

File Interaction:

- `cat`: print the contents of this file (conCATenate; there is no `dog` command)
- `head` / `tail`: show me the start (head) or end (tail) of this file
- `less` / `more`: show me this file's contents in pages (`q` to quit)
- `grep`: search for some "pattern" in this file (Global Regular Expression Print)
- `file`: what kind of file is this?
- `wc`: how many words/lines/bytes are in this file? (Word Count)
- `stat`: show information for file (file STATus)
- `touch`: create some file
- `cp`: copy some file from source to destination (will overwrite)
- `mv`: move some file from source to destination (will overwrite)
- `rm`: remove some file (not reversible)
- `mkdir`: create a new, empty directory at specified path
- `open`: open some file with default or specified application
- `curl`: download some content from a url
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

Processes:

- `top`: Display live information about running processes (helpful to see what's hogging memory, etc.)
- `ps`: Show the status of a process or list all controlled processes (helpful to dig into a particular process)
- `kill`: Terminate an identified process (helpful to force quit some runaway process or lost thread)

Handy Keystrokes:

- `^C`: Stop the current process (usually to cancel a command)
- `^D`: Send exit/end-of-line command (often to exit a session)
- `^R`: Search history of commands (to find a some complex command you looked up last week)
- `^L`: Clear the screen (in case you accidentally type your password or just need some space)
- `!!`: Repeat the last command (e.g. `sudo !!`)
- `Tab`: Autocomplete the command/filename/option you're typing (can install more autocompleters!)
- `↑` / `↓`: Scroll through previous commands in history

##### Vim in a Nutshell

`vim` is a text editor and is based on the more basic version, `vi` (`vim` = `Vi IMproved`).

`vim` has operating **modes**. There are 6 basic modes, we will just cover 4: **Normal Mode** (default), **Visual Mode**, **Insert Mode**, and **Command Mode**

**Normal Mode** (`[esc]`)is active by default when `vim` starts up. This mode allows you to issue commands to vim itself (such as changing modes or exiting).

**Visual Mode** (`v`) allows you to select characters, lines, or blocks of text for manipulation (like cut/copy, delete, indent, etc.).

**Insert Mode** (`i`) is how you actually change the contents of the file (normal editing).

**Command-line Mode** (`:`) is how you execute commands, and most commonly, exit vim (`:q` or `:wq` to **write** and **quit** or `:q!` to force-quit (discard changes)).

Vim is a very powerful editor and many developers use this as their primary IDE! You certainly don't have to know this much about vim, but you should at least know how to make some simple file edits and exit properly as it is the default editor on many systems and used very widely.

## Activity

I won't make you download and modify a bunch of random files that mean nothing to you... However, I think a useful activity may be to download/install/customize a terminal or a new shell of your choosing! One of the most important things for working in the shell is comfort and familiarity - might as well make it your own!

Personally, I use [iTerm2](https://iterm2.com/) with `zsh`. I use [OhMyZsh](https://ohmyz.sh/) to configure/manage the shell and have a few helpful (for me) modifications made to the theme and prompt. I also have a few extra plugins and extensions installed such as aws-autocomplete, an aws profile manager, common-aliases, and kubectl tools. I've also [set quite a few custom aliases](https://www.tecmint.com/create-alias-in-linux/) that have become a regular part of my workflow, like `alias oops='git commit --amend --no-edit'` for when you forget to stage that one file and still haven't pushed.

When I was first learning these things, I started with [this guide](https://sourabhbajaj.com/mac-setup/) on setting up a development environment on mac (sorry Windows friends) and think it's got some great stuff in there (esp. homebrew, iTerm2, zsh, vim, etc.).

On Windows, I use [WSL2](https://docs.microsoft.com/en-us/windows/wsl/install-win10) with [Ubuntu 20.04](https://www.microsoft.com/en-us/p/ubuntu/9nblggh4msv6?activetab=pivot:overviewtab) on the Windows Terminal

[Vim Bootstrap](https://vim-bootstrap.com/) is also a great tool to make `vim` a more friendly environment!

There are lots of great activities out there on linux/shell basics so not going to reinvent the wheel here. In fact, Lewis Meineke from previous years has put together a great list of resources:

<https://github.com/meineke/workflows/blob/master/sessions/shell.md#resources>

## Most Important Nuggets

- `rm` is basically irreversible, be very careful
- similarly, the shell is very powerful, so be curious but also careful (esp. when using `sudo`)
- I use `which` **all** the time for sanity checks and troubleshooting
- your `$PATH` determines what actually runs when you type a command
- in unix-style systems, everything has a standard place and location matters (as do permissions on said thing)
- you can do essentially _everything_ from the terminal, it's worth being comfortable with it and making it your own
- `escape` + `:wq`
