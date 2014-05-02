This repository contains some files which have the same name on a
case-insensitive filesystem (the default in Windows and OS X), which makes it
impossible to check out cleanly.

`SAME` and `same` have identical content. `DIFFERENT` and `different` do not.

Git's response can be a little confusing. Cloning this repo on OS X checks out
the following:

    $ ls
    README.md   different   same

Git thinks that `DIFFERENT` has changed, but doesn't notice that `SAME` doesn't
exist anymore:

    $ git diff --stat
    DIFFERENT | 2 +-
    1 file changed, 1 insertion(+), 1 deletion(-)

And each time you try a `git reset`, Git toggles between deciding whether `DIFFERENT`
or `different` has problems:

    $ git reset --hard -q && git diff --stat
    different | 2 +-
    1 file changed, 1 insertion(+), 1 deletion(-)

    $ git reset --hard -q && git diff --stat
    DIFFERENT | 2 +-
    1 file changed, 1 insertion(+), 1 deletion(-)

This might seem like a weird edge case, but it has caught me out before.  Don't
clone your Linux repositories onto a Mac and expect them to work when the
folder is synced to a Linux Vagrant. Or, if you do, make sure your Linux
repositories don't do insane things like contain both `HTML.pm` and `html.pm`,
or you will get different errors depending on how many Git commands you run
between tests.
