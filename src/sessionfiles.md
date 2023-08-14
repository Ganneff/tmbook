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

## Replacement tokens / tilde and variable expansion
Session files support the expansion of (Shell) variables and one
replacement token. They also support tilde extension.

### Tilde extension
The LIST command used in session files supports tilde expansion, so ~/
as well as ~username/ is supported syntax.

### Variable expansion
Shell variables are expanded in LIST commands and extended session
files. Both, $HOME as well as ${HOME} (with/without {}) are supported
syntax. Expanding an unset variable will return an empty string.

### Replacement token
In addition to shell variables, for historical reasons, tm supports
one replacement token for session files. Within session files it is
written as **++TMREPLACETM++** and will get replaced with the argument
following the session filename on commandline. That is, in `tm vm_nfs
mail` the word **mail** will be the replacement. Unlike Variable
expansion, this also works for the session name.