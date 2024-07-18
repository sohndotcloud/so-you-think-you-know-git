# So You Think You Know Git

A list of interesting git commands from Scott Chacon from his [FOSDEM 2024 talk on Git](https://www.youtube.com/watch?v=aolI_Rz0ZqY)

Scott is a co-founder of GitHub and founded git-scm of [git-scm](https://git-scm.com/) site.


## Git Config Tips

`.gitconfig` adds flexibility when working on different projects. Adding a non-global configuration can smooth-up the workflow.
```
[user]
  email = example@git.edu
```
This config can also look like this in the command-line `git config user.email example@git.edu`. The command updates the user in the local gitconfig file.

### Conditional
To add a little more complexity, IncludeIf sets up the logic for further customization.
```
# The config can be loaded based on current path
[IncludeIf "gitdir:~/projects/work/"]
    path = ~/projects/work/.gitconfig
```

## Useful Commands

|  Command  |  Description  |
| --------  | :------ |
|  `git blame -L 10,16 path/to/file`  |  Git blame with start and end line numbers to the file  |
|  `git log -L 10,16 path/to/file`  |  Git log with start and end line numbers to the file  |
|  `git blame -L :example:path/to/file`|  Git blame using where 'example' is found in the file as the starting line |
| `git blame -w` | Git blame ignore whitespace |
| `git blame -w -C` | AND detect lines moved or copied in the same commit  |
| `git blame -w -C -C` | OR the commit that created the file |
| `git blame -w -C -C -C` | OR any commit at all |
| `git blame -w` | Git blame ignore whitespace |
## Global Configurations

```
# Stashes all changes
# git config --global alias.staash 'stash --all'

# bash commands are run as git comands
# git config --global alias.[alias] ![command]

# Automatically creates upstream branch
# git config --global push.default current
```
