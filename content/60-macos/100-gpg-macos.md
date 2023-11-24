+++
title = "GPG on MacOS"
weight = 100
+++

## Setting up GPG {#gpg-macos}

Recently I had to setup GPG on Mac to be able to encrypt/decrypt
certain files. I'd used GPG tools a long time ago but never liked
the way it works. For example it automatically imports some default
keys on install, which seems unnecessary to me.

Turns out it's not that difficult to install GPG using `brew` and get
it working.

## Keep in mind

### Set an expiry date {#always-set-an-expiry-date}

Make sure that keys are always expirable. They can be renewed later
on. Or if you lose it then you don't have to worry about it for long.


### Keep master key separate {#always-master-key-separate}

This is the main key that has `SC` permission/usages when you run `gpg
--list-secret-keys`. The `C` part means that it can issue/certify new
subkeys.

This key should be kept in a secure location and should only be used
to issue new subkeys or revoke/renew old ones.


### Use subkeys {#always-use-subkeys}

Instead of using your master key for encryption and signing, it's
better to use the subkeys so that you can revoke them easily if you
lose them.


### Encrypting vs signing keys {#encrypting-vs-signing-keys}

Encrypting keys have `E` permission and signing have `S`. You use the
former for encryption and the latter for singing exclusively.


## Steps {#steps}


### Generating key pairs {#generating-key-pairs}

Returns an interactive prompt that guides you through creating a key
pair.

```sh
gpg --full-gen-key
```

There's a question of whether to chose RSA
or ed25519. The latter is selected by default.


### Generating Subkeys for regular usage {#generating-subkeys-for-regular-usage}

This allows us to easily to issue/revoke
certain keys if they're compromised without
throwing away the master key.

Get the key ID

```sh
gpg --list-secret-keys
```

The line beginning with `sec` is the master key. It has `[SC]` which
is Signing and Certification. For the line `ssb`, the `[E]` indicates
that it's an encryption subkey. However, we're going to create a seprate
signing subkey. Start by editing the key.

```sh
gpg --edit-key <key-id-long-very-long>
```

In the interactive gpg shell, enter `addkey` command.

```txt
addkey
```

You'll see:

```txt
Please select what kind of key you want:
   (3) DSA (sign only)
   (4) RSA (sign only)
   (5) Elgamal (encrypt only)
   (6) RSA (encrypt only)
  (10) ECC (sign only)
  (12) ECC (encrypt only)
  (14) Existing key from card
Your selection?
```

Select option 10 and follow from there.


### Exportng keys to backup {#exportng-keys-to-backup}

We export the keys and make a backup. Then we delete the master key
from the keyring and import the subkeys.

```sh
gpg --output keshab.rsa4096.public.gpg --export <KEY-ID>
gpg --output keshab.rsa4096.secret.gpg --export-secret-key <KEY-ID>
gpg --output keshab.rsa4096.secretsubkeys.gpg --export-secret-subkeys <KEY-ID>
```

Now backup those files in a secure location.


### Deleting exported keys from the GPG keyring {#deleting-exported-keys-from-the-gpg-keyring}

To delete the keys, run:

```sh
gpg --delete-secret-keys <KEY-ID>
```

And follow the instructions. Run `gpg --list-secret-keys` to confirm
the deletion.


### Import only the subkeys {#import-only-the-subkeys}

Finally, import only the subkeys:

```sh
gpg --import keshab.rsa4096.secretsubkeys.gpg
```

and follow the instructions. Run `gpg --list-secret-keys` again to
confirm the import worked.

```sh
gpg --list-secret-keys
```

The suffix hash on `sec#` suggests that only subkeys are available.


### Trust a key {#trust-a-key}

This is not needed for most cases, esp. if the key was generated in
this computer. An imported key can be "trusted" by doing the
following.

```sh
gpg --edit-key <email/key-id>
```

Then in the interactive shell, type `trust` command.

```txt
Command> trust
```

You'll be prompted to select how far you want to trust the key.


### Using Emacs to edit `.gpg` files {#using-emacs-to-edit-dot-gpg-files}

Add following in `~/.gnupg/gpg-agent.conf` to use Emacs for pinentry
and not the external dialogue.

```txt
allow-emacs-pinentry
allow-loopback-pinentry
```

Kill the GPG agent by running

```sh
gpgconf --kill gpg-agent
```

Then add this to the init file to select the default encrypting key and recipent.

```el
(setq epa-pinentry-mode 'loopback  ;; use emacs
      epa-file-encrypt-to "self@keshab.net" ;; encryption recipent
      epa-file-select-keys "self@keshab.net" ;; encryption key
      )
```


### Using signing keys to sign git commits {#using-signing-keys-to-sign-git-commits}

Get long key ID

```sh
gpg -K --keyid-format=long
```

Configure git to use this for signing, go to the git repo and run.

```sh
git config user.signingkey <LONG-KEY-ID>
```

The long key ID is of the signing subkey. Remember to not use your
master key.

Use `--global` flag to make this apply to all repos.


#### Let zsh know about GPG {#let-zsh-know-about-gpg}

```sh
if [ -r ~/.zshrc ]; then echo 'export GPG_TTY=$(tty)' >> ~/.zshrc; \
else echo 'export GPG_TTY=$(tty)' >> ~/.zprofile; fi
```


#### Install pinentry-mac for password inputs {#install-pinentry-mac-for-password-inputs}

This doesn't seem necessary when using Emacs for pin entry but is
needed if creating signed commits from the command line.

```sh
brew install pinentry-mac
echo "pinentry-program $(which pinentry-mac)" >> ~/.gnupg/gpg-agent.conf
killall gpg-agent
```

:results:
nil:END:


#### Sign commits {#sign-commits}

```sh
git commit -S -m "something"
```

and to autosign all future commits, run:

```sh
git config commit.gpgsign true
```


#### Add it to github/gitlab {#add-it-to-github-gitlab}

Export the key as ASCII armor format

```sh
gpg --armor --export <KEY-ID>
```


## Troubleshooting {#troubleshooting}

2023-04-03 I received this error when trying to do a `git commit`. My
key wasn't expired though, which I verified by running the key listing
command.

```txt
error: gpg failed to sign the data
fatal: failed to write commit object
```

I tried committing with GIT_TRACE=1 environment variable.

```sh
GIT_TRACE=1 git commit
```

Which printed the failing `gpg` command.

```txt
gpg --status-fd=2 -bsau <key-id>
```

Running this command didn't throw any error though.

I'd just logged out and logged in onto the machine, maybe that broke something? I then killed gpg-agent on a whim.

```sh
gpgconf --kill gpg-agent
```

Now it's working. Not sure what was up.


2023-11-24 Saving .gpg files would hang in Emacs. Found [this
question](https://stackoverflow.com/q/76388376) in stackoverflow that
suggests downgrading the `gpg` package.

```sh
brew uninstall --ignore-dependencies gpg
```

and then 

```sh
brew install gnupg@2.2
```



## Links {#links}

-   <https://mikeross.xyz/create-gpg-key-pair-with-subkeys/>
-   <https://mikeross.xyz/how-to-sign-git-commits-with-gpg-key/>
-   [How to understand the \`gpg failed to sign the data\` problem in git](https://gist.github.com/paolocarrasco/18ca8fe6e63490ae1be23e84a7039374) by paolocarrasco
