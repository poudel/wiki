+++
title = "Homelab"
weight = 20
+++


## ArchLinux Configuration {#configuration}

I've installed cockpit, which turns out to be a fantastic
tool[^fn:4], I still want to setup `ssh` so that I can use `TRAMP`
to edit files remotely. Also, I will probably uninstall cockpit because
what I need can be achieved via ssh.

I'm even thinking about running an Emacs daemon on the server and see
how feasible it is to use that for certain things.


### Add a non-root user {#add-a-non-root-user}

By default `sshd` doesn't allow root to do a password login via ssh.
Also, it's always a good idea to create a separate user for regular
adminstration.

```sh
sudo useradd -m john
```

and set the password

```sh
passwd john
```


### Copy ssh key to the server {#copy-ssh-key-to-the-server}

Nobody uses password login to ssh. Nobody should. So let's copy the
ssh key.

```sh
ssh-copy-id john@<ip>
```


### Sudo {#sudo}

Install vim (or vi) to edit the sudoers file.

```sh
EDITOR=vim visudo
```

Scroll down to the bottom of the file and grant all permissions to
this guy:

```txt
john ALL=(ALL:ALL) ALL
```

Close the file with `:wq`.


### If using docker {#if-using-docker}

Add the user to docker group

```sh
gpasswd -a john <group>
```


### Keep it on when the lid is closed {#keep-it-on-when-the-lid-is-closed}

It's possible to change the behaviour by setting the ACPI event action
which is managed by `logind`, a systemd native service[^fn:5]. Edit the
`logind.conf` file:

```sh
vim /etc/systemd/logind.conf
```

Find the `HandleLidSwitch` event handler and set the action
to `ignore`.

```txt
HandleLidSwitch=ignore
```

Then restart logind:

```sh
systemctl kill -s HUP systemd-logind
```

Works like a charm!

[^fn:1]: I was also using a very slow DSL line which was metered. It capped to 128kbps if you hit daily quota.
[^fn:2]: That holds true even today.
[^fn:3]: But not confident enough to fully eschew it.
[^fn:4]: It reminds me of Webmin and other tools from the past and makes me slightly nostalgic.
[^fn:5]: <https://wiki.archlinux.org/title/Power_management#ACPI_events>
