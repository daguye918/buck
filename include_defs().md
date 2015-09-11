# include_defs()
<code>include_defs()</code>函数是用来包括另一个文件中的[宏(macros)](https://buckbuild.com/extending/macros.html)和常量。
<code>include_defs()</code>函数在当前编译文件上下文中执行的构建文件式（build-file-style）的代码。因此，包含在文件中的代码可以调用 Buck 函数，如<code>java_library()</code>，<code>java_test()</code>等，以及<code>include_defs()</code>本身！

include_defs()是为了避免在多个版本的文件复制和粘贴相同代码。通常情况下，包含的文件将包含数据的声明（如下面的例子），或为创建更复杂的生成规则定义的宏定义。

## 参数

+ 第一个也是唯一一个参数是一个包含 [macrod](https://buckbuild.com/extending/macros.html) 和常量的文件（各种）的 path 路径，。它类似于一个构建的目标，因为它以 // （表示项目的根目录）开始，而不是一个适当的构建目标，因为它相对于项目根确定了文件，而不是一个生成规则。


## 示例

假设该文件的 core/DEFS 包含以下内容：
```
JARS_TO_EXCLUDE_FROM_DX = [
  'third_party/guava/guava-14.0.1.jar',
  'third_party/jackson/jackson-core-2.0.5.jar',
  'third_party/jackson/jackson-databind-2.0.5.jar',
  'third_party/jackson/jackson-datatype-guava-2.0.4.jar',
]
```

然后另一个编译文件能够包含使用 <code>include_defs()</code>的数组。这消除了需要在整个构建文件复制并粘贴定义：
```
include_defs('//core/DEFS')

android_binary(
  name = 'example',
  # ...
  no_dx = JARS_TO_EXCLUDE_FROM_DX,
)
```
