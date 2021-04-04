# SSH (Secure Shell)

SSH is a means of connecting securely to remote servers. We'll go over some basics and get set up to connect and authenticate to github servers via ssh.

Most systems will have a directory for `ssh` config and keys at `~/.ssh`. If it doesn't exist, you can create one yourself. 

```
$ tree ~/.ssh
/home/ubuntu/.ssh
├── authorized_keys     List of public keys for users with access to this login
├── config              Config for the ssh tools (has a specific format, see more below)
├── id_rsa              Default private key (this is used by default, but you can create more or specify later)
├── id_rsa.pub          Default public key (this is the one that can leave your machine and used to recognize you)
└── known_hosts         List of recognized machines (have connected to in past)
```

## SSH Keys

SSH keys come in a pair, one public, one private. You can think of these as a lock and a key. You keep the key secret and then put your lock (the public keys) on things that you want to access. You can also think of this keypair (taking some liberties here) as a username (public) and password (private).

What's really going on under the hood? The ssh public key is a block of encrypted data that the private key knows how to decrypt. It relies on the factoring of prime numbers being computationally very expensive.

In general, you will make one key-pair for a machine and use that to identify (authenticate) yourself to other resources. However, you can manage as many key pairs as you like, the main thing is that private keys should generally not leave the machine on which they were created.

### How do I make one?

Most systems come with a tool for that: `ssh-keygen`

This is an interactive tool that will supply some defaults to help you create a key pair.

1. Location: Keys wil be kept in `~/.ssh/` by default and named `id_rsa`
2. Passphrase: A passphrase is recommended to keep your private key secure (this makes it MFA - something you know + something you have).

### Using your key

We'll test our keys out by authenticating to GitHub.

### Upload your key to GitHub

1. Login to github.com from the browser
2. Go to your account settings (top right icon > "Settings")
3. **SSH and GPG Keys** (from the menu bar at left)
4. **New SSH Key** (top left button)
5. Give it a meaningful title to remember where it came from (like "Macbook Air - Personal")
6. Print the public key (`cat ~/.ssh/id_rsa.pub`) and copy/paste it into **Key** section on GitHub
7. Finish up with **Add SSH Key** button

> If you have trouble copy/pasting from the terminal (`Ctrl`+`C` is a special keystroke if you remember), try `Ctrl`+`Ins`/`Shift`+`Ins` to copy/paste. You can also change your settings in Git Bash to allow `Ctrl`+`Shift`+`C/V` for copy/paste (just right-click the title bar and go to Options > Keys > Ctrl + Shift + letter shortcuts).

### Test your key

GitHub provides an easy way to test new keys: see [Testing your SSH key](https://docs.github.com/en/github/authenticating-to-github/testing-your-ssh-connection)

1. Back in a terminal, `ssh -T git@github.com`

You may see a warning like "The authenticity of host 'github.com (IP ADDRESS)' can't be established." - don't worry about this, it just warns you that you're connecting to something your computer hasn't seen before.

If everything works, you should see:

```
> Hi username! You've successfully authenticated, but GitHub does not
> provide shell access.
```

We'll use this key more later when we cover git and GitHub in more depth.

## SSH connection string

Earlier, we ran `ssh -T git@github.com`; let's take a look at that as well as more complex ssh commands.

First, the `ssh` command can of course be explored in more detail via `man ssh`. But we can go ahead and talk about some of the more common options and sub-commands.

(`-T` in this case just tests the connection (does not attempt to open an interactive session))

The argument to ssh is a resource identifier in the form of `<USER>@<HOSTNAME>`. I can log into my raspberry pi as `ubuntu@10.0.0.17` which is a default username and the static IP address assigned to that device on my home network. You may connect to a Northwestern server with something like `smf2659@wolf.private.analytics.northwestern.edu` (not sure if that's still right).

By default, ssh will use `~/.ssh/id_rsa` to "unlock" that resource for you. You can easily specify a different key with `-i`

```
ssh -i ~/.ssh/keys/some_other_key ec2-user@ec2-123-45-678-90.compute.amazonaws.com
```

If you're interested in seeing more of what goes on behind the scenes when you ssh, you can add the `-v` flag (for `v`erbose)

## SSH Config

Remembering a bunch of server names is hard... even harder is remembering which user and ssh key should be used with each server. Luckily there's an easy way to deal with this!

SSH uses a config file stored in `~/.ssh/config` by default and this is a **very** handy file. In this file, we can define nicknames for all the machines we may connect to and attach the right config to those nicknames (user, hostname, ssh key, connection settings, etc.).

Below is a very simple ssh config example:

```config
Host mfpi
  HostName 10.0.0.9
  User ubuntu
```

At a bare minimum, you must specify:

- `Host`: this is the "nickname" that you will use to identify this resource
- `HostName`: this is the actual address of the server (can be an IP Address or a Domain Name)
- `User`: this is the user you will attempt to log in as

Additionally, some common options include:

- `Port`: usually you'll connect on `22` but you can change this if the server is configured differently.
- `IdentityFile`: `~/.ssh/id_rsa` will be used by default but you can assign a different key if needed.
- `LocalForward`: this will allow you to forward your local port traffic to the remote port specified - useful for seeing a web server running remotely using a browser on your local machine.
- `ForwardAgent`: this will pass your local ssh agent (with ssh keys) to the remote machine so that you can then connect to yet another resource without moving keys off of your machine. 

All options can be found by running `man ssh_config`

## SSH Agent

Above we mentioned ssh agent when describing the `ForwardAgent` setting - what's this? Your ssh agent allows you to keep a set of "unlocked" keys handy in your session so that you don't have to enter your passphrase every time you use it. This is when ssh keys become extra useful!

If you're on linux, this should be pretty straightforward:

1. Run ``eval `ssh-agent` `` (those are back-ticks; below the `~` on most keyboards)
2. Run `ssh-add` to add your default keys to the agent (use the `-t` flag to specify an expiration in seconds)
3. Enter your passphrase to "unlock" the key for your agent
4. Use your ssh key as you normally would but without being prompted for a password again

On Mac, your default ssh keys are actually handled by default with the system's "Keychain" tool - you can use this, it's safe.

On Windows, if you're using git-bash, you can also use `ssh-agent`; if you want this to run by default, see this helpful article from GitHub: [Set up ssh agent on Windows](https://docs.github.com/en/github/authenticating-to-github/working-with-ssh-key-passphrases)
