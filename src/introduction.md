# Introduction

tm is a small helper/wrapper for tmux. It aims at making the creation
and resume of sessions easier wile trying to stay out of the users
way.

# History
tm started as a bash shell script to ease my day-to-day work with
[tmux](https://github.com/tmux/tmux). While tmux itself is easy to
work with (creating new sessions is simple) for simple tasks, it takes
considerably more brain power to setup a session that opens
connections to multiple hosts and sets tmux up in the way, that input
is send to all of them at the same time.

And this being a task I need a *lot* of times every day, I do not want
to do this manually, so tm was born. Back in 2010/2011 when I started
working on it, there wasn't any (to me) useful tool I could have
taken instead.

## Rust
At some point in time I switched over from Shell to Rust, which
brought a **huge** speed boost for tm. While the shell version always
worked good enough and for most sessions one builds also fast enough,
having a compiled binary *still* makes a difference. It is especially
noticable on the sessions that open dozens (or even hundreds) of
windows/panes.

# Commandline

tm offers simple ways to start new sessions using commandline, where
it supports two styles of options - so-called `traditional` and
`getopt` style. They are mostly identica, though some options only
work in one and not the other way.

# Config files

tm also offers different types of config files (called session files).
From simple list of hostnames to files that ca include others to files
that basically contain tmux commands to execute.
