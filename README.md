# So You Think You Know Git

Highlights of git shortcuts by Scott Chacon (YouTube)/More Available in Videos: [Part 1](https://www.youtube.com/watch?v=aolI_Rz0ZqY), [Part 2](https://www.youtube.com/watch?v=Md44rcw13k4)


## Table of contents

<!--ts-->
  * [Git Config Tips](#git-config-tips)
    * [Conditional](#conditional)
  * [Useful Commands](#useful-commands)
    * [Common Commands](#common-commands)
    * [New Commands](#new-commands)
      * [Git Branch Column](#git-branch-column)
      * [Git Push Force with Lease](#git-push-force-with-lease)
      * [Signing Commits](#signing-commits)
      * [Git Prefetching](#git-prefetching)
   * [Default Global Configurations](#default-global-configurations)
<!--te-->

## Git Config Tips

`.gitconfig`


```gitconfig
[user]
  email = example@git.edu
```
The file looks simple: `git config user.email example@git.edu`. That is a command to modify or create the git config file at the starting git directory.


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
| `git blame -w` | Take away changes to spaces in the file |
| `git log -w` | Take away changes to spaces in the logs|
| `git log -p -S --debug` | Reduces the list to commits with message `--debug` |
| `git diff --word-diff` | Shows changes per word instead of lines |

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
# Call isn't completed when there's a conflict
git push --force-with-lease
```
If upstream change history is different, then the push is stopped.


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

#### Visualizing the commit tree
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
# Git will remember how to resolve merge conflicts.
git config --global rerere.enabled true

# Automatically creates upstream branches
git config --global push.default current

# Stashes all changes
git config --global alias.staash 'stash --all'

# 
git config --global alias.[alias] ![command]
```

```bash
[alias]
    staash = 'stash --all'
[rerere]
    enabled = true
[push]
    default = current
    
```
