# prebuilt_native_library()

prebuilt_native_library() 规则用于绑定 Android 本地库（比如 so 库）。

## 参数

- name（必要参数）- 规则的名称。
- native_libs（默认为 None）- 包含本地库的目录路径。该目录有一些针对不同结构的子目录，比兔 armeabi 以及 armeabi-v7a。
- is_asset（默认为 False）- 通常本地共用对象最后会存储在 APK 根目录的 lib/ 文件夹下。如果这个参数设置为 True，呢么这些对象将会放到 assets/lib/ 下。将共用对象放到一个非标准的位置会阻止 Android 将其解压到设备内部储存中。
- visibility（默认为 []）- [构建目标样式](https://buckbuild.com/concept/build_target_pattern.html)列表，用于标识能够包含 deps 内规则的构建规则。

## 示例

大多数情况下，prebuilt_native_library 对 [android_library](https://buckbuild.com/rule/android_library.html) 是私有的：

~~~

prebuilt_native_library(
  name = 'native_libs',
  native_libs = 'libs',
)

android_library(
  name = 'my_lib',
  srcs = glob(['*.java']),
  deps = [
    ':native_libs',
  ],
)

~~~