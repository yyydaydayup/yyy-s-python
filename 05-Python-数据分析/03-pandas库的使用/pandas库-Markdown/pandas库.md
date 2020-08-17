[toc]


# 一.Pandas基础
## 01.Pandas介绍

### 1.1 Why Pandas？

numpy已经能够帮助我们处理数据，能够结合matplotlib解决我们数据分析的问题，那么pandas学习的目的在什么地方呢？

numpy能够帮我们处理处理数值型数据，但是这还不够， 很多时候，我们的数据除了数值之外，还有字符串，还有时间序列等

> 比如：我们通过爬虫获取到了存储在数据库中的数据

所以，pandas出现了。



### 1.2 What Pandas？

> Pandas的名称来自于面板数据（panel data）

Pandas是一个强大的分析结构化数据的工具集，基于NumPy构建，提供了**高级数据结构**和**数据操作工具**，它是使Python成为强大而高效的数据分析环境的重要因素之一。

- 一个强大的分析和操作大型结构化数据集所需的工具集
- 基础是NumPy，提供了高性能矩阵的运算
- 提供了大量能够快速便捷地处理数据的函数和方法
- 应用于数据挖掘，数据分析
- 提供数据清洗功能

详情请见官网：[Pandas官网](http://pandas.pydata.org/)

### 1.3 How Pandas?

#### 1.3.1 安装

```shell
pip install pandas
```

#### 1.3.2 导入

```python
import pandas as pd
```





## 02.Pandas数据结构

Pandas有两个最主要也是最重要的数据结构：**Series**和**DataFrame**。

### 2.1 Series

#### 2.1.1 介绍

[pandas官网-Series介绍](https://pandas.pydata.org/docs/reference/api/pandas.Series.html)

Series是一种**带标签**的**一维**数组型对象，能够保存任何数据类型(int, float, str, python object...)。

Series包含**数据**和**标签（索引）**两部分：

* 索引(index)在左，数据(values)在右
* 索引是**自动创建**的，默认从0开始编号；当然，也可以像使用字典时指定键一样指定索引

![Series数据结构](pandas库.assets/Series数据结构.png)

#### 2.1.2 创建

**1.通过列表创建**

```python
>>> import pandas as pd


>>> s1 = pd.Series([1,2,3,4,5])
>>> s1

0    1
1    2
2    3
3    4
4    5
dtype: int64

>>> type(s1)
pandas.core.series.Series
```

**2.通过numpy数组创建**

```python
>>> import pandas as pd
>>> import numpy as np

>>> arr1 = np.arange(1,6)
>>> arr1
array([1, 2, 3, 4, 5])

>>> s2 = pd.Series(arr1)  # 简单创建，使用默认索引(0~n-1)
>>> s2
0    1
1    2
2    3
3    4
4    5
dtype: int32
    
>>> s2 = pd.Series(arr1, index=['a', 'b', 'c','d','e'])  # 指定索引
>>> s2
a    1
b    2
c    3
d    4
e    5
dtype: int32
    
>>> s2.index  # Series的index属性其实是一个对象
Index(['a', 'b', 'c', 'd', 'e'], dtype='object')

>>> s2.values  # 可通过Series的values属性查看其values
array([1, 2, 3, 4, 5])
```

**3.通过字典创建**

```python
>>> my_dict = {'name':'张三','age':18, 'height':175}
>>> s3 = pd.Series(my_dict, index=['name','height','age','sex'])  # index指定顺序
# 通过字典创建Series时，索引长度可与数据长度不同哦~若通过数组和列表创建Series时，这种操作直接报错！
>>> s3
name       张三
height    175
age        18
sex       NaN
dtype: object
```

#### 2.1.3 索引

**1.位置索引**

```python
>>> my_dict = {'name':'张三','age':18, 'height':175}
>>> s = pd.Series(my_dict, index=['name','height','age','sex']) 
>>> s
name       张三
height    175
age        18
sex       NaN
dtype: object
    
>>> s[0]  # 1.索引一个
'张三'

>>> s[[1,3]]  # 2.索引多个，返回的是其子Series对象
height    175
sex       NaN
dtype: object
    
>>> s[1:3]  # 3.通过“下标”切片——不包含末端
height    175
age        18
dtype: object 
```

**2.标签索引**

```python
>>> my_dict = {'name':'张三','age':18, 'height':175}
>>> s = pd.Series(my_dict, index=['name','height','age','sex']) 
>>> s
name       张三
height    175
age        18
sex       NaN
dtype: object
    
    
>>> s['age'] # 1.索引一个
18
>>> s[['name','age']]  # 2.索引多个，返回的是其子Series对象
name    张三
age     18
dtype: object
>>> s['name': 'age']  # 3.通过“标签”切片——包含末端！！！！
name       张三
height    175
age        18
dtype: object
    
```

**3.布尔索引**

```python
>>> s = pd.Series(arr1, index=['a', 'b', 'c','d','e'])
>>> s
a    1
b    2
c    3
d    4
e    5
dtype: int32

>>> s[s>3]
d    4
e    5
dtype: int32
```

**4.层级索引**

层级索引在后期常用来数据的**分组**和**生成透视表**。

**(1)指定多层索引**

```python
>>> s = pd.Series(np.random.randint(100,size=(12,)),index = [['a','a','a','b','b','b','c','c','c','d','d','d'],[0,1,2,0,1,2,0,1,2,0,1,2]])
>>> s

a  0    80
   1    25
   2    98
b  0    24
   1    89
   2    63
c  0    93
   1    87
   2    14
d  0    67
   1    60
   2    86
dtype: int32
    
>>> type(s.index)
pandas.core.indexes.multi.MultiIndex

>>> s.index
MultiIndex([('a', 0),
            ('a', 1),
            ('a', 2),
            ('b', 0),
            ('b', 1),
            ('b', 2),
            ('c', 0),
            ('c', 1),
            ('c', 2),
            ('d', 0),
            ('d', 1),
            ('d', 2)],
           )
```

**(2)多层索引的引用**

同二维数组的花式索引：**以逗号为分隔，逗号左边是外层索引，逗号右边是内层索引**

```python
>>> s['b']  # 外层引用
0    24
1    89
2    63
dtype: int32
    
>>> s[:,2]  # 内层引用
a    98
b    63
c    14
d    86
dtype: int32
    
>>> s['c',2]
14
```

**(3)索引交换**

```python
>>> s.swaplevel()  # 默认不会改变原Series对象，若想改变原对象，只需令copy=False
0  a    80
1  a    25
2  a    98
0  b    24
1  b    89
2  b    63
0  c    93
1  c    87
2  c    14
0  d    67
1  d    60
2  d    86
dtype: int32
    
    
>>> s.swaplevel().sort_index()
0  a    80
   b    24
   c    93
   d    67
1  a    25
   b    89
   c    87
   d    60
2  a    98
   b    63
   c    14
   d    86
dtype: int32
```



#### 2.1.4 常用属性与方法

**(1)构造函数**

```python
>>> help(pd.Series)

class Series(pandas.core.base.IndexOpsMixin, pandas.core.generic.NDFrame)
 |  Series(data=None, index=None, dtype=None, name=None, copy=False, fastpath=False)
    ……
```

**(2)常用属性——`name`和`index.name`**

```python
>>> arr = np.arange(1,6)
>>> arr
array([1, 2, 3, 4, 5])
    
>>> s = pd.Series(arr, index=['a', 'b', 'c','d','e'])  # 指定索引
>>> s
a    1
b    2
c    3
d    4
e    5
dtype: int32
    
>>> s.name = '序列'          # 对象名
>>> s.index.name = '索引'    # 对象索引名
索引
0    1
1    2
2    3
3    4
4    5
Name: 序列, dtype: int32
```



**(3)常用方法——`isnull()`和`notnull()`——检查缺失值**

```python
>>> my_dict = {'name':'张三','age':18, 'height':175}
>>> s = pd.Series(my_dict, index=['name','height','age','sex']) 
>>> s
name       张三
height    175
age        18
sex       NaN
dtype: object
    
>>> s.isnull()
name      False
height    False
age       False
sex        True
dtype: bool

>>> s.notnull()
name       True
height     True
age        True
sex       False
dtype: bool
```

**(4)常用方法——`head()`和`tail()`**

```python
>>> arr = np.arange(1,6)
>>> arr
array([1, 2, 3, 4, 5])
    
>>> s = pd.Series(arr, index=['a', 'b', 'c','d','e'])  # 指定索引
>>> s
a    1
b    2
c    3
d    4
e    5
dtype: int32
    
    
>>> s.head(3)  # 若不指定，则默认取5条数据
a    1
b    2
c    3
dtype: int32

>>> s.tail(3)
c    3
d    4
e    5
dtype: int32
```



#### 2.1.5 作业练习

**题目**

![Series作业题目](pandas库.assets/Series作业题目.png)

1. 按照上表创建Series对象（对象名“score”，索引名“class”）
2. 查看1-5班的数据
3. 查看那个班级的成绩还没有导入
4. 获取11班的平均成绩
5. 有一道题目的参考答案是错误的，所以在每位同学现有的成绩上加2分
6. 找出平均成绩在90分以上的班级



```python
>>> import pandas as pd
>>> import numpy as np

# 1.创建一个Series对象
>>> score = pd.Series(data=[90,95,85,78,np.NAN,96,94,np.NAN,80,87,86,83],index=range(1,13),name='score')
>>> score.index.name = "class"
>>> score
class
1     90.0
2     95.0
3     85.0
4     78.0
5      NaN
6     96.0
7     94.0
8      NaN
9     80.0
10    87.0
11    86.0
12    83.0
Name: score, dtype: float64
        
# 2.查看1-5班的成绩
>>> score[0:5]   # 此时用的是下标切片索引，所以从0开始
class
1    90.0
2    95.0
3    85.0
4    78.0
5     NaN
Name: score, dtype: float64
 
# 3.查看哪个班级的成绩还没有录入
>>> score[score.isnull()]
class
5   NaN
8   NaN
Name: score, dtype: float64

# 4.获取11班的平均成绩
>>> score[11]   # 此时用的就是“标签”索引了
86.0

# 5.有一道题目的参考答案是错误的，所以在每位同学现有的成绩上加2分
>>> score += 2
>>> score
class
1     92.0
2     97.0
3     87.0
4     80.0
5      NaN
6     98.0
7     96.0
8      NaN
9     82.0
10    89.0
11    88.0
12    85.0
Name: score, dtype: float64

# 6.找出平均成绩在90分以上的班级
>>> score[score>=90]
class
1    92.0
2    97.0
6    98.0
7    96.0
Name: score, dtype: float64
```



### 2.2 DataFrame

#### 2.2.1 介绍

[pandas官网-DataFrame介绍](https://pandas.pydata.org/docs/user_guide/dsintro.html#dataframe)

DataFrame是一个表格型的数据结构，它含有一组有序的列，每列可以是不同类型的值。DataFrame既有行索引，也有列索引，它可以被看做是由Series组成的字典（共用一个索引），数据是以二维结构存放的。

* 类似多维数组/表格数据（eg：Excel，R中的data.frame）
* 每列数据可以是不同的类型
* 索引包括行索引和列索引（在DataFrame的索引是**列优先**的）

![DataFrame数据结构](pandas库.assets/DataFrame数据结构.png)

#### 2.2.2 创建

** 1.字典类——一条数据是一列**

**(1)数组、列表或元组构成的字典构造DataFrame**

```python
# ==================构造===================
>>> data = {  # 先构造一个字典
    'a': [1,2,3,4],
    'b': (5,6,7,8),
    'c': np.arange(9,13)
}
>>> df1 = pd.DataFrame(data)  # 构造DataFrame
>>> df1
	a	b	c
0	1	5	9
1	2	6	10
2	3	7	11
3	4	8	12

# ==============查看属性：行索引、列索引、值===============
# index属性查看行索引
>>> df1.index  
RangeIndex(start=0, stop=4, step=1)

# columns属性查看列索引
>>> df1.columns  
Index(['a', 'b', 'c'], dtype='object')

# values属性查看值
>>> df1.values
array([[ 1,  5,  9],
       [ 2,  6, 10],
       [ 3,  7, 11],
       [ 4,  8, 12]], dtype=int64)

# =============指定属性：行索引、列索引============
# 指定行索引index
>>> df2 = pd.DataFrame(data,index=['A','B','C','D'])
>>> df2
	a	b	c
A	1	5	9
B	2	6	10
C	3	7	11
D	4	8	12

# 指定列索引columns
>>> df3 = pd.DataFrame(data, index=['A','B','C','D'], columns=['a','b','c','d'])
>>> df3

	a	b	c	d
A	1	5	9	NaN
B	2	6	10	NaN
C	3	7	11	NaN
D	4	8	12	NaN
```



**(2)Series构成的字典构造DataFrame**

```python
>>> df = pd.DataFrame({
    'a': pd.Series(np.arange(3)),
    'b': pd.Series(np.arange(3,5))})
>>> df
	a	b
0	0	3.0
1	1	4.0
2	2	NaN
```



**(3)字典构成的字典构造DataFrame（字典嵌套）**

```python
>>> data = {
        '语文': {'张三': 78, '李四': 87, '王五':92},
        '数学': {'张三': 93, '李四': 67, '王五':84},
        '英语': {'张三': 83, '李四': 99, },
	}

>>> df = pd.DataFrame(data)
>>> df
	语文	数学	英语
张三	78	93	83.0
李四	87	67	99.0
王五	92	84	NaN
```

* 注意：一条数据是**一列**。

** 2.列表类——一条数据是一行**

**(1)二维数组构造DataFrame**

```python
>>> arr = np.arange(12).reshape(4,3)
>>> df = pd.DataFrame(arr)
>>> df
	0	1	2
0	0	1	2
1	3	4	5
2	6	7	8
3	9	10	11
```



**(2)字典构成的列表构造DataFrame**

```python
>>> data = [{'张三': 78, '李四': 87, '王五':92},
    	   {'张三': 93, '李四': 67, '王五':84},
	       {'张三': 83, '李四': 99, }]
>>> df = pd.DataFrame(data)
>>> df
	张三	李四	王五
0	78	87	92.0
1	93	67	84.0
2	83	99	NaN
```

* 注意：此时一条数据是**一行**。



**(3)Series构成的列表构造DataFrame**

```python
>>> data = [pd.Series(np.arange(0,3)), pd.Series(np.arange(3,5))]
>>> df = pd.DataFrame(data)
>>> df
	0	1	2
0	0.0	1.0	2.0
1	3.0	4.0	NaN
```

#### 2.2.3 索引

##### 1.使用方括号

```python
import numpy as np
import pandas as pd

>>> df = pd.DataFrame(np.arange(12).reshape(3,4), index=['a','b','c'], columns=['A','B','C','D'])
>>> df
	A	B	C	D
a	0	1	2	3
b	4	5	6	7
c	8	9	10	11


# 1.列索引
>>> df['A']  # 注意：得到的是一个Series对象
a    0
b    4
c    8
Name: A, dtype: int32
        
# 2.元素索引
>>> df['A']['b']
4

# 3.切片行索引（使用切片得到的是行索引结果，但不管几行，得到的均是DataFrame对象）
>>> df[0:1]

	A	B	C	D
a	0	1	2	3
```

##### 2.高级索引

```python
import numpy as np
import pandas as pd

>>> df = pd.DataFrame(np.arange(12).reshape(3,4), index=['a','b','c'], columns=['A','B','C','D'])
>>> df
	A	B	C	D
a	0	1	2	3
b	4	5	6	7
c	8	9	10	11

>>> df.loc['a':'b','B':'C']  # loc用于标签索引（注意最外层是方括号！）
	B	C
a	1	2
b	5	6

>>> df.iloc[0:2,1:3]  # iloc用于位置索引
	B	C
a	1	2
b	5	6
```

* 通过上面的例子可知，`loc[]`和`iloc[]`和numpy数组的**花式索引**及其类似，所以用这个还是相当方便的~



#### 2.2.4 基本用法

PS：下面的四种操作用的均是1中的DataFrame。

**1.转置**

```python
>>> df = pd.DataFrame(np.arange(9).reshape(3,3),index=['a','b','c'],columns=['A','B','C'])
>>> df
	A	B	C
a	0	1	2
b	3	4	5
c	6	7	8

>>> df.T

	a	b	c
A	0	3	6
B	1	4	7
C	2	5	8
```



**2.通过列索引获取列数据**

```python
>>> df['A']
a    0
b    3
c    6
Name: A, dtype: int32

>>> type(df['A'])
<class 'pandas.core.series.Series'>
```

**3.增加列**

```python
>>> df['D'] = [100, 101, 102]
>>> df
	A	B	C	D
a	0	1	2	100
b	3	4	5	101
c	6	7	8	102
```

**4.删除列**

```python
>>> del df['D']
>>> df
	A	B	C
a	0	1	2
b	3	4	5
c	6	7	8
```

#### 2.2.5 作业练习

![DataFrame作业题目](pandas库.assets/DataFrame作业题目.png)

```python
# =======================1.创建表格==========================
>>> data = {
    	"姓名":['张三','李四','王五','小明','小红','小刚','小亮'],
    	"语文":[89,78,79,89,90,87,83],
    	"数学":[59,83,85,92,67,81,77],
    	"英语":[84,97,88,83,67,73,71],
    	"体育":[0,0,0,0,0,0,0]
	}
>>> df = pd.DataFrame(data)
>>>df

	姓名	语文	数学	英语	体育
0	张三	89	59	84	0
1	李四	78	83	97	0
2	王五	79	85	88	0
3	小明	89	92	83	0
4	小红	90	67	67	0
5	小刚	87	81	73	0
6	小亮	83	77	71	0
# =======================2.转置==========================
>>> df.T

	0	1	2	3	4	5	6
姓名	张三	李四	王五	小明	小红	小刚	小亮
语文	89	78	79	89	90	87	83
数学	59	83	85	92	67	81	77
英语	84	97	88	83	67	73	71
体育	0	0	0	0	0	0	0

# =======================3.删除“体育”列==========================
>>> del df['体育']
>>> df
	姓名	语文	数学	英语
0	张三	89	59	84
1	李四	78	83	97
2	王五	79	85	88
3	小明	89	92	83
4	小红	90	67	67
5	小刚	87	81	73
6	小亮	83	77	71

# =======================4.添加“综合”列==========================
>>> df['综合'] = [97, 87, 78, 76, 84, 88, 91]
>>> df

	姓名	语文	数学	英语	综合
0	张三	89	59	84	97
1	李四	78	83	97	87
2	王五	79	85	88	78
3	小明	89	92	83	76
4	小红	90	67	67	84
5	小刚	87	81	73	88
6	小亮	83	77	71	91
```

## 03.对齐运算

### 3.1 数据对齐

#### 3.1.1 Series

两个Series对象做运算，只有二者共有的标签所对应的值才会真正运算，只有一方有的标签值全部替换成NaN。

之所以会产生这样的结果，是因为先对齐，后运算，对齐时没有的标签自动添入NAN，又因为NAN和任何值运算都得NAN，所以只有一方有的标签会在运算结果中显示为NAN。

```python
>>> import numpy as np
>>> import pandas as pd


>>> s1 = pd.Series(np.arange(4), index=['a', 'b','c','d'])
>>> s1
a    0
b    1
c    2
d    3
dtype: int32
    
>>> s2 = pd.Series(np.arange(5), index=['a', 'c','e','f','g'])
>>> s2
a    0
c    1
e    2
f    3
g    4
dtype: int32

>>> s1+s2
a    0.0
b    NaN
c    3.0
d    NaN
e    NaN
f    NaN
g    NaN
dtype: float64
```

#### 3.1.2 DataFrame

DataFrame的对齐更上一层楼，行索引和列索引都会自动对齐，示例代码如下：

```python
>>> df1 = pd.DataFrame(np.arange(12).reshape(4,3),index=['a','b','c','d'],columns=list('ABC'))
>>> df1
	A	B	C
a	0	1	2
b	3	4	5
c	6	7	8
d	9	10	11

>>> df2 = pd.DataFrame(np.arange(9).reshape(3,3),index=['a','d','f'],columns=list('ABD'))
>>> df2
	A	B	D
a	0	1	2
d	3	4	5
f	6	7	8

>>> df1+df2

	A	B	C	D
a	0.0	2.0	NaN	NaN
b	NaN	NaN	NaN	NaN
c	NaN	NaN	NaN	NaN
d	12.0	14.0	NaN	NaN
f	NaN	NaN	NaN	NaN
```



### 3.2 使用填充值的算术方法

在3.1.1节的示例代码中，如果我想在对齐时没有的标签填充不是NAN，而是指定值，这时候就要使用Series和DataFrame对象自带的算数方法了，示例代码如下（下面代码承接3.1.1节代码）：

```python
>>> s1.add(s2,fill_value=0)
a    0.0
b    1.0
c    3.0
d    3.0
e    2.0
f    3.0
g    4.0
dtype: float64
```

上述运算中，先对齐（没有的标签填写`fill_value`指定的值），型号相同后再进行运算。

可以

### 3.3 DataFrame和Series混合运算

先来复习一下numpy中数组的广播机制：

```python
>>> arr = np.arange(12).reshape(3,4)
>>> arr
array([[ 0,  1,  2,  3],
       [ 4,  5,  6,  7],
       [ 8,  9, 10, 11]])

>>> arr[0]
array([0, 1, 2, 3])

>>> arr-arr[0]  # 每一行都会减去arr[0]
array([[0, 0, 0, 0],
       [4, 4, 4, 4],
       [8, 8, 8, 8]])
```

其实，上面的arr就相当于pandas中的DataFrame，而arr[0]则是Series，有了上面的基础，再来思考DataFrame和Series的混合运算，就简单许多了。

先来看一下对行的操作：

```python
>>> df = pd.DataFrame(np.arange(12).reshape(4,3),index=['a','b','c','d'],columns=list('ABC'))
>>> df
	A	B	C
a	0	1	2
b	3	4	5
c	6	7	8
d	9	10	11

>>> s1 = df.loc['a']  # 取出一行
>>> s1
A    0
B    1
C    2
Name: a, dtype: int32
        
>>> df - s1  # 每一行都减去s1，这和上面的数组广播是一模一样滴~
	A	B	C
a	0	0	0
b	3	3	3
c	6	6	6
d	9	9	9
```

上面是一次减去一行，如果我想一次减去一列呢？

```python
>>> s2 = df['A']
>>> s2
a    0
b    3
c    6
d    9
Name: A, dtype: int32
        
>>> df-s2  # 暴力作差的后果
	A	B	C	a	b	c	d
a	NaN	NaN	NaN	NaN	NaN	NaN	NaN
b	NaN	NaN	NaN	NaN	NaN	NaN	NaN
c	NaN	NaN	NaN	NaN	NaN	NaN	NaN
d	NaN	NaN	NaN	NaN	NaN	NaN	NaN

>>> df.sub(s2, axis='index')  # 注意：这里用的是行索引！！！axis='index' <=> axis=0
	A	B	C
a	0	1	2
b	0	1	2
c	0	1	2
d	0	1	2
```

## 04.常用函数与方法

### 4.1 函数

#### 4.1.1 直接使用Numpy函数

pandas是基于numpy开发的，很多numpy中好用的函数在pandas中都是可以直接用的。

```python
>>> df = pd.DataFrame(np.random.randn(3,2))
>>> df
	0			1
0	-0.224400	0.806205
1	-0.156604	0.078354
2	0.622751	0.796878

>>> np.abs(df)
	0			1
0	0.224400	0.806205
1	0.156604	0.078354
2	0.622751	0.796878
```

#### 4.1.2 应用自定义函数

** 1.通过`apply()`方法将函数应用到列或行**

```python
>>> df = pd.DataFrame(np.random.rand(4,3),index=list('abcd'),columns=list('ABC'))
>>> df
	A			B			C
a	0.850124	0.444995	0.733747
b	0.880202	0.839141	0.485449
c	0.197179	0.076783	0.819077
d	0.086722	0.274434	0.482028

>>> f_max = lambda x:x.max()
>>> df.apply(f_max)  # 默认情况下：axis=0
A    0.880202
B    0.839141
C    0.819077
dtype: float64
    
>>> df.apply(f_max,axis=1)
a    0.850124
b    0.880202
c    0.819077
d    0.482028
dtype: float64
```

** 2.通过`applymap()`方法将函数应用到每一个数据**

```python
>>> f_fmt = lambda x:'%.2f'%x
>>> df.applymap(f_fmt)  # 这里的df即上面代码段中的df
	A		B		C
a	0.85	0.44	0.73
b	0.88	0.84	0.49
c	0.20	0.08	0.82
d	0.09	0.27	0.48
```



### 4.2 方法

#### 4.2.1 排序

** 1.索引排序——`sort_index()`方法**

先来看看Series中的`sort_index()`方法：

```python
>>> s = pd.Series(np.arange(4), index=list('dbca'))
>>> s
d    0
b    1
c    2
a    3
dtype: int32
    
>>> s.sort_index()  # 默认升序
a    3
b    1
c    2
d    0
dtype: int32
    
>>> s.sort_index(ascending=False)  # 指定降序
d    0
c    2
b    1
a    3
dtype: int32
```

再来看看DataFrame中的`sort_index()`方法，其实大同小异，只不过多了一个维度，需要指定轴：

```python
>>> df = pd.DataFrame(np.arange(12).reshape(4,3),index=list('bdca'),columns=list('CBA'))
>>> df
	C	B	A
b	0	1	2
d	3	4	5
c	6	7	8
a	9	10	11

>>> df.sort_index()  # 按照行排序
	C	B	A
a	9	10	11
b	0	1	2
c	6	7	8
d	3	4	5

>>> df.sort_index(axis=1)  # 按照列排序
	A	B	C
b	2	1	0
d	5	4	3
c	8	7	6
a	11	10	9
```



** 2.值排序——`sort_values()`方法**

先看看Series中的`sort_values()`方法：

```python
>>> s = pd.Series([5,np.nan,6,9,np.nan])
>>> s
0    5.0
1    NaN
2    6.0
3    9.0
4    NaN
dtype: float64
    
>>> s.sort_values()  # 默认升序，NAN会被放到最后
0    5.0
2    6.0
3    9.0
1    NaN
4    NaN
dtype: float64
    
>>> s.sort_values(ascending=False)  # 使用降序，NAN依然被放在最后
3    9.0
2    6.0
0    5.0
1    NaN
4    NaN
dtype: float64
```

再来看看DataFrame中的`sort_values()`方法，其实大同小异，只不过多了一个维度，需要指定轴：

```python
>>> df = pd.DataFrame(np.random.randint(20,size=(4,3)),index=list('abcd'),columns=list('ABC'))
>>> df
	A	B	C
a	0	8	12
b	15	0	1
c	8	12	18
d	15	19	11

>>>df.sort_values(by=['A']) # 通过by参数指定参照哪几列进行排序
	A	B	C
a	0	8	12
c	8	12	18
b	15	0	1
d	15	19	11

>>> df.sort_values(by=['b'],axis=1)  # 如果想对行排序，同样通过by指定，只不过此时要指定axis=1
	B	C	A
a	8	12	0
b	0	1	15
c	12	18	8
d	19	11	15
```

#### 4.2.2 缺失值处理

```python
>>> df = pd.DataFrame([np.random.randn(3), [1., 2., np.nan],
                    [np.nan, 4., np.nan], [1., 2., 3.]])
>>> df
	0			1			2
0	-0.017182	0.825946	-0.51278
1	1.000000	2.000000	NaN
2	NaN	4.000000	NaN
3	1.000000	2.000000	3.00000

>>> df.isnull()
	0		1		2
0	False	False	False
1	False	False	True
2	True	False	True
3	False	False	False

>>> df.dropna()  # 默认丢弃行
	0			1			2
0	-0.017182	0.825946	-0.51278
3	1.000000	2.000000	3.00000

>>> df.dropna(axis=1)  # 指定丢弃列
	1
0	0.825946
1	2.000000
2	4.000000
3	2.000000

>>> df.fillna(100)  # 填充缺失数据
	0			1			2
0	-0.017182	0.825946	-0.51278
1	1.000000	2.000000	100.00000
2	100.000000	4.000000	100.00000
3	1.000000	2.000000	3.00000
```

#### 4.2.3 唯一值问题

```python
>>> s = pd.Series([2,6,8,9,8,3,6],index=['a','a','c','c','c','c','c'])
>>> s
a    2
a    6
c    8
c    9
c    8
c    3
c    6
dtype: int64
    
>>> s.unique()
array([2, 6, 8, 9, 3], dtype=int64)  # 返回的是一个ndarray哎

>>> s.index.is_unique
False

>>> s.value_counts()
6    2
8    2
3    1
2    1
9    1
dtype: int64
    
>>> s.isin([2,8])
a     True
a    False
c     True
c    False
c     True
c    False
c    False
dtype: bool
```





#### 4.2.4 统计计算

[pandas官网-Series统计描述](https://pandas.pydata.org/docs/reference/series.html#computations-descriptive-stats)

[pandas官网-DataFrame](https://pandas.pydata.org/docs/reference/frame.html#computations-descriptive-stats)



![常用的统计计算](pandas库.assets/常用的统计计算.png)



# 二、数据读写

[pandas官网-数据读写函数](https://pandas.pydata.org/docs/user_guide/io.html)

![数据读写函数一览表](pandas库.assets/数据读写函数一览表.png)

![数据读写函数参数一览表](pandas库.assets/数据读写函数参数一览表.png)

# 三、数据清洗

# 四、聚合和分组

