## 一. Shell 脚本编程

### 1.1 学习路径
- 观看老师视频
- 在GNU官网，查看Bash官方手册
- 遇到的具体问题，通过上网查找相关答案。


### 1.2 Shell脚本编程应用
- 简单的shell command堆叠，完成具体的工作。
- 模仿其他编程语言，处理复杂的文本和业务逻辑(不推荐，建议使用Python等其他编程语言完成此类工作。学有余力的同学可以归纳整理一下shell的语法和使用细节，加深对Shell编程的理解)


## 二. Shell 脚本编程的基本用法

### 2.1 脚本文件首行写上: `#!/bin/bash`

### 2.2 编写脚本文件的备注信息，业务逻辑的备注信息，方便后期维护管理
比如:

``` bash
#!/bin/bash

#==============================
#Name    :      test.sh
#Version :      1.6
#Date    :      20180728
#Author  :      magedu
#Website :      www.magedu.com
#==============================



# 1. 使用echo 输出我爱Linux， 我爱马哥教育
echo "I love Linux, I love Magedu"

# 2. 使用date 命令获取系统时间并赋给变量today,然后使用echo 输出变量 today
today=`date`
echo $today
```

完成脚本编写后使用 bash 命令处理 test.sh 脚本
```bash
[root@localhost ~]# bash test.sh 
I love Linux, I love Magedu
Fri Jul 27 18:40:02 CST 2018
[root@localhost ~]#
[root@localhost ~]#
```

### 2.3 脚本的执行方法
- 使用 bash 执行脚本
- 给脚本设置可执行权限后直接执行脚本
- 使用 source 执行脚本


## 三. Shell 脚本中的变量

### 3.1 变量的定义和删除
使用 = 创建一个变量，等号的左边是变量名称，右边是变量的值，比如：  

`myName=gordon`  
`myAge=36`
> 注意，等号两遍没有空格。

除了直接使用 = 定义变量外，还可以使用 declare 语法定义变量，这样可以指明变量的属性，定义后使用delcare -p 查看变量的属性 比如：

```bash
[root@localhost ~]# myAge=36
[root@localhost ~]# declare -p myAge
declare -- myAge="36"
[root@localhost ~]# declare -i myAge
[root@localhost ~]# declare -p myAge
declare -i myAge="36"
[root@localhost ~]# declare -i myAge=38
[root@localhost ~]# declare -p myAge
declare -i myAge="38"
[root@localhost ~]# 
[root@localhost ~]#
```
> declare 属性中 -- 和 -i的区别稍后介绍

处理定义变量外，shell中可以使用unset命令删除一个变量，比如：


```bash
[root@localhost ~]# declare  myAge=55
[root@localhost ~]# declare  -p myAge
declare -- myAge="55"
[root@localhost ~]# echo $myAge
55
[root@localhost ~]# unset myAge
[root@localhost ~]# echo $myAge

[root@localhost ~]#
[root@localhost ~]# declare  -p myAge
-bash: declare: myAge: not found
[root@localhost ~]#
[root@localhost ~]#
```

### 3.2 Shell中的可操作对象

- 普通变量(字符串)
- 整数(符合整数格式的普通变量)
- 数组
- 函数


#### 3.2.1 变量名
创建一个变量时，首先应该给他起一个良好的名字，这样方便使用。  
Shell 中变量名有大小写字母、下划线、数字构成，变量名不能由数字开头。  
比如我们创建一个学生姓名的变量：  
Stu_name=guodegang

#### 3.2.2 系统变量
使用shell时，有些系统变量已经提前创建好了，我们正常使用就好，不要使用系统变量名创建变  
量，这样会造成冲突，系统变量通常为全大写字母，比如：  
PATH，HOSTNAME等等，可以使用env命令或者set命令列出当前系统的变量。  

#### 3.2.3 关键字
定义变量除了不能使用系统变量名外，关键字也不能使用，比如：  
if、fi、break、for 、while等等。

#### 3.2.4 自定义变量
自定义变量是shell编程中使用频度最高的变量，使用者根据业务逻辑和需要，自行创建变量。
Shell中使用最多的变量是普通变量或者称为字符串变量。使用之前介绍的创建变量方式即可创建，比如： 
`Stu_name=zhangsan`  
之所以称为字符串变量，是因为变量值可以使用 ' ' 或者 " " 包裹起来，内部可以是各种需要的内容，比如:  
`Stu_name= "William Henry Gates III -- Bill" `

" " 和 ' ' 的区别在于，" "为弱引用，包裹的内容包含变量是会被替换，而' '则不会被替换比如： 

```bash
[root@localhost ~]# Stu_name=magedu
[root@localhost ~]# echo "$Stu_name"
magedu
[root@localhost ~]# echo '$Stu_name'
$Stu_name
[root@localhost ~]# 
[root@localhost ~]# 
```

