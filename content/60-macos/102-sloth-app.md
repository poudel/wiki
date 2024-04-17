+++
title = "Sloth app"
weight = 102
+++

Came across this interesting MacOS app called [Sloth](https://sveinbjorn.org/sloth) today.
To quote the website,

>Sloth is essentially a friendly, exploratory GUI built on top of the lsof command line tool. 

To install it, I ran the following command

```text
brew install --cask sloth
```

The binary is not signed so MacOS refused to open it the first time around. I went to 
the Security settings and clicked on "Open anyway". 

The app shows you a list of files, directories, devices, pipes, ip/unix sockets opened 
by running apps. It's really interesting to see all the IP sockets opened by running processes.

Definitely going to keep this around.