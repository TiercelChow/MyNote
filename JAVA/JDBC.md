[toc]



## 1. 数据库驱动

应用程序通过数据库驱动和数据库进行数据交换

## 2. JDBC

SUN公司为了简化开发人员的（对数据库的统一）操作，提供了一个（Java操作数据库）的规范，俗称JDBC，这些规范的实现由具体的厂商去做。

java.sql

javax.sql

还需要导入一个数据库驱动包

驱动下载地址（清华源）：[点击跳转](https://mirrors.tuna.tsinghua.edu.cn/mysql/downloads/Connector-J/)

## 3. 第一个JDBC程序

1. 创建测试数据库

   ![image-20200707202945179](C:\Users\周宇琛\AppData\Roaming\Typora\typora-user-images\image-20200707202945179.png)

2. 导入数据库驱动

新建项目后，在项目目录下新建一个lib文件夹，然后将数据库驱动jar包复制进该目录，完成后文件夹结构如图所示

![image-20200707200758219](C:\Users\周宇琛\AppData\Roaming\Typora\typora-user-images\image-20200707200758219.png)

然后右键lib文件夹，点击Add as Library，这时才算将驱动导入项目中

3. 编写测试代码

![image-20200707204743709](C:\Users\周宇琛\AppData\Roaming\Typora\typora-user-images\image-20200707204743709.png)

> 步骤总结：
>
> 1. 加载驱动
> 2. 连接数据库DriverManager
> 3. 获得执行sql的对象Statement
> 4. 获得返回的结果集
> 5. 释放连接

4. 测试结果

![image-20200707204632363](C:\Users\周宇琛\AppData\Roaming\Typora\typora-user-images\image-20200707204632363.png)

