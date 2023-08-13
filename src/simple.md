# Simple
Simple session files are nothing more than files without an extension
to their name, where each line is treated according to the following spec:

* Line 1: Session name
* Line 2: Ignored, but required for backward compatibility
* Line 3 ...: Argument line, one of two possibilities:
   * Argument for SSH
   * The word LIST followed by a shell command.

## LIST command
If an argument line starts with *LIST*, everything following it is
used as a shell command. The commands stdout is read and every line
read is treated as another argument line. This is recursive, so a line
read by LIST can include another LIST (until stack overflows, so be
careful to not create an endless loop).

Full Shell expansion, that is both tilde and environment variables, is
supported. Details can be taken from the
[shellexpand](https://docs.rs/shellexpand/latest/shellexpand/fn.full.html)
rust crates documentation, but in short: ~ expands to the users
homedir, $VAR and ${VAR} expand to the content of the variable VAR,
and non-existing variables expand to an empty string.
