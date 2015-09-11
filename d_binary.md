#  d_binary()
这个以后可能有所改变

d_binary() 规则 （rule）可以从可供使用的 D 源文件和其依赖的文件构成的集合来编译构建一个原生可执行文件。

## 参数

+ name(必需的) 规则（rule） 的名称
+ srcs(必需的) 由该规则（rule）编译构成的 D 源文件集合。每个元素都应该是特定指向一个源文件的一个字符串（例如：'foo/bar.d'）
+ deps(默认 []) 改规则（rule）依赖文件的集合。每个元素都应该指向一个在别处定义的 d_library 规则的字符串。


##  例子

```
# A rule that builds a D native executable from a single .d file
# and a library dependency.
d_binary(
  name='greet',
  srcs=[
    'greet.d',
  ],
  deps=[
    ':greeting',
  ],
)

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

