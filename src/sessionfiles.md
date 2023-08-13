# Session files
Anyone who carefulle read the section about the
[commandline](commandline.md) and compared it with the actual output
of `tm -h`/`tm --help` may have noticed that one thing hasn't been
mentioned there - `-c <CONFIG>`.

That simple option turns out to be big enough to warrant an own
chapter - actually seperated into two - and so here goes.

**Session files** exist in (currently) two different versions,
currently named *Simple* and *Extended*, and their naming already
sets expectations.

## File locations
Session files are expected at `${HOME}/.tmux.d` and are plain text
files. Files with no extension are handled as simple, files with a
.cfg as extension as extended session files.
