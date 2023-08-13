# Extended
Extended session files are files with the ending `.cfg`, where each
line is treated according to the following spec:

* Line 1: Session name
* Line 2: Ignored, but required for backward compatibility
* Line 3 ...: Argument line

## Argument line
An argument line can contain any valid tmux command, same as one can
enter them in shell directly (but without the leading tmux).
Additionally the following strings will be replaced:
* SESSION - will be replaced by the session name
* TMWIN - will be replaced by the current window number. This number
  is internally increased every time an argument-line contains the
  string "new-window".

Full Shell expansion, that is both tilde and environment variables, is
supported on argument lines. Details can be taken from the
[shellexpand](https://docs.rs/shellexpand/latest/shellexpand/fn.full.html)
rust crates documentation, but in short: ~ expands to the users
homedir, $VAR and ${VAR} expand to the content of the variable VAR,
and non-existing variables expand to an empty string.

### Example
Line numbers added for clarity in the description later, remove them
if you copy this example!

``` bash
1 simpleexample
2 NONE
3 new-session -d -s SESSION -n SESSION ssh -t localhost 'TERM=xterm sleep 90'
4 split-window -h -p 50 -d -t SESSION:TMWIN ssh -t localhost  'watch -n1 -d date -u'
5 split-window -v -p 70 -d -t SESSION:TMWIN ssh -t localhost  'TERM=xterm htop'
6 set-window-option -t SESSION:TMWIN automatic-rename off
7 set-window-option -t SESSION:TMWIN allow-rename off
```
This example will open a tmux session named `simpleexample`.

* Line 1 is the session name
* Line 2 is ignored
* Line 3 opens a new session, with the name taken from Line 1. It will
  start ssh to localhost with a `sleep 90` running in it.
* Line 4 splits the window, horizontally, with a given size, running a
  `watch` on a `date` command.
* Line 5 splits that window again, vertically, with a given size,
  opening a `htop`.
* Line 6 disallows tmux to rename the session windows
* Line 7 sets another option disabling renames
