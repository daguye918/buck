# d_test()
这个在以后可能会有所变化

一个 d_test() 规则被用来定义 D 源文件集合，这些源文件中包含了要用来运行的测试，通过 D 的工具测试支持而实现。测试的源代码必须包含一个主 （main()）函数。


##  参数
+ name(必需的) 规则（rule） 的名称
+ srcs(必需的) 由该规则（rule）编译构成的 D 源文件集合。每个元素都应该是特定指向一个源文件的一个字符串（例如：'foo/bar.d'）
+ labels （默认设为 []）被应用到这些测试的标签的集合。这些标签是任意的文本字符串，在 buck 本身里没有任何意义。但是，他们对于一个测试者是有意义的（如 smoke 或 fast）。当执行 buck 测试时标签可用于过滤或包含特定<code>d_test（）</code>规则。
+ source_under_test （默认设置为 []）<code>d_library()</code> 规定 <code>d_test()</code> 是测试。这些都必须在 <code>d_test</code>的传递依赖中。
+ + deps(默认 []) 改规则（rule）依赖文件的集合。每个元素都应该指向一个在别处定义的 d_library 规则的字符串。

## 例子

```
# A rule that builds and runs D test with a single source file.
d_test(
  name = 'test',
  srcs = [
    'test.d',
  ],
)
```