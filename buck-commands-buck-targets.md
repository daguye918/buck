# buck 目标
在当前项目中列出可用的构建目标。

以下代码会打印出项目中所有构建目标（按字母顺序排列）。

```
buck targets
```

这个命令可以很方便的完成编程任务，如运行所有的 Java 测试，在 `//java/com/myproject`路径下：

```
buck targets --type java_test | \
  grep '//java/com/myproject' | \
  xargs buck test
```

一系列规则也可以被传递进 `buck targets` ，并且 Buck 将只打印出这些规则的目标信息。例如：

```
buck targets --show-output //java/com/myproject:binary
> //java/com/myproject:binary buck-out/gen/java/com/myproject/binary.apk
```

## 参数

- `--type ` 目标类型的筛选，例如

```
buck targets --type java_test java_binary
```

- `--referenced-file` 通过一系列规则过滤文件，这些规则包括在其传递闭包中的 `referenced-file `。

例如，如果开发人员想运行所有可能受特定文件影响的测试，请执行：

```
buck targets --type java_test \
  --referenced-file java/com/example/Foo.java |
  xargs buck test
```

- `--json` 输出每个目标的JSON 表示。

此外， JSON 包括每个目标的 'direct_dependencies' ，其中对于实现 ImplicitDepsInferringDescription 的目标，可能包括附加依赖。它给出了目标的完全限定名称。

例如，当解决一个 genrule ，直接依赖包括在 'deps' 中构建目标，以及在任何与 genrule 关联脚本中建立目标。

- `--print0` 使用 ASCII NUL 字符划定目标（当不指定 `--json` ）。这有利于使用 `xargs` 。

```
buck targets --print0 | xargs -0 buck build
```

- `--resolve-alias` 打印指定 alias[es] 中完全合格的构建目标。这个命令还接受普通构建目标。通过 [`.buckconfig`][1] 了解更多详情。

- `--show-output` 为每一条规则的名称后面输出相对路径。


  [1]: https://buckbuild.com/concept/buckconfig.html