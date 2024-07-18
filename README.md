# So You Think You Know Git

A list of interesting git commands from Scott Chacon from his [FOSDEM 2024 talk on Git](https://www.youtube.com/watch?v=aolI_Rz0ZqY)

Scott is a co-founder of GitHub and founded [git-scm](https://git-scm.com/).

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

`.gitconfig` adds flexibility when working on different projects. Adding a non-global configuration file in the root project directory can smoothen up the workflow.
```gitconfig
[user]
  email = example@git.edu
```
This config can also look like this in the command-line `git config user.email example@git.edu`. The command updates the user in the local gitconfig file.

### Conditional
To add a little more complexity, IncludeIf sets up the logic for further customization.
```gitconfig
# The config can be loaded based on current path
[IncludeIf "gitdir:~/projects/work/"]
    path = ~/projects/work/.gitconfig
```

## Useful Commands

### Common Commands
|  Command  |  Description  |
| --------  | :------ |
| `git blame -L 10,16 path/to/file` |  Git blame with start and end line numbers to the file  |
| `git blame -L :example:path/to/file`|  Git blame using where 'example' is found in the file as the starting line |
| `git blame -w` | Git blame ignore whitespace |
| `git blame -w -C` | AND detect lines moved or copied in the same commit  |
| `git blame -w -C -C` | OR the commit that created the file |
| `git blame -w -C -C -C` | OR any commit at all |
| `git log -p -S --debug` | Search for git logs containing String `--debug` |
| `git log -L 10,16 path/to/file` |  Git log with start and end line numbers to the file  |
| `git reflog` | Log of all the changes to current state of repo for possible reverting |
| `git diff --word-diff` | Shows changes by words instead of lines |
| `git blame -w` | Git blame ignore whitespace |

### New Commands

#### Git Branch Column
`git branch` has a new formatting option called column. It changes the default layout of the branch list. This combined with branch sorting by last commit dates results in a current and clean layout.
```bash
git config --global column.ui auto
git config --global branch.sort -committerdate
git branch
```
OR
```
git branch --column --sort=-committerdate
```

#### Git Push Force with Lease
`git push --force-with-lease` is a new addition that adds a ref check and compares the remote version to the local version and rejects the force push when conflicts are detected.


#### Signing Commits
```
git config gpg.format ssh
git config user.signingkey ~/.ssh/
```
Git has SSH signing integrated which gives a layer of protection for repos. Repositories like GitHub can validate the signature with a copy uploaded in settings.


#### Git Prefetching
Monorepos take a lot of time update, so now git allows incremental prefetching to load changes on command.

To enable maintenance on a local repo
```
git maintenance start
```

The command updates .gitconfig to add the lines below.
```
[maintenance]
    auto = false
    strategy = incremental
```
This adds a cron job to the repository to run a task every hour.

The prefetched references will not be loaded into the project until the merge is done.
To validate that the prefetch is working
`git for-each-ref | grep prefetch`

|`strategy = incremental`| |
|-----|-----|
|prefetch | hourly|
|commit-graph | hourly|

Commit graph shows a tree of the commit logs.
`git log --graph --oneline -10 >/dev/null`

It is a very expensive feature because objects have to traverse through each commit to draw the tree.
Prefetching will cache the tree in a hash map and load it when there's a call to draw the graph.

```
git config --global fetch.writeCommitGraph true
```

## Default Global Configurations
These configurations are examples of commands that are worth running by default or useful global config.

```
# Reuse Recorded Resolution
# Git will remember how to resolve merge conflicts.
git config --global rerere.enabled true

# Automatically creates upstream branch
git config --global push.default current

# Stashes all changes
git config --global alias.staash 'stash --all'

# bash commands are run as git comands
git config --global alias.[alias] ![command]
```
