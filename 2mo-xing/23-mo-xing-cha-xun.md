## 简介

* 查询集表示从数据库中获取的对象集合
* 查询集可以含有零个，一个或多个过滤器
* 过滤器基于所给的参数限制查询的结果
* 从sql的角度，查询集和select语句等价，过滤器像where和limit子句
* 知识点:
  * 查询集
  * 字段查询:比较运算符，F对象，Q对象

---

## 查询集

* 在管理器上调用过滤器方法会返回查询集
* 查询集经过过滤器筛选后返回新的查询集，因此可以写成链式过滤
* 惰性执行:创建查询集不会带来任何数据库的访问，知道调用数据时，才会访问数据库
* 何时对查询集求值:迭代、序列化、与if合用
* 何时对查询集的方法，称为过滤器
  * all\(\)
  * filter\(\)
  * exclude\(\)
  * order\_by\(\)
  * values\(\):一个对象构成一个字典，然后构成一个列表返回
* 写法:
  \`\`\`
  filter\(键1=值1，键2=值2\) 等价于 filter\(键1=值1\).filter\(键2=值2\)

```
views:
def studentsearch(request):
      studentsList = Students.stuObj.filter(sname="美")
      return render(request,"myApp/students.html",{'students':studentsList})
urls:
  url(r'^filter/$',views.studentsearch),
