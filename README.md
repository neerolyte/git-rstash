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

git-rstash by default will create a store for each repo, these can be listed with `list-stores`:
```bash
$ git rstash list-stores
f5a10c44f3ebdac77870f74e9dfb3a88213eb99b3de0da82f47cda135178bf7f: /tmp/foo
```

It can be useful to have multiple copies of a repository all using the same stash store, git-rstash can be configured to do this by pointing a local repository at a different repos store by configuring `rstash.store`:
```bash
$ cd /tmp/foo
$ git rstash save "Saved stash from foo"
HEAD is now at f3b6b95 initial commit
$ cd /tmp
$ git clone foo bar
$ cd bar
$ git config rstash.store f5a10c44f3ebdac77870f74e9dfb3a88213eb99b3de0da82f47cda135178bf7f
$ git rstash list
rstash@{2013-04-06T15:00:20,147034796+1100}: WIP on master: Saved stash from foo
```

Syncing
-------------------------------

Whether or not you need or want to manually sync is basically up to you.

If you're using a tool like Dropbox that automatically syncs changes, you can just ask rstash to use the Dropbox folder for storage:
```bash
$ git config --global rstash.dir "$HOME/Dropbox/git-rstash"
```

If you would like to use a custom syncing script that can easily be done too, e.g. to configure Unison:
```bash
$ git config --global rstash.sync.cmd "unison -batch -ui text $HOME/.git-rstash ssh://<your server>/.git-rstash"
```

and then to trigger a sync with Unison:
```bash
$ git rstash sync
Contacting server...
Connected [//foo//home/user/.git-rstash -> //bar//home/user/.git-rstash]
Looking for changes
  Waiting for changes from server
Reconciling changes
Nothing to do: replicas have not changed since last sync.
```

Known Issues
-------------------------------

It's really early days so the format is still up for change at any point.
