+++
title = "ThinkPad T470 for Homelab"
date = 2023-03-18
tags = ["blog", "linux", "macos", "emacs"]
+++

I bought an old ThinkPad last year so that I could tinker with
Linux. I was a Linux user for a long time before I switched over to
MacOS a couple of years ago.

The first thing I did after receiving the machine was to install
ArchLinux. I also installed KDE, because it's what I used before. I
installed all other tools that I use for my day to day. I also enabled
full disk encryption.

The original plan was to use this as a personal computer in tandem
with my Macbook Air. I also imagined it as a travel laptop that I
could be comfortable with leaving in hotel rooms as I explored the
city. Even if it did get stolen, it's a fairly inexpensive purchase
compared the Macbook. Full disk encryption ensures that nobody has
access to my personal or work data.

But after more than 6 months of owning it, I can't say that I've
utilised this laptop as much as I'd hoped. As a travel laptop this
never became my first choice because of its weight; it's too heavy by
today's standards. Since I'm always looking to reduce the weight of my
backpack when going on a trip, this one got filtered out without even
a consideration.

So I've decided to repurpose this as a home server. I've been meaning
to self host a few things for personal purposes, like a document
management system, media server, and perhaps something to store
bookmarks and personal web archive. This would be the perfect little
device for that. The hardware is more than engouh for this need.


## Oops! Re-install! {#oops-re-install}

When I turned the machine on today, it refused to boot, complaining
about missing `vmlinuz-linux` file. The only thing I could remember
doing was long-pressing the power button because it was taking too
long to power off. That was a couple of months ago.

No worries I still have the bootable USB from before. I just need to
chroot and run `pacman -S mkinitcpio`.

I boot with the installation media and get ready to mount the drive.
But there's just one small problem. I can't seem to remember the
encryption key for the disk. Thankfully there was nothing of
importance stored there. Time to reinstall.

I plug in the same ethernet cable from last time which is still
attached to the router and run `archinstall`. I decided to pick a
`server` install profile because I don't want to use this as a regular
laptop. If that changes later, I can easily install a UI.

After that, it was simply a matter of waiting. I think it took about
10 minutes, maybe a little less. I wasn't even paying attention to the
screen while this was running.


## Thoughts on `archinstall` {#thoughts-on-archinstall}

I must say that I really enjoyed using `archinstall`. For a very long
time Arch didn't come with such installation script. The community
resisted the idea of one because an automated installer was perceived
to be against the arch way. One of the tenets is that the user should
know the inner workings of their system.

Having no installer also made it a great opportunity for a beginner to
learn Linux. You make a ton of mistakes and in doing so, learn a lot.

The first time I installed Arch, I stayed up the whole night because I
couldn't get the wifi to work[^fn:1]. And I also didn't know what I was
doing[^fn:2]. But I persisted and was able to get a working setup by
morning. The feeling of accomplishment topped many others before that.

Since then I have done it countless times. I knew the steps enough to
not even need the guide[^fn:3].

And after you've done it for as many times as I have, you'll know
enough to get the setup that you want, and there's not many new things
to learn from doing the installation processes again and again. I just
want to get it done in as fewer steps as possible.

The addition of `archinstall` in the default install image is
excellent. It is simple, minimal and it is compatible with the Arch
philosophy. It stays out of your way, you have to run the command
yourself to bring it up. It automates the cumbersome and error prone
steps so that you can quickly choose what you need and move on.

It doesn't make decisions for you like other installers do. That's
probably why it's not a "wizard" like thing. It's more of a
configuration editor.

I would still recommend the classic way if you're a beginner. Because
that's one of the quickest way to learn the inner workings of a linux
system. The knowledge you gain from it is transferrable to other linux
distributions, and even to other Unix like operating systems with some
major differences.

That's it for today. I've already accrued about 2 hours of uptime as
I'm finishing this sentence. I still need to set up the services that
I want. And I need to figure out a way to keep the screen off while
keeping the machine running; that's probably not very difficult.


## Configuration {#configuration}

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

```text
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

```text
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