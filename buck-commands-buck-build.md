#  构建 buck

构建一个或多个指定的目标。

buck 作为一个构建系统,这是最常用的命令。要建立一个规则,通过它的目标构建 buck:

```
buck build //java/com/example/app:amazing
```

请注意，如果你在 `.buckconfig` 里的 `[download]` 部分不设置 `in_build = true` ，你建立规则所引用的任何 `[remote_file][1]` 规则，都必须在调用该命令之前下载 `[buck fetch][2]` 。

## 参数
- `--build-report` 指定一个文件，写入有关建立输出的JSON总结。请注意，在没有  `--keep-going` 的情况下，也可以用  `--build-report` 。如果 `--keep-going` 没有被指定，报告只会在所有的目标成功建立之后写入。下面有个建立报告的例子：

```
{
  "success": false,
  "results": {
    "//fake:rule1": {
      "success": true,
      "type": "BUILT_LOCALLY",
      "output": "buck-out/gen/fake/rule1.txt"
    },
    "//fake:rule2": {
      "success": false
    },
    "//fake:rule3": {
      "success": true,
      "type": "FETCHED_FROM_CACHE"
    }
  }
}
```

在这个例子里， `//fake:rule1` 和  `//fake:rule3` 被成功建立，但是只有 `//fake:rule1` 有一个与规则关联的输出文件。

注意,这个包含与 `--keep-going` 打印到控制台相同的信息, 但这样更容易解析编程。这份报告在未来可能包含多个字段。

- `--keep-going` 当指定时, Buck 将尝试构建命令行上指定的所有目标,即使一些目标失败。( Buck 的默认行为是一旦有指定目标失败，立即退出。）

当 `--keep-going` 被指定，一份有关构建的报告将会输出到stderr,详细说明每个目标的构建状态。报告的每一行代表一个构建目标的状态。取决于如下这四条:

1. `OK` 或 `FAIL` ,根据构建的成功法则。
2. 构建规则的目标。
3. 如果成功,成功的类型定义为 `com.facebook.buck.rules.BuildRuleSuccess.Type`。
4. 如果成功且规则的输出文件路径存在，输出。

例如,如果 buck 运行使用以下参数:
```
buck build --keep-going //fake:rule1 //fake:rule2 //fake:rule3
```

打印在 stderr 上的报告是这样的：
```
OK   //fake:rule1 BUILT_LOCALLY buck-out/gen/fake/rule1.txt
FAIL //fake:rule2
OK   //fake:rule3 FETCHED_FROM_CACHE
```

这个例子中 `//fake:rule1` 和 `//fake:rule3` 构建成功， `//fake:rule1` 有一个与规则有关的输出文件。诚然,第1列的状态可能来自第3列的存在,但第1列的编码更易于从失败的规则当中过滤出成功的规则。

请注意，当选定 `--keep-going` ，如果所有目标构建成功，退出代码将是0。

这个选项类似于 Make 中的 `-k/--keep-going` 。

- `--num-threads` 在执行构建时， buck 应该使用的线程数。这个默认为系统中处理器的数量的1.25倍(在有超线程的系统中,这意味着每个核心计算两次)。在执行构建时应该使用的线程数也可以通过在 `.buckconfig` 文件的" `build` "部分中添加" `threads` " 键来设置。设置构建线程的数量的优先顺序(从最高到最低)是:命令行选项， `.buckconfig` 设置,默认值。活动线程的数量可能不总是等于这个参数。

- `--verbose (-v)` 应该多详细得记录日志到控制台，以1为最低详细程度，10表示最详细的。


  [1]: https://buckbuild.com/rule/remote_file.html
  [2]: https://buckbuild.com/command/fetch.html