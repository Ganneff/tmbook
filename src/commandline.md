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

## Traditional
The traditional calling style, some of it dating back to the first
shell script version, is easiest for some options.

### Overview
As of tm version 0.9 the following commands are supported using the
`traditional` style of calling tm:

| Command | Intended action                                           |
|---------|-----------------------------------------------------------|
| ls      | List running sessions                                     |
| s       | Open SSH session to the destination                       |
| ms      | Open multi SSH sessions to hosts, synchronizing input     |
| k       | Kill a session                                            |
| b       | Break a multi-session pane into single windows            |
| j       | Join multiple windows into one single one with many panes |
| help    | Print this message or the help of the given subcommand(s) |

#### ls
Same as `tmux ls`, simply list the existing tmux sessions.

#### s [HOSTS]
This will open a new tmux session and create multiple windows, one per
argument - the windows will be using ssh to connect to the given
host(s). It's arguments can be anything that your SSH config will
accept, this can range from simple hostnames to user@host or whatever
possible matches you may have configured in your *~/.ssh/config* file.

Multiple arguments will open multiple tmux windows in the same
session.

#### ms [HOSTS]
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

#### k [SESSION]
This basically translates to `tmux kill-session` and will kill the
session with the given name.

#### b [SESSION]
This is used to **break** a session built using
[ms](commandline.md#ms) or a session file (or even manually) into
seperate windows. It will take the given session name and split all
panes in the window included into seperate tmux windows.

#### j [SESSION]
This is used to **join** a multi-window session to one that closely
resembles one build using the [ms](commandline.md#ms) command. It will
take the given session name and join all windows to the first window,
as such making them panes of the first window. Afterwards it will
toggle the **synchronize-pane** option of that window on, so any input
will be send to all panes.
