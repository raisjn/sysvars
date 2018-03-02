## system env variables


i want my environment variables to be updated across all my shells.
this is a set of scripts that helps automate that in bash.

the idea is: "sysvar FOO=blah" will set FOO to blah. other bash terminals will
then be sent the SIGCONT command and told to reload their env variables and run
any hooks in sysvars/hooks


## installation

add the following line to your ~/.bashrc or equivalent

    source ~/path/to/sysvars/bin/init

## usage

    sysvar BACKGROUND
    sysvar BACKGROUND=light

    # in another shell
    echo $BACKGROUND
    sysvar BACKGROUND=dark

    # in another shell
    echo $BACKGROUND


### hooks

hooks are files in hooks/ that run on sysvars init and whenever the shell is
handling a sysvar reload. they can be used to automatically adjust other
properties of the terminal

### variables

variables are stored in vars/, as files. when sysvars starts up, it
automatically exports these to your shell, as FILE=contents. they will stick
around until they are deleted.


## how it works

sysvars add a trap signal handler to all new bash shells. when the handler is
invoked, it reloads the variables defined in the sysvars director.

at the moment, the signal is SIGCONT, which is one of the few signals that
are ignored by default.


## caveats and gotchas

### vim

TBD how to tell vim to reload its config when sysvars update

### sticky settings

sysvar settings will stay set indefinitely since they are contained in a file.
whenever sysvar reloads, it will re-export all the variables in var/. If you
find a variable not going away in your shell, make sure to delete its sysvar
