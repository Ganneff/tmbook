# Commandline
As tm started out as a small shell script, not much thought was given
to the way of calling it, it just accumulated options. That is what is
now labeled `traditional` style. Some of those, like the `s` and `ms`
sub-commands are still the most used arguments to tm - typing their
`getopt` equivalents of `-s` and `-m` is just more/less nice to type.

## Common beheaviour
No matter if one uses traditional or getopt style, common options like
`-v`/`-q`/`-h` as well as their long versions
`--verbose`/`--quiet`/`--help` behave the same.

# Log output
tm includes extensive logging. To see it, use the `-v`/`--verbose`
switch. Use it more often for more detailed output - with *TRACE*
level being the most detailed one, including tracing of function
calls and timing information for them.

Default is to only output error messages.

| Count of `-v` | Output detail            |
|---------------|--------------------------|
|               | Errors only              |
| -v            | Above plus Warnings      |
| -vv           | Above plus Informational |
| -vvv          | Above plus Debug         |
| -vvvv         | Above plus Trace         |

Usually a `-vvv` is really helpful to debug new session files.

## Traditional (subcommand) vs getopt calling style
The traditional calling style (subcmmand style), some of it dating
back to the first shell script version, is easiest for some options.
Say, a `tm s host` is less to type than `tm -s host`, same for
`ms|-ms`, though that may be personal preference too. A few options
are only available using getopt style, for example the `-g` or `-n`.

### Overview
As of tm version 0.9 the following commands/options are supported,
explained in more detail below:

| Subcommand | Getopt  | Intended action                                                                |
|------------|---------|--------------------------------------------------------------------------------|
| ls         | -l      | List running sessions                                                          |
| s          | -s      | Open SSH session to the destination                                            |
| ms         | -ms     | Open multi SSH sessions to hosts, synchronizing input                          |
| k          | -k      | Kill a session                                                                 |
| b          | -b      | Break a multi-session pane into single windows                                 |
| j          | -j      | Join multiple windows into one single one with many panes                      |
| help       | -h      | Print this message or the help of the given subcommand(s)                      |
|            | -v[vvv] | Verbosity, the more v, the more log output                                     |
|            | -n      | Open second session to the target, do not attach to existing                   |
|            | -g      | Group session - attach to an existing session, but keep seperate window config |
|            |         |                                                                                |

#### ls / -l
Same as `tmux ls`, simply list the existing tmux sessions.

#### s / -s [HOSTS]
This will open a new tmux session and create multiple windows, one per
argument - the windows will be using ssh to connect to the given
host(s). It's arguments can be anything that your SSH config will
accept, this can range from simple hostnames to user@host or whatever
possible matches you may have configured in your *~/.ssh/config* file.

Multiple arguments will open multiple tmux windows in the same
session.

#### ms / -ms [HOSTS]
This will open a new tmux session and create multiple panes in the
first window, one per argument. The panes will be using ssh to connect
to the given hosts. It's arguments can be anything that your SSH
config will accept, this can range from simple hostnames to user@host
or whatever possible matches you may have configured in your
*~/.ssh/config* file.

When all panes are opened the tmux switch **synchronize-pane** will be
toggled on, so that anything input will be sent to all hosts at the
same time.

This subcommand will check if tmux has been able to open the pane, and
if not it will tell tmux to rearrange the layout, to "gain" some space
again. At the end it will select the **tiled** layout, so that all
panes will roughly have the same size.

#### k /-k [SESSION]
This basically translates to `tmux kill-session` and will kill the
session with the given name.

#### b /-b [SESSION]
This is used to **break** a session built using
[ms](commandline.md#ms) or a session file (or even manually) into
seperate windows. It will take the given session name and split all
panes in the window included into seperate tmux windows.

#### j / -j [SESSION]
This is used to **join** a multi-window session to one that closely
resembles one build using the [ms](commandline.md#ms) command. It will
take the given session name and join all windows to the first window,
as such making them panes of the first window. Afterwards it will
toggle the **synchronize-pane** option of that window on, so any input
will be send to all panes.

#### -n
This is only valid as an addition to `s/-s` / `ms/-ms` and will
ensure, that a new session will be opened, even if the arguments
passed would otherwise reopen an already existing session. That way
one can have multiple, independent, sessions open to the same set of
arguments.

Example:
``` bash
tm s host1 host2
tm -n s host1 host2
```

This will open two sessions to host1 and host2, in two different tmux
sessions, with the second session having a random string added to the
session name. A `tm ls` will look similar to the following:
```
s_29814_host1_host2: 1 windows (created ...)
s_host1_host2: 1 windows (created ...)
```

#### -g
This is only valid as an addition to `s/-s` / `ms/-ms` and will
attach to an existing session - but with a seperate window config -
tmux calls this a session group. Citation from tmux manpage about it:

```
[...] a session group. Sessions in the same group share the same
set of windows - new windows are linked to all sessions in the group
and any windows closed removed from all sessions. The current and
previous window and any session options remain independent and any
session in a group may be killed without affecting the others. The
group-name argument may be:
```

So in effect one has the same session with the same windows and panes
open, but one can display and use different windows at the same time.
