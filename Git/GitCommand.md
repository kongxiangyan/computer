### 移除已提交的文件的版本控制

```bash
# 实质就是删除缓冲区里的文件，再提交给服务器端
git rm -r -n --cached ${some-directory} # -n：加上这个参数，执行命令时，是不会删除任何文件，而是展示此命令要删除的文件列表预览。
git rm -r --cached ${some-directory} # 最终执行命令
git commit -m 'Remove the now ignored directory "${some-directory}"' # 提交
git push origin master # 提交到远程服务器

此时 git status 看到 ${some-directory} 目录状态变为 untracked 可以修改 .gitignore  文件，添加 ${some-directory} 并提交 .gitignore 文件到远程服务器，这样就可以不对 ${some-directory} 目录进行版本管理了。以后需要的时候,只需要注释 .gitignore 里 ${some-directory} 内容,重新执行 git add ${some-directory}，即可重新纳入版本管理。

# git rm命令参数
-n --dry-run 
# Don’t actually remove any file(s). Instead, just show if they exist in the index and would otherwise be removed by the command.
-r 
# Allow recursive removal when a leading directory name is given. 
--cached 
# Use this option to unstage and remove paths only from the index. Working tree files, whether modified or not, will be left alone.
```

> :link: 进阶：https://www.dovov.com/git-gitignore.html

### git log 的简化及美化

```bash
# 简化及美化
git log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit

# 指定别名
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```

