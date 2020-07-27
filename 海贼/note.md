[toc]

## WEEK Ⅰ

### 1.1 输入输出函数说明

> printf 函数

```C
//头文件：
	stdio.h
//原型：
    int printf(const char *fromat, ...);
/*--
--format；格式控制字符串
--... ； 可变参数列表
--返回值：输出字符的数量
--*/
```

>scanf 函数

```c
//头文件：
	stdio.h
//原型：
    int scanf(const char *fromat, ...);
/*--
--format；格式控制字符串
--... ； 可变参数列表
--返回值：成功读入的参数个数
--*/
//循环读入的实现(EOF=-1)：
while (scanf("%d",t) != EOF){
    ...
}
```

>字符串匹配集

```c
#include<stdio.h>

int main() {
    char str[1000]={0};
    scanf("%[^\n]s",str);
    printf("%s\n",str);
    return 0;
}
/*--
--[^\n]：表示读入的只要是非换行符都读入算作一行
--*/
```

