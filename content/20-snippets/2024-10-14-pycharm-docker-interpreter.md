+++
title = "Pycharm not finding Docker interpreter"
date = 2024-10-14
tags = ["python", "docker", "tools"]
+++

I use Pycharm Professional for work. Last week, I updated it to the latest 
version (2024.2.3) and with that I also lost the ability to run tests via docker compose.

Pycharm just couldn't find the interpreter. It frustrated me to no end because it was
working just fine before the update.

I crawled through that by running `pytest` locally because I had something I needed
to finish. But the lack of a debugger meant that I had to sprinkle `breakpoint()`
everywhere like I used to do. Ugh!

First, I thought it had something to do with the license. I know Docker support is only
available in the Pro version. That I could rule out easily because I have an active license.

Next, I disabled and enabled the Docker and Dev container plugins. That didn't help at all.

I dug a little deeper and came across this comment on a community thread[^1]

>[..] it seems you need to enable the FTP/SFTP/WebDAV Connectivity plugin: https://youtrack.jetbrains.com/issue/PY-71931

And that was it. Problem solved!

Seems like there's a dependency on this FTP/SFTP/WebDAV plugin but it's not made clear in any way.

## Footnotes

[^1] Can't see the Docker option <https://intellij-support.jetbrains.com/hc/en-us/community/posts/4403483524242-Can-t-see-the-Docker-option-under-python-Interpreter-to-connect-remotely>
