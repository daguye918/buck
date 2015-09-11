# python_binary()
该规则在未来可能会有所改变

一个 <code>python_binary()</code>规则是用来建立一个 [PEX](http://pantsbuild.github.io/pex_design.html) 文件 - 一个可执行的 Python 包，该包包括所有传递的<code>python_library</code>依赖中的 python 源和其他资源。

## 参数

+ name（必需）规则的名称。输出的 PEX 文件也将获得这个名字，另外还有<code> .pex</code> 后缀。
+ main（必需）作为此规则入口的<code>.py</code>源。当 PEX 文件运行时，这个源将被编译运行。
+ base_module（默认为无）为其主模块应该驻留在在它的二进制最终位置的包。如果不设置，即房屋的BUCK文件的项目相对目录使用。
+ deps（默认为[]）<code>python_library</code>规则，包括在 PEX 文件（包括所有的传递依赖）。
+ visibilty（默认为[]）[构建目标模式（build target patterns）](https://buckbuild.com/concept/build_target_pattern.html)的列表，该模式标识可以在本身的 <code>deps</code>中包含其他规则的编译规则。

## 例子
```
python_binary(
  name = 'tailer',
  main = 'tailer.py',
  deps = [
    ':tailerutils',
  ],
)

python_library(
  name = 'tailerutils',
  srcs = glob(['*.py'], excludes = ['tailer.py']),
)
```