# GIT 骚操作

## git 全局配置
```
[user]
	email = wszxlleo@gmail.com
	name = zxlleo
[alias]
	lg = log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit
	last = log -1
	unstage = reset HEAD
	st = status
[core]
	editor = code --wait
[diff]
    tool = default-difftool
[difftool "default-difftool"]
    cmd = code --wait --diff $LOCAL $REMOTE
```


## git 对比两个分支 具体某个文件的差异
> 显示出所有有差异的文件列表

- git diff branch1 branch2 --stat

> 显示指定文件的详细差异

- git diff branch1 branch2 具体文件路径

> 显示出所有有差异的文件的详细差异
- git diff branch1 branch2


## git 不同分支部分文件的合并
1. 切换到需要合并的分支
> git checkout master-branch

2. checkout修改的文件
> git checkout source_branch <path>

3. git status可发现这些文件出现在当前分支中,commit提交