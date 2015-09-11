# prebuilt_native_library()
<code>prebuilt_native_library()</code>规则是用来为 Android 捆绑本机库（即.so文件）。


## 参数

+ name（必需）规则的名称。
+ native_libs（默认为 None）包含本机库目录的路径。该目录的子目录有不同的结构，如<code>armeabi</code>和<code>armeabi-V7A</code>。
+ is_asset（默认为 false）通常本地共享对象最终在 APK 名为 <code>lib/</code> 的根目录下。如果这个参数设置为 true，则这些对象被置于 <code>asset/lib/</code> 下。将共享对象放在非标准位置下会 阻止 Android 将他们提取到本机内部存储。
+ visibilty（默认为[]）[构建目标模式（build target patterns）](https://buckbuild.com/concept/build_target_pattern.html)的列表，该模式标识可以在本身的 <code>deps</code>中包含其他规则的编译规则。

## 例子
大多数时候， <code> prebuilt_native_library</code>是 [android_library](https://buckbuild.com/rule/android_library.html)私有的。

```
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

```