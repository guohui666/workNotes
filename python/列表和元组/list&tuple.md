#### 基本操作

```python
#  合并
print(["a","b","c"] + ["d","e","f"]) # ['a', 'b', 'c', 'd', 'e', 'f']
#  重复
print(["a","b","c"] * 3) # ['a', 'b', 'c', 'a', 'b', 'c', 'a', 'b', 'c']
```

#### tuple中只有一个元素时需要在元素后加","标识是一个tuple类型，否则会与数学运算符冲突，python默认以数学运算符优先
```python
type((1)) # <class 'int'>
type((1,)) # <class 'tuple'>
```
#### 判断元素是否在序列中
```python
# 使用`in`运算符可以方便的判断一个元素是否存在于序列中(Str,Tuple,List)，返回bool
print([1,2] in [[1,2],[2],[3]]) # True
print([1,1] in [[1,2],[2],[3]]) # False
print("a" in "abcde") # True
print("a" in ("a","b","c")) # True
```
#### 元组(tuple)与列表(list)区别