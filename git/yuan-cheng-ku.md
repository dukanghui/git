多人共同协作开发的时候，就需要远程仓库了，之前也讲过github就是一个远程仓库

例如：github用户名为steven-L，有仓库demo

## git clone

语法：

> git  clone &lt;url&gt;：默认生成和库同名的文件夹
>
> git clone &lt;url&gt; &lt;local&gt;：指定生成目录名

git clone 支持多种协议

```
git clone http[s]://github.com/steven-L/demo.git
git clone ssh://github.com/steven-L/demo.git
git clone git://github.com/steven-L/demo.git
git clone /opt/git/project.git
git clone file:///opt/git/project.git
git clone ftp[s]://github.com/steven-L/demo.git
git clone rsync://github.com/steven-L/demo.git
```

例：clone远程库

```
git clone https://github.com/steven-L/demo.git
echo 'init'>README.md
git add --all
git commit -m 'init'
git push
```

# git remote

git要求每个远程主机都必须指定一个主机名。git remote命令用于管理主机名

> git remote  ：列出所有远程主机
>
> git remote -v：查看远程主机的网址
>
> git remote show &lt;主机名&gt;：查看主机的详细信息
>
> git remote add &lt;主机名&gt; &lt;网址&gt;：添加远程主机
>
> git remote rm &lt;主机名&gt;：删除远程主机
>
> git remote rename &lt;原主机名&gt; &lt;新主机名&gt;：重新命名
>
> git remote set-url &lt;主机名&gt; &lt;url&gt;：改变url地址

clone版本库时，git会自动命名远程主机为`origin`  ，如果想用其他主机名，使用-o参数

```
git clone -o myname https://github.com/steven-L/test1.git
```

### **例：**

clone版本

```
git clone https://github.com/steven-L/test1.git
```

查看

```
git remote
git remote -v
git remote show origin
```

改变地址

```
git remote set-url origin https://github.com/steven-L/test1.git

git remote set-url origin git@github.com:steven-L/test1.git
```

更改主机名

```
git remote rename origin testorigin
git remote
git remote rename testorigin origin
```

# git fetch

> git fetch &lt;远程主机名&gt;：取回远程主机的全部更新
>
> git fetch &lt;远程主机名&gt; &lt;分支名&gt;：取回远程主机对应的分支的更新
>
> git branch -r：查看远程分支
>
> git branch -a：查看所有分支

一旦远程主机的版本库有了更新（Git术语叫做commit），需要将这些更新取回本地，这时就要用到`git fetch`命令。

`git fetch`命令通常用来查看其他人的进程，因为它取回的代码对你本地的开发代码没有影响。

所取回的更新，在本地主机上要用“远程主机名/分支名”的形式读取

```
git checkout origin/master
```

## 例

我们继续使用test1这个远程库

```
git branch -a
```

发现没有任何版本

```
echo '#hello'>README.md
git add --all
git commit -m 'init'
```

运行上面命令后

```
git branch -a
```

发现生成了本地版本master，仍然没有远程版本

```
git push
git branch -a
```

这下远程版本库也创建了

# git push

语法：

> #### **git push &lt;远程主机名&gt; &lt;本地分支名&gt;:&lt;远程分支名&gt;**

`git push`命令用于将本地分支的更新，推送到远程主机

```
git checkout -b dev
echo 'hello'>index.html
git add --all
git commit -m 'dev add index.html'
git push origin dev:dev
git branch -a
```

* ##### 如果省略远程分支名，则表示将本地分支推送与之存在"追踪关系"的远程分支（通常两者同名），如果该远程分支不存在，则会被新建。

```
git push origin master
```

上面命令表示，将本地的`master`分支推送到`origin`主机的`master`分支。如果后者不存在，则会被新建。

* ##### 如果省略本地分支名，则表示删除指定的远程分支，因为这等同于推送一个空的本地分支到远程分支。

```
$ git push origin :dev
# 等同于
$ git push origin --delete dev
```

上面命令表示删除`origin`主机的dev分支。

* ##### 如果当前分支与远程分支之间存在追踪关系，则本地分支和远程分支都可以省略

```
  git push origin
```

上面命令表示，将当前分支推送到`origin`主机的对应分支

* ##### 如果当前分支只有一个追踪分支，那么主机名都可以省略 git push,如果当前分支与多个主机存在追踪关系，则可以使用`-u`选项指定一个默认主机，这样后面就可以不加任何参数使用`git push`

```
git push -u origin master
```

上面命令将本地的`master`分支推送到`origin`主机，同时指定`origin`为默认主机，后面就可以不加任何参数使用`git push`了

* ##### 不带任何参数的`git push`，默认只推送当前分支，这叫做simple方式。此外，还有一种matching方式，会推送所有有对应的远程分支的本地分支。Git 2.0版本之前，默认采用matching方法，现在改为默认采用simple方式。如果要修改这个设置，可以采用`git config`命令。

```
$ git config --global push.default matching
# 或者
$ git config --global push.default simple
```

* ##### 还有一种情况，就是不管是否存在对应的远程分支，将本地的所有分支都推送到远程主机，这时需要使用`--all`选项。

```
 git push --all origin
```

* ##### 如果远程主机的版本比本地版本更新，推送时Git会报错，要求先在本地做`git pull`合并差异，然后再推送到远程主机。这时，如果你一定要推送，可以使用`--force`选项。

```
$ git push --force origin
```

上面命令使用`--force`选项，结果导致远程主机上更新的版本被覆盖。除非你很确定要这样做，否则应该尽量避免使用`--force`选项

* ##### `git push`不会推送标签（tag），除非使用`--tags`选项

```
git push origin --tags
```

# git pull

> git pull &lt;远程主机名&gt;  &lt;远程分支名&gt;:&lt;本地分支名&gt; ：远程分支于本地分支合并

`git pull`命令的作用是，取回远程主机某个分支的更新，再与本地的指定分支合并。

```
git pull origin master:master
```

* ##### 如果远程分支于当前分组合并，本地分支名可省略

```
git pull origin master
```

* ##### 建立追踪关系

```
git branch --set-upstream master origin/next
```

* ##### 如果当前分支与远程分支存在追踪关系，`git pull`就可以省略远程分支名

```
git pull origin
```

* ##### 如果当前分支只有一个追踪分支，连远程主机名都可以省略

```
git pull
```

* ##### 如果远程主机删除了某个分支，默认情况下，`git pull`不会在拉取远程分支的时候，删除对应的本地分支，可以改变这个行为，加上参数`-p`就会在本地删除远程已经删除的分支

```
$ git pull -p
# 等同于下面的命令
$ git fetch --prune origin 
$ git fetch -p
```

# 



