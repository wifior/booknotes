# 精通Git

- #### git remote 

```xml
git remote -v 列出所有的远程仓库
git remote add <shortname> <url> 添加一个新的远程git仓库，shortname为url简写
git remote show origin 查看远程仓库的更多信息
git remote rename a b 重命名引用的名字，如将a重命名为b
git remote rm a 移除一个远程仓库
```

- #### git fetch

```xml
git fetch [remote-name] 访问远程仓库从中拉取所有你还没有的数据，但拉取下来后，不会自动修改或合并你的当前工作区，如果想自动合并或修改请使用git pull命令
```

- #### git tag

```xml
git tag 列出所有标签
git tag -a <label> -m 'remark' -a创建一个附注标签，-m
git tag -a <label> <hash> 后期也可以加标签
```

- #### git别名

```xml
git config --global alias.co checkout 在输入git checkout时可以直接输入git co
```

- #### git push

```xml
git push [remote-name] [branch-name]
```

- #### git diff

```xml
git diff 默认本身只显示尚未暂存的改动。
git diff --cached/--staged  查看已经暂存起来的变化。
```

- #### git commit

```xml
git commit -v 所做的改变diff信息会显示在编辑器中
git commit -m 将提交信息与命令放在同一行
git commit -a 把所有已经跟踪过的文件暂存起来一并提交
git commit --amend 将暂存区中的文件提交
```

> 当使用git commit进行提交时，git会先计算每一个子目录的校验和，然后在git仓库中把这些校验和保存为树对象，然后，git便会创建一个提交对象，它除了包含上面提到的那些信息外，还包括指向这个树对象的指针，如此一来，git就可以在需要时重现些次保存的快照





- #### git rm

```xml
git rm <文件名> 从暂存区移除
git rm -f 强制移除（删除之前修改过并已放入暂存区，就要用-f）
git rm --cached Readme 把Readme文件从远程仓库删除，本地磁盘留存，相当于之前的.gitignore文件
```

- #### git mv

```xml
git mv ==mv readme.md readme 
         git rm readme.md
         git add readme
```

- #### git log

```xml
git log 按提交列出所有更新
git log -p 显示每次提交的内容差异
git log -2 显示最近两次的提交 
git log --stat 可以看到每次提交的简略统计信息
git log --pretty=oneline 每个提交放在一行显示
git log --pretty=format:"%H "
git log --pretty=format:"%h %s" --graph 添加一些字符串来开解的展示你的分支、合并历史
git log -Sfunction_name 搜索找出添加和移除某一个特定函数的引用的提交
```

- #### git reset

```xml
git reset HEAD <file> 取消暂存
```

- #### git checkout

```xml
git checkout -- <file> 对文件做的修改会消失
```

























