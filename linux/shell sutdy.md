# shell 脚本学习

### 第一个shell脚本

```shell
#!/bin/bash
echo "hello shell"
```

### shell变量
```shell
	my_name="youngguo"
```
> 注意：变量名与等号间不能有空格

同时,变量命名遵循以下规则:
- 英文字母,数据,下划线,不能以数字开头
- 中间不能有空格,可有_
- 无标点 
- 不能使用bash关键字

有效变量如:

>RUNOOB
LD_LIBRARY_PATH
_var
var2

无效变量如:
>?var=123 
>user*name=runoob


除了显式地直接赋值，还可以用语句给变量赋值，如：
```shell
for  file  in  `ls  /etc`
```
或
```shell
for  file  in $(ls  /etc)
```


使用变量:
使用定义过的变量,在变量名前加$
```shell
username=young
echo $username
echo ${username}
```

变量外花括号可选 ，花括号为识别变量边界。如：
```shell 
for  skill  in Ada Coffe Action Java; do  
echo  "I am good at  ${skill}Script"  
done
```

已定义的变量，可以被重新定义，如：
```shelll
my_name="young"
echo $my_name
my_name="xiaosa"
echo $my_name
```
> 注意，第二次赋值的时候不能写$your_name="xiaosa"，使用变量的时候才加美元符（$）。

### 只读变量
readonly将变量定义为只读变量
此例子尝试更改只读变量，结果报错:

```shell
myUrl="https://young"
readonly myUrl
myUrl="http://young/test"
```

###  删除变量
使用 unset 命令可以删除变量。语法：
>unset variable_name

实例：
```shell
myUrl="https://young"
unset myUrl
echo $myUrl
```


### 变量类型
运行shell时，存在三种变量:
- 局部变量:	 在脚本或命令中定义，仅在当前实例生效，其他shell程序不能访问
- 环境变量：所有shell程序，包括shell启动的程序，都能访问，某些程序须要该变量来保证正常工作，必要时，shell脚本也可以定义环境变量
- shell变量: shell变量是由shell程序设置的特殊变量。shell变量中有一部分是环境变量，有一部分是局部变量，这些变量保证了shell的正常运行


### shell字符串
字符串是shell编程中最常用最有用的数据类型（除了数字和字符串，也没啥其它类型好用了），字符串可以用单引号，也可以用双引号，也可以不用引号。

单引号:
`str='young'`

单引号字符串限制：
- -   单引号里的任何字符都会原样输出，单引号字符串中的变量是无效的；
- -   单引号字串中不能出现单独一个的单引号（对单引号使用转义符后也不行），但可成对出现，作为字符串拼接使用。

双引号:
```shell
name="xiaosa"
str="Hello ,I Know you are \"$name\"!  "
echo -e $str
```

双引号优点：
- 可使用变量
- 可使用转义字符

### 拼接字符串
```shell
#双引号拼接
name="young"
str="hello, "$name" !"
str_1=" hello, ${name}  "

echo $str $str_1

#使用引号拼接
str=' hello, ' $name ' ! ' 
str_1=' hello, ${name} !  '
```
返回结果：
```shell
hello, young ! hello, young
shell-test8.sh: line 9: young: command not found
hello, young ! hello, ${name} !
```
# ???   此处与单引号字符串限制冲突
> 单引号字符串中的变量是无效的 ???

### 获取字符串长度
```shell
str="abcd"
echo ${#str}
```

> 变量为数组时，${#string}  等价于  ${#string[0]}:
```shell
str="abcd"
echo ${#str[0]}
```

### 提取字符串
从第三个字符开始截取5个字符
```shell
str="My heart will go on"
echo ${str：2:6}
```

### 查找子字符串
> 查找字符  **i**  或  **o**  的位置(哪个字母先出现就计算哪个)：
```shell
str="My heart will go on"
echo `expr index "$str" io `
```

2022/10/18/1:03
学习至shell 数组......

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE4NTg1NDM2ODhdfQ==
-->