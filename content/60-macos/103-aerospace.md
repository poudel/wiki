+++
title = "Aerospace"
weight = 103
description = "Aerospace is a window manager for MacOS"
+++

I've never been a tiling window manager person. On Linux, I've always
used KDE.

But after enduring the default macos window manager, and flirting with
many different tiling window managers over the last 4 years, I've
finally found something that I might actually stick to:
[aerospace](https://nikitabobko.github.io/AeroSpace).

Aerospace's killer feature is the virtual workspace that it comes
with. It's worth switching just for that.

# Key mapping

The default keymaps are probaly good for most people but they conflict
a lot with my existing workflow, especially with my Emacs keybindings.


So I decided to finally start using the Hyper key. I'm using the
[Hyperkey](https://hyperkey.app/) app to bind my right Command to
Hyper key, but I'll probably switch to the
[Karabiner-Elements](https://karabiner-elements.pqrs.org/) in the
future.

When I actually started remapping things to `hyper`, I learned that
Aerospace doesn't support it. I found a [pull
request](https://github.com/nikitabobko/AeroSpace/pull/389) that was
adding `hyper` but it was closed. I was getting disheartened. Reading
through the pull request comments led me to [this
comment](https://github.com/nikitabobko/AeroSpace/issues/224#issuecomment-2420161436)

>alt-shift-cmd-ctrl-w = '...' works for me (AeroSpace v0.14.2-Beta
>0cb8dbd on Sonoma 14.7)

and bingo! that works for me as well. That seems obvious but I didn't
think of that.

I decided to go with a `hyper` without the `shift` so as to have one
modifier layer with `hyper`.

# Current config

This is my config right now, but I'm sure it'll go through a few
transformations. I want to have a setup ready when I open my laptop so
I'm gonna add a few things to the `after-startup-command` list.

```toml
# Place a copy of this config to ~/.aerospace.toml
# After that, you can edit ~/.aerospace.toml to your liking

# You can use it to add commands that run after login to macOS user session.
# 'start-at-login' needs to be 'true' for 'after-login-command' to work
# Available commands: https://nikitabobko.github.io/AeroSpace/commands
after-login-command = []

# You can use it to add commands that run after AeroSpace startup.
# 'after-startup-command' is run after 'after-login-command'
# Available commands : https://nikitabobko.github.io/AeroSpace/commands
after-startup-command = []

# Start AeroSpace at login
start-at-login = true

# Normalizations. See: https://nikitabobko.github.io/AeroSpace/guide#normalization
enable-normalization-flatten-containers = true
enable-normalization-opposite-orientation-for-nested-containers = true

# See: https://nikitabobko.github.io/AeroSpace/guide#layouts
# The 'accordion-padding' specifies the size of accordion padding
# You can set 0 to disable the padding feature
accordion-padding = 30

# Possible values: tiles|accordion
default-root-container-layout = 'tiles'

# Possible values: horizontal|vertical|auto
# 'auto' means: wide monitor (anything wider than high) gets horizontal orientation,
#               tall monitor (anything higher than wide) gets vertical orientation
default-root-container-orientation = 'auto'

# Mouse follows focus when focused monitor changes
# Drop it from your config, if you don't like this behavior
# See https://nikitabobko.github.io/AeroSpace/guide#on-focus-changed-callbacks
# See https://nikitabobko.github.io/AeroSpace/commands#move-mouse
# Fallback value (if you omit the key): on-focused-monitor-changed = []
on-focused-monitor-changed = ['move-mouse monitor-lazy-center']
on-focus-changed = 'move-mouse window-lazy-center'

# You can effectively turn off macOS "Hide application" (cmd-h) feature by toggling this flag
# Useful if you don't use this macOS feature, but accidentally hit cmd-h or cmd-alt-h key
# Also see: https://nikitabobko.github.io/AeroSpace/goodness#disable-hide-app
automatically-unhide-macos-hidden-apps = true

# Possible values: (qwerty|dvorak)
# See https://nikitabobko.github.io/AeroSpace/guide#key-mapping
[key-mapping]
preset = 'qwerty'

# Gaps between windows (inner-*) and between monitor edges (outer-*).
# Possible values:
# - Constant:     gaps.outer.top = 8
# - Per monitor:  gaps.outer.top = [{ monitor.main = 16 }, { monitor."some-pattern" = 32 }, 24]
#                 In this example, 24 is a default value when there is no match.
#                 Monitor pattern is the same as for 'workspace-to-monitor-force-assignment'.
#                 See: https://nikitabobko.github.io/AeroSpace/guide#assign-workspaces-to-monitors
[gaps]
inner.horizontal = 4
inner.vertical =   4
outer.left =       5
outer.bottom =     5
outer.top =        5
outer.right =      5

# 'main' binding mode declaration
# See: https://nikitabobko.github.io/AeroSpace/guide#binding-modes
# 'main' binding mode must be always presented
# Fallback value (if you omit the key): mode.main.binding = {}
[mode.main.binding]

# All possible keys:
# - Letters.        a, b, c, ..., z
# - Numbers.        0, 1, 2, ..., 9
# - Keypad numbers. keypad0, keypad1, keypad2, ..., keypad9
# - F-keys.         f1, f2, ..., f20
# - Special keys.   minus, equal, period, comma, slash, backslash, quote, semicolon, backtick,
#                   leftSquareBracket, rightSquareBracket, space, enter, esc, backspace, tab
# - Keypad special. keypadClear, keypadDecimalMark, keypadDivide, keypadEnter, keypadEqual,
#                   keypadMinus, keypadMultiply, keypadPlus
# - Arrows.         left, down, up, right

# All possible modifiers: cmd, alt, ctrl, shift

# All possible commands: https://nikitabobko.github.io/AeroSpace/commands

# See: https://nikitabobko.github.io/AeroSpace/commands#exec-and-forget
# You can uncomment the following lines to open up terminal with alt + enter shortcut (like in i3)
# alt-enter = '''exec-and-forget osascript -e '
# tell application "Terminal"
#     do script
#     activate
# end tell'
# '''


# See: https://nikitabobko.github.io/AeroSpace/commands#layout
alt-slash = 'layout tiles horizontal vertical'
alt-comma = 'layout accordion horizontal vertical'

# 
# Hyper: alt-cmd-ctrl
# Hyper+Shift: alt-cmd-ctrl-shift
#

# See: https://nikitabobko.github.io/AeroSpace/commands#focus
alt-cmd-ctrl-j = 'focus left'
alt-cmd-ctrl-k = 'focus down'
alt-cmd-ctrl-i = 'focus up'
alt-cmd-ctrl-l = 'focus right'

# See: https://nikitabobko.github.io/AeroSpace/commands#move
alt-cmd-ctrl-shift-j = 'move left'
alt-cmd-ctrl-shift-l = 'move right'
alt-cmd-ctrl-shift-k = 'move down'
alt-cmd-ctrl-shift-i = 'move up'

# See: https://nikitabobko.github.io/AeroSpace/commands#resize
alt-cmd-ctrl-minus = 'resize smart -50'
alt-cmd-ctrl-equal = 'resize smart +50'

# See: https://nikitabobko.github.io/AeroSpace/commands#workspace
# Switch to workspaces using 
alt-cmd-ctrl-1 = 'workspace 1'
alt-cmd-ctrl-2 = 'workspace 2'
alt-cmd-ctrl-3 = 'workspace 3'
alt-cmd-ctrl-4 = 'workspace 4'

# See: https://nikitabobko.github.io/AeroSpace/commands#move-node-to-workspace
alt-cmd-ctrl-shift-1 = 'move-node-to-workspace 1'
alt-cmd-ctrl-shift-2 = 'move-node-to-workspace 2'
alt-cmd-ctrl-shift-3 = 'move-node-to-workspace 3'
alt-cmd-ctrl-shift-4 = 'move-node-to-workspace 4'

alt-cmd-ctrl-f = 'fullscreen'

# See: https://nikitabobko.github.io/AeroSpace/commands#workspace-back-and-forth
alt-tab = 'workspace-back-and-forth'
# See: https://nikitabobko.github.io/AeroSpace/commands#move-workspace-to-monitor
alt-shift-tab = 'move-workspace-to-monitor --wrap-around next'

# See: https://nikitabobko.github.io/AeroSpace/commands#mode
alt-shift-semicolon = 'mode service'

# 'service' binding mode declaration.
# See: https://nikitabobko.github.io/AeroSpace/guide#binding-modes
[mode.service.binding]
esc = ['reload-config', 'mode main']
r = ['flatten-workspace-tree', 'mode main'] # reset layout
f = ['layout floating tiling', 'mode main'] # Toggle between floating and tiling layout
backspace = ['close-all-windows-but-current', 'mode main']

# sticky is not yet supported https://github.com/nikitabobko/AeroSpace/issues/2
#s = ['layout sticky tiling', 'mode main']

alt-cmd-ctrl-j = ['join-with left', 'mode main']
alt-cmd-ctrl-k = ['join-with down', 'mode main']
alt-cmd-ctrl-i = ['join-with up', 'mode main']
alt-cmd-ctrl-l = ['join-with right', 'mode main']
```

# Links

* This video was really helpful <https://www.youtube.com/watch?v=-FoWClVHG5g>
