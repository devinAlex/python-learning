# python-learning
### 一，如何在列表、字典、集合中根据条件筛选数据？
#### 实际案例:
1,过滤掉列表[3,9,-1,10,20,-2...]中的负数</br>
2,筛出字典{'lilei':79,'Jim':88,'Lucy':92...}中高于90的项</br>
3,筛出集合{77,89,32,20....}中能被3整除的元素</br>
#### 方案：
1，列表：filter函数 filter(lambda x:x>=0,data)</br>
		 列表解析 [x for x in data if x>=0]</br>
2, 字典：字典解析{k:v for k, v in d.iteritems() if v > 90}</br>
3,集合：集合解析 {x for x in s if x%3 == 0}</br>
### 二，如何为元组中的每个元素命名，提高程序的可读性？
#### 实际案例：</br>
学生信息系统中数据为固定格式：（名字，年龄，性别，邮箱地址。。）学生数量很大为了减小存储开销，对每个学生信息用元组表示：</br>
('Jim',16,'male','jim787@gmail.com')</br>
('LiLei',17,'male','lilei@qq.com')</br>
('LUcy',16,'female','Lucy123@yahoo.com')...</br>
访问时，我们使用引索（index）访问，大量引索降低程序可读性。</br>
#### 解决方案：</br>
1，定义类似与其他语言的枚举类型，也就是定义一系列数值常量。</br>
2，使用标准库中collection.namedtuple替代内置tuple.</br>
```
# NAME = 0
# AGE = 1
# SEX = 2
NAME, AGE, SEX = xrange(3)
student = ('Jim', 16, 'male','jim787@gmail.com')
print student[NAME]
if student[AGE] >= 17:
    pass
if student[SEX] == 'male':
    pass
 ```
```
from collections import namedtuple
Student = namedtuple('Student',['name','age','sex','email'])
s = Student('Jim', 16, 'male','jim787@gmail.com')
print s.name
print isinstance(s, tuple)	
```
### 三，如何统计序列中元素的出现频度？
#### 实际案例：
1，某随机序列[13,3,5,4,6,3,4,5,...]中，找到出现次数最高的3个元素，他们出现的次数是多少？</br>
2，对某英文文章的单词，进行词频统计，找到出现次数最高的10个单词，他们出现的次数是多少？</br>
#### 解决方案：
使用collections.Counter对象，将序列传入Counter的构造器，得到Counter对象是元素频度的字典。</br>
Counter.most_common(n)方法得到频度最高的n个元素的列表。</br>
```
from random import randint
data = [randint(0, 20) for _ in xrange(30)]
c = dict.fromkeys(data, 0)
for x in data:
    c[x] += 1
==========================================	
from collections import Counter
from random import randint
data = [randint(0, 20) for _ in xrange(30)]
print data
c2 = Counter(data)
print c2.most_common(3)
```
```
from collections import Counter
import re
txt = open('C:\\Users\\Administrator\\Desktop\\cc.txt').read()
c3 = Counter(re.split('\W+', txt))
print c3.most_common(10)
```
