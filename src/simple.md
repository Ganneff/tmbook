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

### Example
Line numbers (if any) added for clarity in the description later,
remove them if you copy this example!

#### Cluster of machines
```bash
somecluster
NONE
host1
user@host2
```
This will open a session named "somecluster", connecting to host1
using the default user (usually your username, or whatever your ssh
config says, if any) and user@host2. It will open one window with two
panes and synchronize input to them, so that any action happens on
both hosts.

#### Machine list from external command
```bash
nfsserver_mail
NONE
LIST ssh -tt clustermaster "sudo /usr/sbin/gnt-instance list --no-headers -o name --filter '(\"nfs\" in tags and \"prod\" in tags and \"mail\" in tags) and admin_state == \"up\"'"
```
This needs sudo for the gnt-instance command on the cluster master, recommended NOPASSWD, and will then fetch a list of (running) hosts where the ganeti tags match nfs, prod and mail. Any other host, or hosts with those tags but marked down, will be ignored.

#### Extended, machine list from external command using ++TMREPLACETM++
```bash
nfsserver_++TMREPLACETM++
NONE
LIST ssh -tt clustermaster "sudo /usr/sbin/gnt-instance list --no-headers -o name --filter '(\"nfs\" in tags and \"prod\" in tags and \"++TMREPLACETM++\" in tags) and admin_state == \"up\"'"
```
This is nearly the same as the example just before, except that the mail function is replaced by a token. This token can be given as one more argument after the session file and allows flexibility in the LIST command.
To open the same session as in the example before, that is, all mail VMs, one can run `tm vm_nfs mail` - assuming the session file is saved as **vm_nfs** in the *.tmux.d*.
