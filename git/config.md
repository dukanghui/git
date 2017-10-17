# config

设置或获取仓库和git全局的配置

* 查看帮助

```
git config --help
```

* git 的配置分为

> 系统（sytem）：整个git系统，不分登陆用户
>
> 全局（global）：当前登录用户
>
> 本地（local）：当前项目

作用域越小的优先级越高 local&gt;global&gt;system

* 查看配置列表

```
git config --list
git config --global --list
git config --local --list
git config --system --list
```

* 设置用户名和邮箱

```
git config --global user.name '[useranme]'
git config --global user.email '[email]'
```



