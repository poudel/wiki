+++
title = "dwim-shell-commands"
weight = 200
+++

Excellent package to wrap repetitive shell commands into interactive commands.

# Commands

## csv to datasette

I keep having to interact with csv files and using datasette to explore the 
data would really be helpful sometimes.

```lisp
(defun kes-dwim-shell-command-csv-to-sqlite ()
"Create a database file for the given csv file"
(interactive)
(dwim-shell-command-on-marked-files
    "Create sqlite db for csv"
    "sqlite-utils insert '<<fne>>.sqlite' dwim_import '<<f>>' --csv"
    :utils "sqlite-utils"))

(defun kes-dwim-shell-command-open-datasette ()
"Open datasette for a sqlite db file"
(interactive)
(dwim-shell-command-on-marked-files
    "Open datasette on sqlite db"
    "datasette '<<f>>' -o"
    :utils "datasette")
)
```

I can reduce this down to just one command like this

```lisp
(defun kes-dwim-csv-to-datasette ()
"Convert csv into sqlite db and run datasette"
(interactive)

(dwim-shell-command-on-marked-files
    "CSV to SQLITE to Datasette"
    "sqlite-utils insert '<<fne>>.sqlite' dwim_import '<<f>>' --csv && datasette '<<fne>>.sqlite' -o"
    :utils "sqlite-utils"
    :focus-now t))
```

Here, `:focus-now t` just means that the `dwim-shell-command` buffer is brought into focus
so that we can see the output immediately.