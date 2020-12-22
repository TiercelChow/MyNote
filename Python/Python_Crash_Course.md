# Chapter01：起步

# Chapter02：变量和简单数据类型


## 2.1 运行hello_world.py发生的情况

```python
print("Hello Python World!")
```

编译器使用Python解释器来运行它

## 2.2 变量

```python
message = "Hello Python World!"
print(message)
```

### 2.2.1 变量的命名和使用

- 变量名只能包含数字，字母和下划线，且不能以数字开头；
- 变量名不能包含空格，但可使用下划线来分隔其中的单词；
- 不要将Python关键字和函数名用作变量名；
- 变量名应简短又具有描述性；
- 慎用字母l和O，易被错看成1和0。

### 2.2.2 使用变量名时避免命名错误

拼写，大小写都应保证完全一致

## 2.3 字符串

字符串就是一系列字符。在Python中，用引号（双引号、单引号）括起的都是字符串

```
"This is a string."
'This is also a string.'
```

### 2.3.1 使用方法修改字符串的大小写

```python
# 单词首字母大写：title()
name = "ada lovelace"
print(name.title())

# 输出
Ada Lovelace

# 全置为大写：upper()
# 全置为小写：lower()
name = "Ada Lovelace"
print(name.upper())
print(name.lower())

# 输出
ADA LOVELACE
ada lovelace
```

### 2.3.2 合并（拼接）字符串

```python
# “ + ”合并字符串
first_name = "ada"
last_name = "lovelace"
full_name = first_name + " " + last_name

print(full_name)
print("Hello," + fullname.title() + "!")

# 输出
ada lovelace
Hello, Ada Lovelace!
```

### 2.3.3 使用制表符或换行符来添加空白

```python
# 换行符：\n
# 制表符：\t
print("Languages:\n\tPython\n\tC++")

# 输出
Language:
    Python
    C++
```

### 2.3.4 删除空白

```python
# 删除字符串开头的空白：lstrip()
# 删除字符串末尾的空白：rstrip()
# 删除字符串头尾的空白：strip()
>>>favorite_language = ' python '
>>>favorite_language
' python '
>>>favorite_lanuage.rstrip()
' python'
>>>favorite_lanuage.lstrip()
'python '
>>>favorite_language.strip()
'python'
>>>favorite_language
' python '
```

### 2.3.5 使用字符串时避免语法错误

若字符串中含有单引号或双引号，可在符号前面加上转义字符“ \ ”

### 2.3.6 Python 2中的print语句

Python 2中无需将要打印的内容放在括号内。从技术上说，Python 3中的print是一个函数，因此括号必不可少，有些Python 2 print语句也含括号，但其行为与Python 3中稍有不同。简单地说，在Python 2代码中，有些print语句包含括号，有些不包含。

## 2.4 数字

### 2.4.1 整数

加（ + ）减（ - ）乘（ * ）除（ / ）

### 2.4.2 浮点数

从很大程度上说，使用浮点数都无需考虑其行为，只需输入要使用的数字，Python通常会按期望的方式处理它们，但需要注意的是，结果包含的小数位数可能是不确定的

### 2.4.3 使用函数str()避免类型错误

在字符串拼接中，若需要对将数字拼接进字符串，需要通过str()对其先进行类型转换，否则出现类型错误

```python
age = 23
message = "Happy" + str(age) + "rd Birthday!"
print(message)
```

### 2.4.4 Python 2中的整数

Python 2中，两个整数相除得到的结果依然为整数，小数部分被直接删除，若需要避免这样的情况，需要保证操作数中至少有一个数为浮点数。

## 2.5 注释

```python
# 单行注释
'''
多行注释
'''
```

### 2.6 Python之禅

```python
>>> import this
```

# Chapter03：列表简介

## 3.1 列表是什么

列表是由一系列特定顺序排列的元素组成。

### 3.1.1 访问列表元素

通过索引访问，

### 3.1.2 索引从0而不是1开始

访问最后一个元素时索引指定为-1，依此类推（当列表为空时这种访问方式会出错）

### 3.1.3 使用列表中的各个值

## 3.2 修改、添加和删除元素

#### 3.2.1 修改列表元素

通过索引找到元素，对其进行赋值即可

### 3.2.2 在列表中添加元素

> 在列表末尾添加元素

```python
motorcycles = ['honda', 'yamaha', 'suzuki']

motocycles.append('ducati')
```

> 在列表中插入元素

```python
motorcycles = ['honda', 'yamaha', 'suzuki']

motocycles.instert(0, 'ducati')
# 新元素的索引和值
```

### 3.2.3 从列表中删除元素

> 使用del语句删除元素

```python
motorcycles = ['honda', 'yamaha', 'suzuki']

del motocycles[1]
```

> 使用方法pop()删除元素

```python
motorcycles = ['honda', 'yamaha', 'suzuki']

popped_motorcycle = motorcycles.pop()
# 删除列表最后一个元素
```

> 弹出列表中任何位置处的元素

```python
motorcycles = ['honda', 'yamaha', 'suzuki']

first_owned = motorcycles.pop(0)
# 弹出列表中索引为0的元素
```

> 根据值删除元素

```python
motorcycles = ['honda', 'yamaha', 'suzuki']

motorcycles.remove('yamaha')
# 方法remove()只删除第一个指定的值，如果要删除的值可能在列表中出现多次，就需要使用循环来判断是否删除了所有的这样的值
```

## 3.3 组织列表

### 3.3.1 使用方法sort()对列表进行永久排序

```python
cars = ['bmw', 'audi', 'toyota', 'subaru']
# 按字母序进行排序
cars.sort();
print(cars)
# 按字母反序进行排序
cars.sort(reverse=True)
```

### 3.3.2 使用函数sorted()对列表进行临时排序

```python
cars = ['bmw', 'audi', 'toyota', 'subaru']
# 按字母序进行排序
print(sorted(cars))
# 按字母反序进行排序
print(sorted(cars, reverse=True))
```

### 3.3.3 倒着打印列表

```python
cars = ['bmw', 'audi', 'toyota', 'subaru']

cars.reverse()
```

### 3.3.4 确定列表的长度

```python
cars = ['bmw', 'audi', 'toyota', 'subaru']

len(cars)
```

## 3.4 使用列表时避免索引错误

# Chapter04：操作列表

## 4.1 遍历整个列表

```python
magicians = ['alice', 'david', 'carolina']

for magician in magicians:
	print(magician)
```

### 4.1.1 深入地研究循环

建议的命名规则：

```python
for cat in cats:
for item in list_of_items:
```

### 4.1.2 在for循环中执行更多的操作

注意缩进

### 4.1.3 在for循环结束后执行一些操作

取消缩进即可

## 4.2 避免缩进错误

### 4.2.1 忘记缩进

### 4.2.2 忘记缩进额外的代码行

### 4.2.3 不必要的缩进

### 4.2.4 循环后不必要的缩进

### 4.2.5 遗漏了冒号

## 4.3 创建数值列表

### 4.3.1 使用函数range()

函数range()让Python从指定的第一个值开始数，并在到达指定的第二个值后停止

```python
for value in range(1,5):
  print(value)
# 打印数字 1～5
```

### 4.3.2 使用range()创建数字列表

要创建数字列表，可使用函数list()将range()的结果直接转换为列表

```python
numbers = list(range(1,6))
```

函数range()还可指定步长

```python
# 1~10内的偶数
even_numbers = list(range(2,11,2))
```

