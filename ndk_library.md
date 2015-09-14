#ndk_library()

ndk_library() 用于定义一组 C/C++ 文件，一个 Android.mk 文件以及被 NDK 的 ndk_tool 用来生成一个或多个共用对象的 Application.mk 文件。

## 参数

- name（必要参数）- 规则的名称。
- deps（默认为 []）- 该规则之前应该构建的构建目标列表。
- flags（默认为 []）- 传递给 ndk-build 的字符串数组。正常情况下这是不需要的，但是有些情况下你希望放些什么在这里。例如，这可以用于调试模式(NDK_DEBUG=1)下构建库或者设置 ndk_build 产生的工作数目（默认情况下，这是和核心数一样的）。
- is_asset（默认为 False）- 通常是本机共享对象，最终存放在 APK 文件根目录的 lib 文件夹下。如果这个参数是 True，那么这些对象将会放在 assets/lib 下。将共用对象放在一个非标准的位置会组织 Android 把它们解压到设备内部储存中。
- visibility（默认为 []）- [构建目标样式](https://buckbuild.com/concept/build_target_pattern.html)列表，用于标识能够包含 deps 内规则的构建规则。

包含这个库的 Android_binary() 会把本地共用对象集中到 APK 根路径下名为 lib/ 或者 assets/lib/ 的目录下。

不同于 ndk 生成的默认调用，buck 将把所有中间文件和输出放到 buck-out/gen 下的子目录。

同样地，因为 ndk-build 以及 Android.mk 没有公开必要的依赖信息，buck 假定具有以下所有扩展名的文件都是构建的一部分：c, cpp, cc, cxx, h, hpp, mk。