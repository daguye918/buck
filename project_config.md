# project_config()

该规则以后可能会有所变化。

project_config() 规则用于声明生成 IDE 配置文件所需要的信息，所以 BUCK 文件的主要构建目标（以及其测试文件）可以使用相应 IDE 来构建。

注意，每个 BUCK 文件中只有一个 project_config() 规则。

## 参数

- src_target（必要参数）- 一种构建规则，该规则将是生成项目的基础。目前，它必须是下列之一：java_library()，java_test()，android_library() 或者 android_binary()。生成的 IDE 项目类型将会对应规则的类型。<br>
&nbsp;同样地，该规则的 deps 将会决定该规则对应的其他 IDE 项目。注意，这是启发式的，但是在实际中它运行得相当好。
- src_roots（默认为 []）- src_target 对应的源代码的根目录。其值可能包括：
	- None 表明没有根目录。当 src_target 标识的是一个纯资源并且没有 Java 代码的 Android 库项目的时候就会出现这种情况。
	- [] 包含构建文件的目录是一个 Java 包。这种情况下，包的根目录必须是其祖先目录之一。Buck 可以借助 .buckconfig 文件中 [java] 部分的 src_roots 属性推断出包的根路径位置。
	- ['src'] build 文件目录下的 src 目录是源文件的根目录。这个列表中可能含有多个元素，但是实际上，它最多只包含一个元素。此选项支持列表，以允许生成的源代码可以和引入的源码一起检查。在这种情况下，src_roots 将会是 ['src', 'src-gen']。
- test_target（默认为 None）- 如果声明了，就会为 src_target 生成一个配套的测试项目。在 Intelli J 里，源代码和测试代码可以放在同一个模块中，但是有着不同的路径。在 Eclipse 里，源代码和测试代码通常放在分离的项目中以使它们有着不同的路径。
- test_roots（默认为 []）- 和 src_roots 类似，但是该参数对应 test_target。

## 示例

~~~

project_config(
  src_target = ':lib-base',
  src_roots = [ 'src' ],
  test_target = ':tests',
  test_root = 'tests',
)

~~~