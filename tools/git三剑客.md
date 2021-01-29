[toc]



### 版本管理的演变

> VCS出现前

- 用目录拷贝区别不同的版本
- 公共文件容易被覆盖
- 成员沟通成本很高，代码集成效率低下

> 集中式VCS

- 有集中的版本管理服务器
- 具备文件版本管理和分支管理能力
- 集成效率有明显的提高
- 客户端必须时刻和服务器相连

> 分布式VCS

- 服务端和客户端都有完整的版本库
- 脱离服务器，客户端照样可以管理版本
- 查看历史和版本比较等多数操作都不需要访问服务器，比集中式VCS更能提高版本管理效率

### Git的特征

- 最优的存储能力
- 非凡的性能
- 开源的
- 很容易做备份
- 支持离线操作
- 很容易定制工作流程

### Git的安装

官网下载安装包安装即可

### 使用Git之前需要做的最小配置

> 配置user信息

配置user.name和user.email

```
$ git config --global user.name 'your_name'
$ git config --global user.email 'your_email_address'
```



> config的三个作用域

缺省等同于local

```
# local只对某个仓库有效
$ git config --local
# global对当前用户所在仓库有效 
$ git config --global
# system对系统所有登录的用户有效
$ git config --system
```

显示config的配置，加--list

```
$ git config --list local
$ git config --list global
$ git config --list system
```

