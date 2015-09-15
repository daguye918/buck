# 审查 buck

提供有关构建目标和输出的构建信息。

```
buck audit input //java/com/example/app:amazing
```

## 命令集

- `alias --list` 列出 .buckconfig 或 .buckconfig.local中声明的 alias 集合。（目前， --list 是被 buck audit alias 支持的唯一标记，但是我们在想其他方法） 
- `classpath <targets>` 列出用于构建给定目标的classpath，但这并不是对于所有建立规则类型都适用的。
- `dependencies <targets>` 列出用于构建给定目标的依赖。结果按字母顺序列出。默认情况下,只有直接依赖被列出。用 `--transitive` 标记，可显示传递性的依赖。用  `--include-tests` 标记，可显示规则的测试结果。这个打印出一个规则的测试结果，就好像他们是规则的依赖。结合上 `--transitive` 标记，会打印出测试结果的所有依赖。
- `input <targets>` 列出用于构建给定目标的输入源和资源文件。
- `tests <targets>` 为给定目标列出测试结果。结果按字母顺序列出。只有给定目标产生的测试结果才被打印出，即使会提供多个目标。这个命令需要和 `audit dependencies` 结合使用。例如,要检索给定项目的所有测试列表,使用

```
buck audit dependencies --transitive PROJECT | xargs buck audit tests
```

## 参数
- `--json` 以 JSON 格式输出结果。

## 例子

```
# For all of the following examples, assume this BUCK file exists in
# the `examples` directory.
java_library(
  name = 'one',
  srcs = [ '1.txt' ],
  deps = [
    ':two',
    ':three',
  ],
)

java_library(
  name = 'two',
  srcs = [ '2.txt' ],
  deps = [
    ':four',
  ],
)

java_library(
  name = 'three',
  srcs = [ '3.txt' ],
  deps = [
    ':four',
    ':five',
  ],
)

java_library(
  name = 'four',
  srcs = [ '4.txt' ],
  deps = [
    ':five',
  ]
)

java_library(
  name = 'five',
  srcs = [ '5.txt' ],
)
```

列出用于构建 one 库的所有源文件。

```
buck audit input //examples:one
```

```
examples/1.txt
examples/2.txt
examples/3.txt
examples/4.txt
examples/5.txt
```

输出用于构建 two 库所有源文件的 JSON 表示。在此JSON对象中,每个键是构建目标，每个值是用于构建规则的源路径组成的数组。

```
buck audit input --json //examples:two
```

```
{
  "//examples:two": ["examples/2.txt"],
  "//examples:four": ["examples/4.txt"],
  "//examples:five": ["examples/5.txt"],
}
```

列出 one 库中所有直接依赖的规则。

```
buck audit dependencies //examples:one
```

```
//examples:three
//examples:two
```

列出 one 库中所有传递依赖的规则。

```
buck audit dependencies --transitive //examples:one
```

```
//examples:five
//examples:four
//examples:three
//examples:two
```

输出 two 库中所有传递依赖规则的 JSON 表示。

```
buck audit dependencies --transitive --json //examples:two
```

```
{
  "//examples:two": ["//examples:five","//examples:four"]
}
```

输出 two 库和 three 库中所有直接依赖关系的 JSON 表示。

```
buck audit dependencies --json //examples:two //examples:three
```

```
{
  "//examples:three": ["//examples:five","//examples:four"],
  "//examples:two": ["//examples:four"]
}
```






