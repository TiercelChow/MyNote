[toc]



# 使用教程

## 1. 将本地项目推送至远程

> 新建远程仓库

在Github或者Gitee上新建远程仓库

> 进入本地文件夹

首先进入需要推送至远程的项目的文件夹

>初始化

```
$ git init
```

> 添加需要推送的文件

```
$ git add .
```

该命令即为将该目录下所有文件全部进行添加

> 提交修改

```
$ git push -m "first commit"
```

""里的内容为本次提交的备注

> 与远程仓库相关联

```
$ git remote add origin 新建仓库的地址
```

> 推送至远端

```
$ git push -u origin master
```







# 常见报错分析