通过上面的对比操作，我们发现了两者的区别。那么我想在 " " 中打印一个$符号，难道就不可以吗？  
我们可以使用一个字符转义符号 \ 来进行操作，比如：  
```bash
[root@localhost ~]# Stu_name=magedu
[root@localhost ~]# echo "\$Stu_name"
$Stu_name
[root@localhost ~]#
```

#### 3.2.5 整数变量
算数运算是编程中最常见的运算操作，有广泛的使用场景。  
算数运算处理的变量类型为数值，不能是字符串，比如：

```bash
[root@localhost ~]# Stu_age=30years
[root@localhost ~]# echo `expr $Stu_age + 30 `
expr: non-numeric argument

[root@localhost ~]# Stu_age=30
[root@localhost ~]# echo `expr $Stu_age + 30 `
60
[root@localhost ~]#
```
通过上面的演示，一个普通变量既可以是字符串，也可以是数值，当把一个非数值变量进行算数运算时，  
算数运算命令报错，为了避免这样的情况发生，我们可以在定义变量时指定一个变量为整数，使用 declare -i 实现，如图：  
```bash
[root@localhost ~]# declare -i Stu_age=30
[root@localhost ~]# declare -p Stu_age
declare -i Stu_age="30"
[root@localhost ~]# Stu_age=30year
-bash: 30year: value too great for base (error token is "30year")
[root@localhost ~]# Stu_age=60
[root@localhost ~]# declare -p Stu_age
declare -i Stu_age="60"
[root@localhost ~]# 
[root@localhost ~]#
```
> 如上图，当给一个变量指定为数值行后，再次给这个变量赋值时，Shell会做检查，不符合数值规范的赋值操作将会失败。  

根据需要，也可以将一个数值型的变量恢复为普通变量，使用 declare +i 实现，如图：  
```bash
[root@localhost ~]# declare -p Stu_age
declare -i Stu_age="60"
[root@localhost ~]# declare +i Stu_age
[root@localhost ~]# declare -p Stu_age
declare -- Stu_age="60"
[root@localhost ~]# Stu_age=30year
[root@localhost ~]# declare -p Stu_age
declare -- Stu_age="30year"
[root@localhost ~]# 
[root@localhost ~]#
```

当一个变量为整数型变量时，他可以为负整数、0、正整数。并且有取值范围，-(2^63) ~ 2^63-1 , 因此有大数值运算时，需要注意。
```bash
[root@localhost ~]# echo "2^63 - 1" | bc
9223372036854775807
[root@localhost ~]# declare -i Stu_age=9223372036854775808
[root@localhost ~]# echo $Stu_age
-9223372036854775808
[root@localhost ~]# declare -i Stu_age=-9223372036854775809
[root@localhost ~]# echo $Stu_age
9223372036854775807
[root@localhost ~]#
```


#### 3.2.6 数值运算工具


# expr ：evaluate expressions  
expr 是 GNU coreutils 软件包中的一个数值计算工具，出来数值计算，他还有一些其它功能。

1. 基算数运算  
进行乘、整除、取余数是运算符要是用 \ 转义一下
```bash
[root@localhost ~]# expr 3 + 0
3
[root@localhost ~]# expr 3 - 1
2
[root@localhost ~]# expr 3 \* 2
6
[root@localhost ~]# expr 3 \/ 2
1
[root@localhost ~]# expr 8 \% 2
0
[root@localhost ~]#
```
expr 不支持复杂数值类型，仅处理整数， 运算符两边要有空格
```bash
[root@localhost ~]# expr 3 + 1
4
[root@localhost ~]# expr 3.5 + 1
expr: non-numeric argument
[root@localhost ~]# expr 3.5 +1
expr: syntax error
[root@localhost ~]#
```

2. 数值比较运算  
进行数据比较运算，成立返回 1 ， 不成立返回 0 ， 同样要是用转义符号
```bash
[root@localhost ~]# expr 2 \> 5
0
[root@localhost ~]# expr 2 \>= 5
0
[root@localhost ~]# expr 2 \< 5
1
[root@localhost ~]# expr 2 \<= 5
1
[root@localhost ~]# expr 2 = 5
0
[root@localhost ~]# expr 2 = 2
1
[root@localhost ~]# expr 2 != 2
0
[root@localhost ~]# expr 2 != 4
1
```

3. 表达式默认运算
arg1 | agr2  
当arg1和arg2中有一个值为0或者空时，返回另一个值。  
当arg1和arg2中都不为0或者空时，返回arg1  
当arg1和arg2中都为空时，返回 0 
```bash
[root@localhost ~]# expr 1 \| 2
1
[root@localhost ~]# expr 1 \| ""
1
[root@localhost ~]# expr 1 \| 0
1
[root@localhost ~]# expr "" \| 2
2
[root@localhost ~]# expr 0 \| 2
2
[root@localhost ~]# expr 0 \| 0
0
[root@localhost ~]# expr "" \| ""
0
[root@localhost ~]#
```

