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
### 四，如何根据字典中值的大小，对字典中的项排序
#### 实际案例：
某班英语成绩以字典形式存储为：{'Lilei':79,'Jim':88,'Lucy':92...}根据成绩高低，计算学生排名。</br>
#### 解决方案：
使用内置函数sorted:1，利用zip将字典数据转化元组。2，传递sorted函数的key参数</br>
```
from random import randint
d = {x: randint(60, 100) for  x in 'xyzabc'}
print sorted(zip(d.itervalues(), d.iterkeys()))
print sorted(d.items(), key = lambda x:x[1])
```
### 五，如何快速找到多个字典中的公共键(key)?
#### 实际案例：
西班牙足球甲级联赛，每轮球员进球统计：</br>
第一轮：{'苏亚雷斯':1,'梅西':2，...}</br>
第二轮：{'苏亚雷斯':2,'C罗':1，...}</br>
第三轮：{'苏亚雷斯':2,'托雷斯':2，...}</br>
统计出前N轮，每场比赛都有进球的球员</br>
```
from random import randint,sample
s1 = {x: randint(1,4) for x in sample('abcdefg', randint(3,6))}
s2 = {x: randint(1,4) for x in sample('abcdefg', randint(3,6))}
s3 = {x: randint(1,4) for x in sample('abcdefg', randint(3,6))}
res = []
for k in s1:
    if k in s2 and k in s3:
        res.append(k)
print res
```
#### 解决方案：
利用集合(set)的交集操作</br>
1，使用字典的viewkeys()方法，得到一个字典keys的集合。</br>
2，使用map函数，得到所有字典的keys的集合。</br>
3，使用reduce函数，取得所有字典的Keys的集合的交集。</br>
```
from random import randint,sample
s1 = {x: randint(1,4) for x in sample('abcdefg', randint(3,6))}
s2 = {x: randint(1,4) for x in sample('abcdefg', randint(3,6))}
s3 = {x: randint(1,4) for x in sample('abcdefg', randint(3,6))}
#print s1.viewkeys() & s2.viewkeys() & s3.viewkeys()
print map(dict.viewkeys, [s1,s2,s3])
print reduce(lambda a, b: a & b, map(dict.viewkeys, [s1, s2, s3]))
```
### 六，如何实现可迭代对象和迭代器对象？
#### 实际案例：
某软件要求，从网络抓取各个城市气温信息，并依次显示：</br>
北京：15~20</br>
天津：17~22</br>
长春：12~18 ....</br>
如果一次抓取所有城市天气再显示，显示第一个城市气温时，有很高的延时，并且浪费存储空间，我们期望以“用时访问”的策略，并且能把所有城市气温封装到一个对象里，可用for语句进行迭代，如何解决？</br>
#### 解决方案：
1，实现一个迭代器对象WeatherIterator,next方法每次返回一个城市气温。</br>
2，实现一个可迭代对象WeatherIterable，__iter__方法返回一个迭代器对象。</br>
```
# coding:utf8
import requests
from collections import Iterable, Iterator
def getWeather(city):
    r = requests.get(u'http://wthrcdn.etouch.cn/weather_mini?city=' + city)
    data = r.json()['data']['forecast'][0]
    return '%s: %s, %s' % (city, data['low'], data['high'])
#[u'北京', u'上海',u'广州', u'长春']
#print getWeather(u'北京')
#print getWeather(u'长春')

class WeatherIterator(Iterator):
	def __init__(self, cities):
		self.cities = cities
		self.index = 0
	
	def getWeather(self, city):
		r = requests.get(u'http://wthrcdn.etouch.cn/weather_mini?city=' + city)
		data = r.json()['data']['forecast'][0]
		return '%s: %s, %s' % (city, data['low'], data['high'])
	
	def next(self):
		if self.index == len(self.cities):
			raise StopIteration
		city = self.cities[self.index]
		self.index += 1
		return self.getWeather(city)
class WeatherIterable(Iterable):
	def __init__(self, cities):
		self.cities = cities
	def __iter__(self):
		return WeatherIterator(self.cities)
for x in WeatherIterable([u'北京', u'上海',u'广州', u'长春']):
	print x
```