# buck 测试

构建并运行含有一个或多个指定目标的测试:

```
buck test //javatests/com/example:tests
```

你可以直接指定测试目标,也可以通过其他任何包含 `tests = ['...']` 字段的目标来指定测试。

## 参数

- `--all` 运行树中的所有可用测试。如果没有指定目标,这是默认选项。

- `--code-coverage` 运行测试时,收集代码覆盖信息。目前,这只适用于 Java 应用 [JaCoCo][1] 。运行后:

```
buck test --code-coverage
```

代码覆盖信息在以下目录中：

```
buck-out/gen/jacoco/code-coverage/
```

- `--debug` 如果指定,那么测试将开始暂停JDWP调试端口5005。这意味着他们将不会运行,直到调试器连接。

- `--include` 用测试标签来运行这个测试。标签是一种对特定类型进行分组的方法，并将他们一起运行起来。例如,开发人员可以用`快速`标签标记所有的运行不到100毫秒的测试,然后使用:

```
buck test --all --include fast
```

若想只运行快速标签测试，阅读 [`java_test()`][2] 来了解更多。

使用多个参数可匹配任何标签,用 `+` 匹配一组标签。例如匹配所有稳定或值得信赖的的快速测试,不匹配不稳定的:

```
… --include fast+stable fast+trustworthy --exclude fast+unstable
```

- `--exclude ` 该参数正好与 `include` 相反。 被标签标记为 exclude 选项的，不会被运行。 例如，如果我们想运行除慢测试以外的测试，我们可以执行：

```
buck test --all --exclude slow
```

- `--test-selectors (-filter)` 选择要运行测试的名字,使用 `class#method` 语句。所有其他测试将不会运行并且测试结果缓存被禁用:

```
buck test --all --test-selectors 'com.example.MyTest#testX'
```

匹配是通过使用 `java.util.regex` 正则表达式和类或方法中一部分被省略的部分来匹配所有的类或方法。选择器被固定在类或者方法名的末尾。（例如：将 $ 放在正则表达式末尾进行暗示 ）

```
buck test --all --filter 'Foo.*'  # ...every class starting Foo
```

```
buck test --all --filter '#testX' # ...run testX in every class
```

你可以用 `！` 进行排除测试，如果你所有的测试选择器是互斥的，默认会运行除带 ！ 以外的所有测试：

```
buck test --all --test-selectors '!MyTest'  # ...all except MyTest
```

测试选择器也可以从格式化为 `@/path/to/fiwle` 的路径下阅读文件。文件应该每行包含一个测试选择。

第一个匹配选择器决定是包含测试还是排除测试。完整的逻辑描述在 `--help` 中。

- `--dry-run` 打印与命令行参数，如 `--include` 和 `--exclude` 匹配的规则。但是会在构建或运行测试之前退出。

- `--num-threads` 在执行构建时， buck 应该使用的线程数。这个默认为系统中处理器的数量的1.25倍(在有超线程的系统中,这意味着每个核心计算两次)。活动线程的数量可能不总是等于这个参数。

- `--ignore-when-dependencies-fail` 如果一个库被中断，测试可能失败。如果另一个库依赖于这个库且其测试也失败,这可能是因为依赖中有一个bug。
例如,如果 `HouseBuilder` 库 依赖于 `Bricks` 库，且 `Bricks` 库被破坏，,这可能会导致两个库的测试都失败。

因此,如果库分别测试 `HouseBuilderTest` 和 `BricksTest` ,然后两个测试都失败，但只有 `BricksTest` 打印出错误;而 `HouseBuilderTest` 的错误被忽略。

你仍被通知 `HouseBuilderTest` 有错误,不选此选项并再次运行测试，将显示缓存的测试结果(和错误)。

- `--verbose (-v)` 应该多详细得记录日志到控制台，以1为最低详细程度，10表示最详细的。
- `--xml` 如果选中， Buck 将把测试结果写在本地的指定位置，例如：

```
buck test --all --xml testOutput.xml
```


  [1]: http://www.eclemma.org/jacoco/
  [2]: https://buckbuild.com/rule/java_test.html