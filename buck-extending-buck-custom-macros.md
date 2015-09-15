# 宏命令

由于构建文件接收有效的 Python 代码,所有有了定义 Python 函数的可能，该函数对创建规则有副作用。这样的函数被称为**宏命令**。

**警告**:构建文件以 Python 形式进行评估,因此可以做任何事(写文件,访问网络等)。这样做可能会导致 Buck 以特有的方式失败,因此既不支持也不鼓励。

例如,这是一个宏命令，命名为 `java_library_using_guava` 创建一个构建规则,创建一个 `java_library` 取决于 Guava :

```
def java_library_using_guava(
    name,
    srcs=[],
    resources=[],
    deps=[],
    visibility=[]):
  java_library(
    name = name,
    srcs = srcs,
    resources = resources,
    deps = [
      # This assumes this is where Guava is in your project.
      '//third_party/java/guava:guava',
    ] + deps,
    visibility = visibility,
  )
```

调用这个函数，看起来像是定义一个内置的生成规则。

```
# Calling this function has the side-effect of creating
# a java_library() rule named 'util' that depends on Guava.
java_library_using_guava(
  name = 'util',
  # Source code that depends on Guava.
  srcs = glob(['*.java']),
)
```

您还可以创建更复杂的宏命令，创建多个构建规则。例如,您可能想要创建一个构建规则，该规则可为 APK 产生调试和各种发布版本:

```
def create_apks(
    name,
    manifest,
    debug_keystore,
    release_keystore,
    proguard_config,
    deps):

  # This loop will create two android_binary rules.
  for type in [ 'debug', 'release' ]:
    # Select the appropriate keystore.
    if type == 'debug':
      keystore = debug_keystore
    else:
      keystore = release_keystore

    android_binary(
      # Note how we must parameterize the name of the
      # build rule so that we avoid creating two build
      # rules with the same name.
      name = '%s_%s' % (name, type),
      manifest = manifest,
      target = 'Google Inc.:Google APIs:16',
      keystore = keystore,
      package_type = type,
      proguard_config = proguard_config,
      deps = deps,
      visibility = [
        'PUBLIC',
      ],
    )
)
```

再一次调用这个函数，看起来像是定义一个内置的生成规则。

```
create_apks(
  name = 'messenger',
  manifest = 'AndroidManifest.xml',
  debug_keystore = '//keystores:debug',
  release_keystore = '//keystores:prod',
  proguard_config = 'proguard.cfg',
  deps = [
    # ...
  ],
)
```

请注意，如果这个被定义在 `apps/messenger/BUCK` ，会创建下列构建规则：

```
//apps/messenger:messenger_debug
//apps/messenger:messenger_release
```

然而，下列构建规则**不会**存在。

```
//apps/messenger:messenger
```

如果开发人员期望以下命令执行，运行结果一定会另开发人员很困惑。

```
buck build //apps/messenger:messenger
buck targets --type create_apks
```

在给宏命令命名时，包含你公司名称作为前缀，可以帮助实施宏命令不是一个内置规则的想法。另一方面宏的优点是他们类似于使用内置的规则。你如何将这些传达给你的团队完全取决于你。




