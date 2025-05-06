+++
title = 'Git Usage Notes'
date = 2025-01-02T18:56:07+08:00
draft = false
tags = ["programming"]
+++

# Some Fancy Commands

Use a more fancy command `git lg` to substitute the `git log` by:
```bash
git config --global alias.lg "log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```

# How to Delete/Squash Commits

For example I have 3 commits.
```git
* 0547d60 - (HEAD -> master) Add post "How to Debug C/C++ in VSCode." (1 second ago) <Yilin Li>
* bcc2c81 - Add workflow (19 seconds ago) <Yilin Li>
* c437b88 - init (5 months ago) <Yilin Li>
```

1. Use `git rebase -i HEAD~3` to enter an interactive mode.
2. You can see the contents like this:
```git
pick bcc2c81 Add workflow
pick 0547d60 Add post "How to Debug C/C++ in VSCode"
```
3. Modify the content:
```git
pick bcc2c81 Add workflow
squash 0547d60 Add post "How to Debug C/C++ in VSCode"
```
4. Apply the changes by saving the modifications. You get a clean git log like this:
```git
* bcc2c81 - (HEAD -> master) Add workflow (19 seconds ago) <Yilin Li>
* c437b88 - init (5 months ago) <Yilin Li>
```
5. Use `git push --force` to override remote content.
