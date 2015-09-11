# prebuilt_python_library()
改规则将来可能改变。

<code>prebuilt_python_library()</code> 规则用于将预建的 Python 包含进一个顶级 <code>python_binary</code>或<code>python_test</code>规则的输出。要为包创建一个 egg 包装，运行<code>Python setup.py bdist_egg</code>。

## 参数

+ name（必需）规则的名称。
+ binary_src（必需）要使用的.egg 文件的路径。注：.egg 文件也规定了必须遵循一个非常特殊的命名约定 - 否则将 pex 在运行时找不到正确的依赖！
+ deps（默认为[]）这个库依赖的其它 <code>prebuilt_python_library()</code>规则。如果你想依赖于该库的基于源代码的副本，这也可以是<code>python_library</code>的规则。
+ visibilty（默认为[]）[构建目标模式（build target patterns）](https://buckbuild.com/concept/build_target_pattern.html)的列表，该模式标识可以在本身的 <code>deps</code>中包含其他规则的编译规则。

## 例子
```
# A simple prebuilt_python_library with no external dependencies.
prebuilt_python_library(
  name = 'requests',
  binary_src = 'requests-2.7.0-py2.7.egg',
)

# A slightly more complex example
prebuilt_python_library(
  name = 'greenlet',
  binary_src = 'greenlet-0.4.7-py2.7-macosx-10.10-x86_64.egg',
)

prebuilt_python_library(
  name = 'gevent',
  binary_src = 'gevent-1.0.2-py2.7-macosx-10.10-x86_64.egg',
  deps = [
    ':greenlet',
  ],
)

```