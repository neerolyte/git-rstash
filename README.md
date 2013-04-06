Git R(emote)Stash
==============================

git-rstash aims to be a substitute for git-stash that allows stashes to be pushed to a location where they can easily be synchronised with other remote machines.

[![Build Status](https://travis-ci.org/neerolyte/git-rstash.png)](https://travis-ci.org/neerolyte/git-rstash)

Basic Usage
------------------------------

Limited inbuilt help is available with `--help`:
```bash
$ git-rstash --help
Usage: git rstash list
   or: git rstash show
   or: git rstash save [<message>]
   or: git rstash ( pop | apply )
   or: git rstash clear
```

Saving a rstash is similar to saving a stash:
```bash
$ git rstash save "Stuff to come back to later"
HEAD is now at eaa2b8e initial version
```

rstashes can be listed similarly to stashes, they just have more complicated IDs to reduce the chance of collision when working on distinct machines:
```bash
$ git rstash list
rstash@{2013-04-05T18:33:16,604969889+1100}: WIP on master: Stuff to come back to later
```

rstashes can be applied or popped similarly to stashes, either provide a string that's in the rstash ID or the most recent one will be applied:
```bash
$ git rstash apply 'Stuff to come back to later' # match in the message
$ git rstash apply '2013-04-05T18:33:16' # match in the ID
$ git rstash apply # apply the last one
$ git rstash pop # works the same as apply, but removes the applied stash from the list
```

Known Issues
-------------------------------

It's really early days so the format is still up for change at any point.
