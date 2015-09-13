# prebuilt_jar()

prebuilt_jar 规则用于标识 JAR 文件，该文件是作为预编译的二进制文件签入我们的库中的，而不是通过 Buck 源来构建的。通常，这些在引用第三方 JAR 文件的时候才使用（比如 junit.jar）并且我们会将其作为 java_library() 规则的依赖。

## 参数

- name（必要参数）- 规则的名称。
- binary_jar（必要参数）- 预编译 JAR 文件的路径。
- source_jar（默认为 None）- JAR 文件的路径，包含了 binary_jar 中用于生成 .class 文件的 .java 文件。这个参数主要用于调戏。
- javadoc_url（默认为 None）- binary_jar 中相应 .class 文件的 Javadoc。
- deps（默认为 []）- 在该规则构建前应当构建好的规则。因为 binary_jar 是已经编译好的，所以应该没有其他需要构建的，故这个参数应当为空。
- visibility（默认为 []）- [构建目标样式](https://buckbuild.com/concept/build_target_pattern.html)列表，用于标识能够包含 deps 内规则的构建规则。

## 示例

~~~

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

~~~