arg1 & agr2 当arg1和agr2中没有 空 或者 0 时，返回arg1，否则返回 0。
```bash
[root@localhost ~]# expr 3 \& 5
3
[root@localhost ~]# expr 3 \& 0
0
[root@localhost ~]# expr 3 \& ""
0
[root@localhost ~]# expr 0 \& 5
0
[root@localhost ~]# expr "" \& 5
0
[root@localhost ~]#
```

4. 字符匹配操作
str1 : str2 ,str2从str1左边开始匹配，返回匹配到第几个位置坐标。如下图:  
```bash
[root@localhost ~]# expr hello : h
1
[root@localhost ~]# expr hello : hello
5
[root@localhost ~]# expr hello : helloo
0
[root@localhost ~]# expr hello : ello
0
[root@localhost ~]#
```

match str1 str2 ，用法和上面的效果一样:
```bash
[root@localhost ~]# expr match hello h
1
[root@localhost ~]# expr match hello hello
5
[root@localhost ~]# expr match hello helloo
0
[root@localhost ~]# expr match hello ello
0
[root@localhost ~]#
```

substr str1 POS LEN， 从str1的第POS个字符开始截取LEN个字符。  
```bash
[root@localhost ~]# expr substr hello 2 2
el
[root@localhost ~]# expr substr hello 2 3
ell
[root@localhost ~]# expr substr hello 2 4
ello
[root@localhost ~]#
```

length str1 , 返回str1字符串的长度
```bash
[root@localhost ~]# expr length hello
5
[root@localhost ~]# expr length hellooo
7
[root@localhost ~]#
```

index str1 str2 , 返回str2在str1中的位置坐标,如图：  
```bash
[root@localhost ~]# expr index hello el
2
[root@localhost ~]# expr index hello h
1
[root@localhost ~]# expr index hello hel
1
[root@localhost ~]#
```
# Arithmetic Expansion 算数扩展
使用双括号将 `算数表达式` 括起来，进行运算,括号前不使用 $ 符号仅进行运行，使用$后可获取运算结果，如图：
```bash
[root@localhost ~]# ((3 + 6))
[root@localhost ~]# echo $?
0
[root@localhost ~]# name=$((8+9))
[root@localhost ~]# echo $name
17
[root@localhost ~]#
```
除了使用双括号外，还可以使用let 进行运算，他俩都是对 `算数表达式` 处理，如图。
```bash
[root@localhost ~]# let 3+9
[root@localhost ~]# echo $name
11
[root@localhost ~]# let 3+9
[root@localhost ~]# echo $?
0
[root@localhost ~]# let name=3+9
[root@localhost ~]# echo $name
12
[root@localhost ~]#
```

# arithmetic evaluation 和 arithmetic expression
Shell中算数表达式，要符合算数运算定义的规则，如下：
1. var--   var++ 数值自增或者自减
```bash
[root@localhost ~]# age=30
[root@localhost ~]# let age--
[root@localhost ~]# echo $age
29
[root@localhost ~]# let age++
[root@localhost ~]# echo $age
30
[root@localhost ~]#
```

2. 加法、减法 运算
```bash
[root@localhost ~]# echo $age
30
[root@localhost ~]# let age=age+1
[root@localhost ~]# echo $age
31
[root@localhost ~]# let age=age-5
[root@localhost ~]# echo $age
26
[root@localhost ~]#
```

3. 乘法、整除、取余、乘方运算
```bash
[root@localhost ~]# age=$((age*3))
[root@localhost ~]# echo $age
78
[root@localhost ~]# age=$((age/5))
[root@localhost ~]# echo $age
15
[root@localhost ~]# age=$((age/4))
[root@localhost ~]# echo $age
3
[root@localhost ~]# age=$((age**3))
[root@localhost ~]# echo $age
27
[root@localhost ~]#
```
4. 与、或、异或 位运算
```bash
[root@localhost ~]# age=0
[root@localhost ~]# echo $age
0
[root@localhost ~]# age=$((1&3))
[root@localhost ~]# echo $age
1
[root@localhost ~]# age=0
[root@localhost ~]# echo $age
0
[root@localhost ~]# age=$((1|7))
[root@localhost ~]# echo $age
7
[root@localhost ~]# age=0
[root@localhost ~]# echo $age
0
[root@localhost ~]# age=$((2^7))
[root@localhost ~]# echo $age
5
[root@localhost ~]# 
```
5. 
# bc ：An arbitrary precision calculator language
相对于expr 而言，bc是一个强大的计算器，后期我单独给大家介绍一下。  
简单说，bc 在数值运算时支持小数，如图：
```bash
[root@localhost ~]# echo "2+2" | bc
4
[root@localhost ~]# echo "2.2+2" | bc
4.2
[root@localhost ~]# echo "2.2 + 2" | bc
4.2
[root@localhost ~]#
```




