# d_library()
这在以后可能会有所变化

一个 d_library() 规则代表 D 源文件的集合。

## 参数
+ name(必需的) 规则（rule） 的名称
+ srcs(必需的) 由该规则（rule）编译构成的 D 源文件集合。每个元素都应该是特定指向一个源文件的一个字符串（例如：'foo/bar.d'）
+ deps(默认 []) 改规则（rule）依赖文件的集合。每个元素都应该指向一个在别处定义的 d_library 规则的字符串。

##  例子

```
# A simple library with a single source file and a single dependency.
d_library(
  name='greeting',
  srcs=[
    'greeting.d',
  ],
  deps=[
    ':join',
  ],
)

d_library(
  name='join',
  srcs=[
    'join.d',
  ],
)
```