```

* 返回单个值的方法
* get\\(\\):返回单个满足条件的对象
* 如果未找到会引发"模型类.DoesNotExist"异常
* 如果多条被返回，会引发"模型类.MultipleObjectsReturned"异常
* count\\(\\):返回当前查询的总条数
* first\\(\\):返回第一个对象
* last\\(\\):返回最后一个对象
* exists\\(\\):判断查询集中是否有数据，如果有则返回True

---

## 限制查询集

* \* 查询集返回列表，可以使用下标的方式进行限制，等同于sql中的limit和offset子句
* \* 注意:不支持负数索引
* \* 使用下标后返回一个新的查询集，不会立即执行查询
* \* 如果获取一个对象，直接使用\\[0\\]，等同于\\[0:1\\].get\\(\\)，可是如果没有数据,\\[0\\]引发IndexError异常\\[0:1\\].get\\(\\)引发DoestNotExist异常

---

## 查询集的缓存

* 每个查询集都包含一个缓存来最小化对数据库的方法
* 在新建的查询集中，缓存为空，首次对查询集求值时，会发生数据库查询，django会将查询的结果存在查询集的缓存中，并返回请求的结果，接下来对查询机求值将重用缓存的结果
* 情况一:这构成了两个查询集，无法重用缓存，每次查询都与数据库进行一次交互，增加了数据库的负载

print\(\[e.ename for e in Entry.objects.all\(\)\]\)  
print\(\[e.ename for e in Entry.objects.all\(\)\]\)

```
* 情况二:两次循环使用同一个查询集，第二次使用缓存中的数据
```

queryList = Entry.objects.all\(\)  
print\(e.ename for e in queryList\)  
print\(e.ename for e in queryList\)

* 何时查询集不会被缓存：当只对查询集的部分进行求值时会检查缓存，但是如果这部分不在缓存中，那么接下来查询返回的记录将不会被缓存，这意味着使用索引来限制查询集将不会填充缓存，如果这部分数据已经被缓存，则直接使用缓存中的数据

---

## 字段查询

* \* 实现where子名，作为方法filter\\(\\)、exclude\\(\\)、get\\(\\)的参数
* \* 语法:属性名称\\_比较运算符 = 值
* \* 标识两个下划线，左侧是属性名称，右侧是比较类型
* \* 对于外键，使用"属性名\\_id"表示外键的原始值
* \* 转义:like语句中使用了%与，匹配数据中的%与，在过滤器中直接写，例如:filter\\(name\\_\\_contains="%"\\) 等价于 where name like "%\%%",表示查找标题中的"%"

---

## 比较运算符

* exact:表示判等，大小写敏感；如果没有写"比较运算符"，表示判等

```
filter(isDelete=False)
```

* contains:是否包含，大小写敏感

```
exclude(sname__contains="美")
```

* startswith、endswith:以value开头或结尾，大小写敏感

```
exclue(sname__endswith="美")
```

* isnull、isnotnull:是否为null

```
filter(sname__isnull = False)
```

* 在前面加个i表示区分大小写，如iexact、icontains、istartswith、iendswith

* in:是否包饭在范围内

```
filter(ok__in = [1,2,3,4,5]
```

* gt、gte、lt、lte:大于、大于等于、小于、小于等于

```
filter(id__gt = 3)
```

* year、month、day、week\\_day、hour、minute、second:对日期间类型的属性进行运算

year、month、day、week\_day、hour、minute、second：对日期间类型的属性进行运算

```
filter(bpub_date__year=1980)
filter(bpub_date__gt=date(1980, 12, 31))
```

* 跨关联关系的查询：处理join查询
  * 语法：模型类名&lt;属性名&gt;&lt;比较&gt;
  * 注：可以没有\_\_&lt;比较&gt;部分，表示等于，结果同inner join
  * 可返向使用，即在关联的两个模型中都可以使用

```
filter(heroinfo_ _hcontent_ _contains='美')
```

* 查询的快捷方式：pk，pk表示primary key，默认的主键是id

```
filter(pk__lt=6)
```

#### 聚合函数 {#聚合函数}

* 使用aggregate\(\)函数返回聚合函数的值
* 函数：Avg，Count，Max，Min，Sum

```
from django.db.models import Max
maxAge = Students.stuObj.aggregate(Max('sname'))
print(maxAge)
```

* count的一般用法：

```
count = list.count()
```

#### F对象 {#f对象}

* 可以使用模型的字段A与字段B进行比较，如果A写在了等号的左边，则B出现在等号的右边，需要通过F对象构造

```
g = Grades.objects.filter(ggirlnum__lt=F('gboynum'))
```

* django支持对F\(\)对象使用算数运算

```
g = Grades.objects.filter(ggirlnum__lt=F('gboynum')*2)
```

* F\(\)对象中还可以写作“模型类\_\_列名”进行关联查询

```
list.filter(isDelete=F('sgrade__isDelete'))
```

* 对于date/time字段，可与timedelta\(\)进行运算

```
list.filter(a_date__lt=F('a_date') + timedelta(days=1))
```

#### Q对象 {#q对象}

* 过滤器的方法中关键字参数查询，会合并为And进行
* 需要进行or查询，使用Q\(\)对象
* Q对象\(django.db.models.Q\)用于封装一组关键字参数，这些关键字参数与“比较运算符”中的相同

```
from django.db.models import Q
list.filter(Q(pk_ _lt=6))
```

* Q对象可以使用
  &
  （and）、\|（or）操作符组合起来
* 当操作符应用在两个Q对象时，会产生一个新的Q对象

```
list.filter(pk_ _lt=6).filter(sage__gt=10)
list.filter(Q(pk_ _lt=6) | Q(sage_ _gt=10))
```

* 使用~（not）操作符在Q对象前表示取反

```
list.filter(~Q(pk__lt=6))
```

* 可以使用&\|~结合括号进行分组，构造做生意复杂的Q对象
* 过滤器函数可以传递一个或多个Q对象作为位置参数，如果有多个Q对象，这些参数的逻辑为and
* 过滤器函数可以混合使用Q对象和关键字参数，所有参数都将and在一起，Q对象必须位于关键字参数的前面

---

```
view.py / filter

"""
# def studentsearch(request):
    #过滤
    # studentsList = Students.stuObj.filter(sname="美")
    #contains
    # studentsList = Students.stuObj.filter(sname__contains = "美")
    #判等
    # studentsList = Students.stuObj.filter(isDelete=False)
    # exclude（排除）contains（包含）的内容
    # studentsList = Students.stuObj.exclude(sname__contains="美")
    #isnull,isnotnull:是否为null
    # studentsList =Students.stuObj.filter(sname__isnull=False)
    #startswith、endswith：以value开头或结尾，大小写敏感
    # studentsList = Students.stuObj.exclude(sname__endswith="美")
    #包含在范围内
    # studentsList = Students.stuObj.filter(pk__in = [1,3,6,10])
    #gt、gte、lte、lt
    #pk大于3
    # studentsList = Students.stuObj.filter(id__gt = 3)
    #过滤日期
    # studentsList = Students.stuObj.filter(createTime_date__gt = 2016)
    # 关联_one
    # studentsList = Students.stuObj.filter(sgrade__gname="python01")
    # 聚合函数
    #Max()函数
    # maxAge = Students.stuObj.aggregate(Max('sname'))
    # print(maxAge)
    # F对象
    # g = Grades.objects.filter(ggirlnum__lt=F('gboynum')*2)
    # print(g)
    # return render(request,"myApp/students.html",{'students':studentsList})

"""
```



