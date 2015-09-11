# java_library()

java_library() 规则被用来定义可以联合编译的 java 文件的集合。 java_library() 规则的主要输出是一个包含所有编译完成的 class 文件和源文件的 单个 JAR 文件。

## 参数

+ name(必需的) 规则的名称
+ src(默认设为 []) 由该规则编译的 .java 文件的集合。若在列表中有任何文件名以 .src.zip 结尾，那么 ZIP 文件中的 .java 文件会被当作普通输入项包括进去以完成编译。当使用一个 <code>genrule</code>来自动生成一些手写java代码中编译需要的 java 代码。
+ resources （默认设为 []）包含在编译生成的 .class 文件中的静态文件。这些文件不能通过[ Class.getResource()](http://docs.oracle.com/javase/7/docs/api/java/lang/Class.html#getResource(java.lang.String)) 方法加载。
	**注意：**如果 resources_root 没有被设定，Buck 会在 .buckconfig 中使用 src_roots 属性来帮助决定资源放置在生成的 JAR 文件中的哪里。
+ deps (默认 设为[])被用来生成编译这个 java_library 所需的 classpath 的规则（一般指 其他 java_library 规则）。
+ source（默认设为 &lt;global value>）指定编译源文件的 java 的版本（用一个字符串）。重写了.buckconfig 文件中 "java"下的  “source_level” 对应值。
+ target（默认设为 &lt;global value>） 指定用来生成代码的 java 的版本（用一个字符串）。重写了.buckconfig 文件中 "java"下的  “target_level” 对应值。
+ java_version(默认设为 &lt;global value>)相当于同时用给定值设定 source 和 target 的值。当设置这个后，在设置 source target 中任何一个都会产生错误。
+ javac （默认设为 &lt;global value>） 指定Java编译器程序使用此规则。值是源路径（例如：只有 "javac" 和 “javac_jar” 中的一个能被设置成指定的规则。）重写 .buckconfig 文件中 “tools” 部分 "javac"对应的值。
+ javac_jar(默认设为 &lt;global value>)指定Java编译器程序使用此规则。值是源路径（例如：只有 "javac" 和 “javac_jar” 中的一个能被设置成指定的规则。）重写 .buckconfig 文件中 “tools” 部分 "javac_jar"对应的值。
+ extra_arguments(默认设为 [])传入 java 编译器多余参数的列表。这些参数参照 .buckconfig 文件中的指定项。
+ exported_deps(默认设为 []) 其他依赖这个规则的 java_library 规则也会在他们的类加载路径包含它的 exported_deps。当一条规则的公共 API 返回在其他规则中定义的类型，或者检查其他规则中定义的异常，这也是很有用的，不然还需要用来增加多余依赖的调用。对于传递一个由prebuilt_jar 规则构成的集合来作为调用的依赖也是很有帮助的。在 exported_deps 中的目标都隐式包含在这条跪着的 deps 中，所以他们并不需要重复出现。
+ provide_deps（默认设为 []）这些代表相关依赖。这些相关依赖是在运行时被提供的，也是编译代码所需要的。provided_deps 的实例包括 JEE 的 servlet API。当该规则被包括在java_binary ， provided_deps不会被打包成输出。
+ visibility(默认设为 []) [构建目标模式（build target patterns）](https://buckbuild.com/concept/build_target_pattern.html)的列表，标识能在它的 deps 中包括这条规则编译规则。

## 例子
```
# A rule that compiles a single .java file.
java_library(
  name = 'JsonUtil',
  srcs = ['JsonUtil.java'],
  deps = [
    '//third_party/guava:guava',
    '//third_party/jackson:jackson',
  ],
)

# A rule that compiles all of the .java files under the directory in
# which the rule is defined using glob(). It also excludes an
# individual file that may have additional dependencies, so it is
# compiled by a separate rule.
java_library(
  name = 'messenger',
  srcs = glob(['**/*.java'], excludes = ['MessengerModule.java']),
  deps = [
    '//src/com/facebook/base:base',
    '//third_party/guava:guava',
  ],
)

java_library(
  name = 'MessengerModule',
  srcs = ['MessengerModule.java'],
  deps = [
    '//src/com/facebook/base:base',
    '//src/com/google/inject:inject',
    '//third_party/guava:guava',
    '//third_party/jsr-330:jsr-330',
  ],
)

# A rule that builds a library with both relative and
# fully-qualified deps.
java_library(
  name = 'testutil',
  srcs = glob(['tests/**/*.java'], excludes = 'tests/**/*Test.java'),
  deps = [
    ':lib-fb4a',
    '//java/com/facebook/base:base',
  ],
)
```
