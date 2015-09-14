# apk_genrule()

apk_genrule() 用于后置处理 APK 文件。BUCK 生成 APK 的时候，它知道哪些 apk_genrule 可以从 genrule 中分离出一条一条的 apk_genrule，所以像 buck install 或者 buck uninstall 这样的命令仍然可以使用。另外，apk_genrule 也可以作为其他 apk_genrule() 规则的输入。

## 参数

- name（必要参数）- 规则的名称。
- apk（必要参数）- 输入的 android_binary() 规则。APK 的路径可以通过 shell 变量 $APK 来获取。
- srcs（默认为 []）- 在 shell 命令中使用的一系列文件。
- cmd（默认为 None）- 运行时用于生出输出文件的 shell 命令。这是 bash 以及 cmd_exe 的回退。它将被下列环境变量替换：<br>
&nbsp;SRCS<br>
&nbsp;srcs 参数扩展成的由空格分隔开的字符串，srcs 的每个元素都会转换为绝对路径。<br>
&nbsp;SRCDIR<br>
&nbsp;运行命令时优先复制的资源的绝对路径。<br>
&nbsp;OUT<br>
&nbsp;genrule() 产生的输出文件。由该变量声明的文件必须是由该命令书写的。如果不是，该规则的执行将会被视为失败，并且停止整个构建过程。<br>
&nbsp;在 cmd 内将引用扩展到其他规则也是可能的，只需要使用内建的[字符串参数宏定义](https://buckbuild.com/function/string_parameter_macros.html)就可以。这种扩展采用了两种形式：<br>
&nbsp;$(classpath /F;path/to:target)<br>
&nbsp;展开一个构建规则，将会显示一个可执行命令，命令是执行那条指令所必须的。比如，一个 java_binary() 规则展开后是对 java -jar path/to/target.jar 的调用。可执行的文件（可能是由 genrule() 生成的）也会被展开。如果构建规则没有生成一个可执行的输出文件，一个异常将会被抛出，并且构建将会停止。<br>
&nbsp;$(location //path/to:target)<br>
&nbsp;展开构建规则的输出文件的路径。这意味着你可以引用这个位置而不需要知道 Buck 是怎么在构建中的时候在磁盘上存储数据的。注意，在命令中展开的全部构建规则都会被自动视为是 genrule() 的依赖。
- bash（默认为 None）- cmd 参数所对应的特定平台版本。在 UNIX 系统上安装了 bash，并且其优先级高于 cmd。命令的执行都将是 /bin/bash -c 的格式。
- cmd_exe（默认为 None）- cmd 参数所对应的特定平台版本。在 Windows 系统上该参数比 cmd 有着更高优先级。执行命令时通常是 cmd.exe /c 的格式。
- out（必要参数）- 输出文件的名字。在 genrule() 被声明的构建文件中必须是唯一的。
- deps（默认为 []）- 在该规则之前必须构建的全部规则。这些可能是在 cmd 中通过括号括起来使用的变量：<br>
&nbsp;$(location //like:this)。
- visibility（默认为 []）- [构建目标样式](https://buckbuild.com/concept/build_target_pattern.html)，用于标识能够包含 deps 规则的构建规则。

## 示例

这里有一堆关于使用 apk_genrule() 打开 APK 的示例，我们对其做了一些高级标记，然后再次打包了那个 APK 文件。

~~~

# 构建该规则会生成一个名为 message.apk 的文件
android_binary(
  name = 'messenger',
  manifest = 'AndroidManifest.xml',
  target = 'Google Inc.:Google APIs:16',
  keystore = '//keystores:prod',
  package_type = 'release',
  proguard_config = 'proguard.cfg',
  deps = [
    ':res',
    ':src',
  ],
)

apk_genrule(
  name = 'messenger_super_sign_unalign',
  apk = ':messenger',
  deps = [
    '//java/com/facebook/sign:super_sign',
  ],
  bash = '$(exe //java/com/facebook/sign:super_sign) --input $APK --output $OUT',
  cmd_exe = '$(exe //java/com/facebook/sign:super_sign) --input %APK% --output %OUT%',
  out = 'messenger_super_sign_unalign.apk',
)

apk_genrule(
  name = 'messenger_super_sign',
  apk = ':messenger_super_sign_unalign',
  deps = [],
  bash = '$ANDROID_HOME/tools/zipalign -f 4 $APK $OUT',
  cmd_exe = '%ANDROID_HOME%\\tools\\zipalign -f 4 %APK% %OUT%',
  out = 'messenger_super_sign.apk',
)

~~~