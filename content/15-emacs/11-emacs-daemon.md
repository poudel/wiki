+++
title = "Emacs Daemon"
weight = 11
slug = "emacs-daemon"
+++

I hit an error when trying to reinstall the
[emacs-plus](@/15-emacs/10-using-emacs-plus.md) homebrew package
recently. So I'm back to using the
[Emacs.app](https://emacsformacosx.com/) package for now.

I have workflow around running Emacs as a daemon and invoking
`emacsclient`. So I need to run emacs server.

The process is slightly different. I found this [Setting up Emacs
daemon on OS
X](https://gist.github.com/redinger/1394910/c0dc6877c158a13760212dbe10b4abb32ae3c4f8)
gist and followed it.

Copy of the apple script that I saved as Emacs Daemon inside my
Applications directory.

```
tell application "Terminal"
do shell script "/Applications/Emacs.app/Contents/MacOS/Emacs --daemon"
endtell
```

```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" 
        "http://www.apple.com/DTDs/PropertyList-1.0.dtd"> 
     <plist version="1.0">
      <dict> 
        <key>Label</key>
        <string>gnu.emacs.daemon</string>
        <key>ProgramArguments</key>
        <array>
          <string>/Applications/Emacs.app/Contents/MacOS/Emacs</string>
          <string>--daemon</string>
        </array>
       <key>RunAtLoad</key>
       <true/>
       <key>ServiceDescription</key>
       <string>Gnu Emacs Daemon</string>
      </dict>
    </plist>
```

