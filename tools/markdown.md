# 目录

[toc]

## 1. 斜体和粗体

```
*斜体*
_斜体_
**粗体**
***加粗体***
~~删除线~~
```

[返回目录](#目录)

## 2. 分级标题

```
#  一级标题
## 二级标题
...
###### 六级标题
在最开头[toc]标记可以生成目录
```

[返回目录](#目录)

## 3. 超链接

markdown支持形式的链接语法：行内式和参考式行内式一般使用较多。

### 3.1 行内式

语法说明：
[]内些链接文字，()中“”中可以为链接指定title属性，可不加，其效果为鼠标悬停在链接上会出现指定的title文字

```
[链接文字](链接地址 "链接标题")     链接地址与链接标题前有个空格
```

### 3.2 参考式

参考式链接一般用于学术论文上或某一链接在文章中多次使用。

```
文中的写法：
[链接文字][链接标记]
文本任意位置添加：
[连接标记]:链接地址 "链接标题"
链接地址与链接标题前有一个空格
若链接文字本身可以作为链接标记，也可以写为[链接文字][]
```

### 3.3 自动链接

```
只要用<>将网址或邮箱地址包起来，Markdown就会自动把它转换成链接
```

[返回目录](#目录)

## 4. 锚点

在准备跳转到的指定标题后插入锚点{#标记}，然后在文档的其他地方写上连接到锚点的链接。

```
## 0.目录{#index}
某个地方：
...任意文字[锚点标题](#index)
```

[返回目录](#目录)

## 5. 列表

### 5.1 无序列表

使用*，+，-表示无序列表

```
- 无序列表一
- 无序列表二
...
退出两次回车即可
```

### 5.2 有序列表

有序列表则使用数字接着一个英文句点

### 5.3 定义型列表

定义型列表由名词和解释组成，一行写上定义，紧跟一行写上解释，解释的写法：紧跟一个缩进（Tab）

```
	Markdown
	:	解释
```



[返回目录](#目录)

## 6. 引用



## 7. 插入图像



## 8. 表格

1. 无论哪种方式，第一行为表头没第二行分隔表头和主体部分，第三行开始每一行为一个表格行。

2. 列于列之间用管道符|隔开，原生方式的表格每一行的两边也要有管道符。
3. 第二行还可以为不同的列指定对齐方向。默认为左对齐，在-右边加上:就右对齐。

简单方式写表格：

```
学号|姓名|分数
-|-|-
00|a|100
01|b|99
02|c|80
```

原生方式写表格：

```
|学号|姓名|分数|
|-|-|-|
|00|a|100|
|01|b|99|
|02|c|80|
```

为第二列指定方向：

```
学号|姓名|分数
-|-:|-
00|a|100
01|b|99
02|c|80
```