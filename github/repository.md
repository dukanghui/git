## 2 new repository
repository就是版本库呢，又名仓库，就是用来存放项目代码的。

 ### 1创建
 登录github后在页面右上角选择：

![](/assets/1.png)

出现如下界面：

![](/assets/repository0.png)


填写项目名称，可勾选下面的红色区域选项，是否自动创建readme.md文件
点击绿色的创建仓库按钮，创建成功后如下图显示

![](/assets/repository1.png)

默认是使用https方式提交代码，也可以选择ssh方式

### 2 本地库
远程的仓库创建好了，但是要写代码还的在本地操作才方便，我们需要在本地也建立一个仓库，关联github远程的

选择一个文件夹(gitwork),运行如下代码


```
git clone https://github.com/steven-L/test.git
cd test
echo "hello git">index.html
git add --all
git commit -m 'init'
git push

```
![](/assets/rep3.png)

上图红色部分一定要注意，我们clone仓库后需要进入仓库内部才能操作仓库


![](/assets/rep4.png)

把已存在的代码上传到github


```
git remote add origin https://github.com/steven-L/test.git
git push -u origin master
```

如果出现错误
![](/assets/error1.png)
主要原因是github中的README.md文件不在本地代码目录中
　　可以通过如下命令进行代码合并
　　`git pull --rebase origin master`
