#  prebuilt_jar()
prebuilt_jar() 规则是用来标识作为预编译的二进制文件签入我们的资源库的 JAR 文件，而不是通过 Buck 由源代码编译生成的 JAR 文件。通常，这些用于引用第三方 JAR文件（如的junit.jar ），也用作<code> java_library() </code>规则的依赖。


## 参数

+ 名字（必需的）规则的名称。
+ binary_jar（必需）预编译 JAR 文件的路径。
+ source_jar（默认为 None）JAR 文件（包含用于在 binary_jar 中构建 <code>.class</code> 的 <code>.java </code>文件）的路径。这通常提供用于调试的。
+ javadoc_url（默认为 None）在 <code>binary_jar</code> 中<code>.class</code>文件的 java文档的 URL 路径。
+ DEPS（默认为[]）必须在此规则之前建成的规则。由于binary_jar 已经完成编译，所以没有什么还要进行编译的，所以这应该是空的。[
+ visibility（默认为[]）构建目标模式规则（build target patterns）](https://buckbuild.com/concept/build_target_pattern.html)的列表，可以标识那些能在自己的<code> deps</code> 中包含这条规则的编译规则。

## 例子

```

prebuilt_jar(
  name = 'junit',
  binary_jar = 'junit-4.8.2.jar',
  source_jar = 'junit-4.8.2-sources.jar',
  javadoc_url = 'http://kentbeck.github.com/junit/javadoc/4.8/',
)

java_library(
  name = 'tests',
  srcs = glob(['tests/**/*Test.java']),
  deps = [
    ':junit',
  ],
)
```