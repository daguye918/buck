# android_prebuilt_aar()

android_prebuilt_aar() 规则需要一个 .aar 文件并且让其可以作为 Android 依赖来使用。不出所料，依赖于 android_prebuilt_aar() 的 android_binary 会将其内容包含在生成的 APK 中。

关于 .aar 文件的细节请参见 [official Android documentation](http://tools.android.com/tech-docs/new-build-system/aar-format)。

## 参数

- name（必要参数）- 规则的名称。
- arr（必要参数）- .arr 文件的路径。这也可以是一个输出 .arr 文件的规则的构建目标。（比如 genrule）
- visibility（默认为 []）- [构建目标样式](https://buckbuild.com/concept/build_target_pattern.html)的列表，用于标识能够包含 deps 内规则的构建规则。

## 示例

~~~

android_prebuilt_aar(
  name = 'play-services',
  aar = 'play-services-4.0.30.aar',
)

android_library(
  name = 'lib',
  # This Java code can compile against Play services and reference its resources.
  srcs = glob(['*.java']),
  deps = [ ':play-services' ],
)

~~~