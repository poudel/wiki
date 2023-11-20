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

# csv to json

```lisp
(defun kes-dwim-shell-command-csv-to-json ()
  "Read large csv file and convert it to a json file"
  (interactive)
  (dwim-shell-command-on-marked-files
    "Convert csv to json file"
    "
import csv
import json
import sys
csv.field_size_limit(sys.maxsize)   # sometimes the csv is too large

rows = csv.DictReader(open('<<f>>'))
newrows = []
for row in rows:
    newrow = {}
    for key, value in row.items():
        try:
            newvalue = json.loads(value)    # sometimes the values might be json strings
#            if not isinstance(newvalue, (dict, list)):
                # don't care about scalar values that might be valid json
#                newvalue = value
        except Exception:
            newvalue = value
        newrow[key] = newvalue
    newrows.append(newrow)
as_json = json.dumps(newrows, indent=2)
filename = '<<fne>>.json'
with open(filename, 'w') as f:
    f.write(as_json)"
     :shell-util "python3"
     :shell-args "-c"
     :silent-success t))
```
