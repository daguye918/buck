#prebuilt_native_library()

#参数

- `name`（这个参数是必需的）规则的名称。 

- `native_libs` （默认为空）原生库的路径。该目录的子目录为不同的架构，例如 armeabi 或者 armeabi-v7a 。

- `is_asset` （默认为 false ）普通的共享库，位于 APK 的根目录下的 lib 文件夹。如果它的值为 ture ，这些对象会被放置到 assets/lib/ 下。将共享对象放置在非标准的位置，防止 Android 从中提取到设备的内部存储。



- `visibility` （默认是 [] ）参数是build target patterns给出的列表

#例子

大多是情况下，prebuilt_native_library 对于 [android_library](https://buckbuild.com/rule/android_library.html) 是私有的。

````
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
````