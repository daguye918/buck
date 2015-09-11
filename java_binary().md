# java_binary()
java_binary()规则是被用来创建一个已经编译的 .class 文件的 JAR 文件 和 java_binary() 规则所依赖的资源文件。

## 参数

+ name(必需的) 规则的名称，也是生成的 JAR 文件的名称
+ mdeps(默认设为[]) 一系列规则（一般类型 java_library），应该被编译而且它的 .class 文件和源文件应该包含在生成的 JAR 文件中。
+ main_class (默认设为 None) 如果有，它的值就如在生成的JAR 文件中 META-INF?MANIFEST.MF 文件的 Main-Class 属性。当然，如果该规则被当作[genrule()](https://buckbuild.com/rule/genrule.html)里的可执行文件使用, main_class 会调用主方法中处理命令行参数（ command-line arguments）的类。这和使用 <code>java -jar &lt;name.jar>  &lt;args>  </code>效果一样。
+ mainfest_file(默认设为 None) 如果有，这个 mainfest 文件在生成 JAR 文件的时候会被使用到。如果和 main_class 配合，指定的 mainfest 文件会被使用，但是 main_class 会重写文件清单中的主类。
+ mate_inf_directory(默认设为 None) 注意：现在是测试版本。如果有，这个目录里的内容会包括到生成的 JAR 文件内部的 META_INF 目录中结束。
+ visibility(默认设为 []) [构建指定模式](https://buckbuild.com/concept/build_target_pattern.html)的列表，该模式能识别可以在 deps 中包括改规则的构建规则（build rules）。