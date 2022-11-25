# Python语法

# 输入输出

## 输入

input函数接受输入信息，参数为要输出的提示信息，返回值默认为输入的字符串。可以配合split、map函数进行分割、类型转换。

```python
input([提示信息])
#默认接收字符串：
a = input()
#将输入值转为整形：
a = int(input())
#一次性接收多个值：
a,b,c = (input("请输入三角形三边的长：").split())
a= int(a)
b= int(b)
c= int(c)
#接收多个值的同时进行类型转换：
a,b,c = map(int,input().split())
```

## 输出

# 数据类型

