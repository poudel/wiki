+++
title = "Trying Nix on MacOS"
weight = 101
+++

Here's what I'm doing to try nix.


# Installation

Ran this command from the [Nixos download](https://nixos.org/download) page.

```sh
sh <(curl -L https://nixos.org/nix/install)
```

What follows is a really nice and friendly series of prompts. The
verbosity is perfect. For example:


```txt
Switching to the Multi-user Installer
Welcome to the Multi-User Nix Installation

This installation tool will set up your computer with the Nix package
manager. This will happen in a few stages:

1. Make sure your computer doesn't already have Nix. If it does, I
   will show you instructions on how to clean up your old install.

2. Show you what I am going to install and where. Then I will ask
   if you are ready to continue.

3. Create the system users (uids [301..332]) and groups (gid 30000)
   that the Nix daemon uses to run builds.

4. Perform the basic installation of the Nix files daemon.

5. Configure your shell to import special Nix Profile files, so you
   can use Nix.

6. Start the Nix daemon.

Would you like to see a more detailed list of what I will do?
[y/n] y
```

I go with `y`, which makes it spit the following

```txt
I will:

 - make sure your computer doesn't already have Nix files
   (if it does, I will tell you how to clean them up.)
 - create local users (see the list above for the users I'll make)
 - create a local group (nixbld)
 - install Nix in to /nix
 - create a configuration file in /etc/nix
 - set up the "default profile" by creating some Nix-related files in
   /var/root
 - back up /etc/bashrc to /etc/bashrc.backup-before-nix
 - update /etc/bashrc to include some Nix configuration
 - back up /etc/zshrc to /etc/zshrc.backup-before-nix
 - update /etc/zshrc to include some Nix configuration
 - create a Nix volume and a LaunchDaemon to mount it
 - create a LaunchDaemon (at /Library/LaunchDaemons/org.nixos.nix-daemon.plist) for nix-daemon

Ready to continue?
[y/n] y
```

After a few similar prompts, I'm presented with the following:

```txt
[...]

Alright! We're done!
Try it! Open a new terminal, and type:

  $ nix-shell -p nix-info --run "nix-info -m"

Thank you for using this installer. If you have any feedback or need
help, don't hesitate:

You can open an issue at
https://github.com/NixOS/nix/issues/new?labels=installer&template=installer.md

Or get in touch with the community: https://nixos.org/community

---- Reminders -----------------------------------------------------------------
[ 1 ]
Nix won't work in active shell sessions until you restart them.
```

So yeah, the nix-daemon is alive and kicking and I have a `nix-shell`
that I need to invoke. Everything went well. Running the command for
the first time fetched the required packages and then printed the
following.

```txt
 - system: `"aarch64-darwin"`
 - host os: `Darwin 22.5.0, macOS 13.4.1`
 - multi-user?: `yes`
 - sandbox: `no`
 - version: `nix-env (Nix) 2.19.1`
 - channels(root): `"nixpkgs"`
 - nixpkgs: `/nix/var/nix/profiles/per-user/root/channels/nixpkgs`
```

# First package

I decided to install `zola` as the first ever package with nix. I ran
the following:

```sh
nix-env -iA nixpkgs.zola
```

Running `which zola` gave me the following

```txt
/Users/kes/.nix-profile/bin/zola
```

# More packages

Currently I'm not using the nix lang to define my packages. I intend
to do that at a later point i.e. after getting more comfortable with
it.



# Links

* [Download nixos](https://nixos.org/download)
* [Some notes on using nix](https://jvns.ca/blog/2023/02/28/some-notes-on-using-nix/) - jvns
