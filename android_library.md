# Android

## android_library()

android_library() 规则用于定义一组可以和 Android SDK 一起编译的 Java 文件。android_library() 规则的主要产物是一个包含了所有编译后的 class 文件以及相关资源的 JAR 文件。

### 参数

- name（必须的参数）- 该规则的名称。
- srcs （默认为 []）- 该规则需要编译的 .java 文件集
- resources （默认为 []）- 包含在编译后的 .class 文件之间的静态文件<br>
  这些文件可以通过 [Class.getResource()](http://docs.oracle.com/javase/7/docs/api/java/lang/Class.html#getResource(java.lang.String)) 方法加载<br>
  **注意**：Buck 使用 .buckconfig 文件中的 src_root 属性来帮助你确定相应资源在生成的 JAR 文件中的位置。
- manifest（默认为 None）- 这是一个可选参数，用于指明 [Android Manifest](http://developer.android.com/guide/topics/manifest/manifest-intro.html) 文件。该文件可以声明资源库中需要的权限或者是 intent。该参数可以是一个文件或者是 [android_manifest](https://buckbuild.com/rule/android_manifest.html) 目标。
- deps（默认为 []）- 编译当前 android_library 所需要的类路径的生成规则（通常是其他的 android_library 规则）
- source（默认为 <global value>）- 声明了解释源文件所用的 Java 版本（以字符串的形式声明），可以在 [.buckconfig](https://buckbuild.com/concept/buckconfig.html) 里 "java" 片段中的 "source level" 处重写该值。
- target（默认为 <global value>）- 声明了用于生成代码的 Java 版本（以字符串的形式声明）,在 [.buckconfig](https://buckbuild.com/concept/buckconfig.html) 里 "java" 片段中的 "target_level" 处可以重写该值。
- javac（默认为 <global value>）- 声明了对应当前规则的 Java 编译器。该值是一个源文件的路径（比如，给定的规则只可能设置 "javac" 以及 "javac_jar" 中的一个），在 [.buckconfig](https://buckbuild.com/concept/buckconfig.html) 里 "tools" 片段中的 "javac" 处可以重写该值。
- javac_jar（默认为 <global value>）- 声明了该规则所使用的 Java 编译器。该值是一个源文件的路径（比如，给定的规则只可能设置 "javac" 以及 "javac_jar" 中的一个），在 [.buckconfig](https://buckbuild.com/concept/buckconfig.html) 里 "tools" 片段中的 "javac_jar" 处可以重写该值。
- extra_arguments（默认为 []）- 传递给 Java 编译器的附加参数列表。这些参数会跟在 [.buckconfig](https://buckbuild.com/concept/buckconfig.html) 中声明的参数之后。
- exported_deps（默认为 []）- 这和在 [java_library](https://buckbuild.com/rule/java_library.html) 中时是一样的。
- visibility（默认为 []）- 是 [build target patterns](https://buckbuild.com/concept/build_target_pattern.html) 列表，用于标识能够包含 deps 内规则的构建规则。

## 示例

android_library 规则通常与 [android_resource](https://buckbuild.com/rule/android_resource.html) 规则一起使用。在 [http://developer.android.com/tools/projects/index.html](http://developer.android.com/tools/projects/index.html) 中将这种方式定为 Android Library 项目的标准配置方式。

~~~

android_resource(
  name = 'res',
  res = 'res',
  package = 'com.example',
)

android_library(
  name = 'my_library',
  srcs = glob(['src/**/*.java']),
  deps = [
    ':res',
  ],
)

~~~