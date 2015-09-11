# java_test()
java_test（）规则是用来定义包含通过JUnit来运行的测试一组 .java 文件。

## 参数
+ name(必需的) 规则的名称
+ src(默认设为 [])像 [java_library()](https://buckbuild.com/rule/java_library.html)当该规则构建好时，所有用 srcs 参数指定的 .java 文件都会被编译。另外,当该规则按测试来运行时，由该规则构建的所有相关的 .class 文件都会被当作参数传入 Juit。被传入到没有任何 @test 注解方法的 JUnit 测试中的 .class 文件会测试失败，所以确保只有测试案例的类被指定为 srcs . 这个目前通过指定 srcs 为 glob(['**/*Test.java']) 实现。
+ resources(默认设为[]) 和 [java_library](https://buckbuild.com/rule/java_library.html) 相同。
+  labels （默认设为 []）被应用到这些测试的标签的集合。这些标签是任意的文本字符串，在 buck 本身里没有任何意义。但是，他们对于一个测试者是有意义的（如 smoke 或 fast）。当执行 [buck 测试](https://buckbuild.com/command/test.html)时标签可用于过滤或包含特定<code>d_test（）</code>规则。
+  source_under_test （默认设置为 []）<code>java_library()</code> 规定 <code>java_test()</code> 是测试。这些都必须在 <code>java_test</code>的传递依赖中。如果 buck test 执行时带有 -code-converage 选项， source_under_test 指定规则的 .class 文件会通过代码覆盖工具（EMMA）进行检测。
+  source（默认设为 '6'）用来编译的 java 语言目标等级。对应于 javac 的 -source 参数。
+  target(默认为 '6')编译字节码的目标水平。对应于 javac 的 -target 参数。
+  deps(默认 [])和 java_library() 一样。必须包含有 JUnit 测试作为依赖（版本4.7或者之后的）
+  test_type(默认 junit)指定使用那个测试框架。目前可选的有 'testing' 和 'junit'
+ run_test_separately(默认 false) 如果设为 true，这条规则下的测试会和其他测试分开运行（对于物理设备或者其他有限资源的集成测试是有帮助的）
+ std_out_log_level(默认为FINE)在 buck 会输出到 std out 域的测试下的资源消息的日志级别。
	值必须是一个 java.util.logging.Level 有效的值。
+ std_err_log_level（默认设为iWARNING）和 std_out_log_level 一样，不过是对于 std err。
+ visibility(默认设为 []) [构建指定模式](https://buckbuild.com/concept/build_target_pattern.html)的列表，该模式能识别可以在 deps 中包括改规则的构建规则（build rules）。