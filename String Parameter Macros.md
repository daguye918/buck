# String Parameter Macros
在将来可能改变。

一些规则允许在其参数的字符串嵌入指定宏，提供额外的功能，诸如引用其他规则的输出路径。字符串参数宏的形式为<code> $(macro[ argument])</code>。对于支持字符串参数宏的规则，你可以在任何时候使用一个反斜杠，以防止膨胀（如<code>\$(dirname ...)</code>）。

## 参数

+ 字符串参数宏可能需要一个参数。

## 示例

下面的示例在 <code>genrule()</code>使用一个字符串参数中的宏来复制另一个规则的输出：
```
genrule(
  name = 'gen',
  out = 'out.txt',
  cmd = 'cp $(location //some/other:rule) $OUT',
)
```

下面的示例使用一个反斜杠直接防止宏展开，并将<code>dirname</code>名称的子shell命令传给 shell 的：
```
genrule(
  name = 'gen',
  out = 'out.txt',
  cmd = 'cp $SRCS \$(dirname $OUT)',
  srcs = [
    'test1.txt',
    'test2.txt',
  ],
)
```