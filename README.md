# So You Think You Know Git

Highlights of git shortcuts by Scott Chacon (YouTube)/More Available in Videos: [Part 1](https://www.youtube.com/watch?v=aolI_Rz0ZqY), [Part 2](https://www.youtube.com/watch?v=Md44rcw13k4)


## Table of contents

<!--ts-->
  * [Git Config Tips](#git-config-tips)
    * [Conditional](#conditional)
  * [Useful Commands](#useful-commands)
    * [Common Commands](#common-commands)
    * [New Commands](#new-commands)
      * [Show Branches in Columns](#show-branches-in-columns)
      * [Safe Version of Force Push](#safe-version-of-force-push)
      * [Restore](#restore)
      * [Commits with SSH Key Signatures](#commits-with-ssh-key-signatures)
      * [Git Prefetching](#git-prefetching)
      * [Visualizing the Commit Tree](#visualizing-the-commit-tree)
   * [Default Global Configurations](#default-global-configurations)
<!--te-->

## Git Config Tips

`.gitconfig`


```gitconfig
[user]
  email = example@git.edu
```
or add it with a command: `git config --global user.email example@git.edu`.

### Conditional

```gitconfig
# The config can be loaded based on working path
[IncludeIf "gitdir:~/projects/work/"]
    path = ~/projects/work/.gitconfig
```

## Useful Commands

### Common Commands
|  Command  |  Description  |
| --------  | :------ |
| `git blame -L :example:path/to/file`|  Shows commit author for line number of `:example:` |
| `git blame -w` | Ignore changes to spacing in the file |
| `git log -w` | Ignore changes to spaces in the logs |
| `git log -p -S --debug` | Reduces the list to commits with message `--debug` |
| `git diff --word-diff` | Shows word count instead of lines changed |

### New Commands

#### Show Branches in Columns

```bash
# Saves the setting
git config --global column.ui auto              #1
git config --global branch.sort -committerdate  
git branch 
```
OR
```bash
# Optional way
git branch --column --sort=-committerdate       #2
```

#### Safe Version of Force Push
```bash
# This stops any changes overwriting the upstream repository
git push --force-with-lease
```

#### Restore
```bash
# Start with a clean copy by `restoring` it or the usual `checkout`
git restore file.txt
git checkout file.txt
```
OR
```bash
# Restore from x time ago
git restore --source HEAD@{2.hours.ago} file.txt
```
It's very useful to jump to when the project was working with this command.

#### Commits with SSH Key Signatures
```bash
# create a new key with ssh-keygen
git config gpg.format ssh
git config user.signingkey ~/.ssh/
```
Remember to add the public key to your upstream account.

#### Git Prefetching
```bash
git maintenance start
```
OR

```bash
# .gitconfig
[maintenance]
    auto = false
    strategy = incremental
```
A cron job will be created to prefetch per hour by default.

The prefetched references will not be loaded into the project until the changes are merged with `git fetch`. 

```bash
# To get more info on what prefetching does in the background
git for-each-ref | grep prefetch
```

##### Default configurations
|`strategy`| `incremental` |
|-----|-----|
|prefetch | hourly|
|commit-graph | hourly|

#### Visualizing the Commit Tree
```
git log --graph --oneline -10 >/dev/null
```

It is a very expensive feature because objects have to traverse through each commit to draw the tree.
Prefetching will cache the tree in a hash map and load it when there's a call to draw the graph.

```
git config --global fetch.writeCommitGraph true
```

### Useful Default Global Configurations
```bash
# Reuse Recorded Resolution
# Git will remember how to resolve similar merge conflicts
git config --global rerere.enabled true

# Repository creates a new branch by default on push
git config --global push.default current

# Add an alias to stash all changes
git config --global alias.staash 'stash --all'

# Run a command as a git alias
git config --global alias.[alias] ![command]
```

```bash
# .gitconfig
[alias]
    staash = 'stash --all'
[rerere]
    enabled = true
[push]
    default = current
```
