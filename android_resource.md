# android_resource()

android_resource() 规则用于存放在 res 以及 assets 目录下的 Android 资源绑定。

android_resource() 输出的是通过 aapt 生成的一个 R.txt 文件 --output-text-symbols。

## 参数

- name（必要参数）- 规则的名称。
- res（默认为 None）- 包含 Android 资源的目录路径。
- package（默认为 None）- Java 包，用于存放根据资源生成的 R.java 文件等。
- assets（默认为 None）- 包含 Android assets 文件的目录路径。
- deps （默认为 None）- 其他 android_resource 规则，包括在运行 aapt 时使用 -s 命令。
- visibility（默认为 None）- [构建目标样式](https://buckbuild.com/concept/build_target_pattern.html)，用于标识能够包含 deps 内规则的构建规则。

## 示例

大多数时候，android_resource 规则只定义了 name,res 以及 package。为了方便，我们通常将几个规则命名为 res：

~~~

android_resource(
  name = 'res',
  res = 'res',
  package = 'com.example',
)

~~~ 