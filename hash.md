### 语法

```
hash(object)
```

### 作用

用于获取取一个对象（字符串或者数值等）的哈希值

### 参数

* object:对象

### 实例

```
>>> hash(str([1,2,3])) # 集合
-1147547787461630827

>>> hash('test') # 字符串
8622230414623538404

>>> hash(str(sorted({'1':1}))) # 字典
-124731967285827641
```



