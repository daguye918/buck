# keystore()

keystore() 包含由 keytool 可执行文件以及 JDK 创建的 密匙对数据。这是 [android_library](https://buckbuild.com/rule/android_binary.html) 规则的必要输入内容。

## 参数

- name（必要参数）- 规则的名称。
- store（必要参数）- 包含键的文件路径。这相当于使用 keytool 时的 -keystore 参数。
- properties（必要参数）- 含有下面内容的的 .properties 文件路径：

~~~

# The value that you passed as the argument to -alias
# when you ran keytool.
key.alias=my_alias

# The value that you entered in response to
# the "Enter keystore password:" prompt.
key.store.password=store_password

# The value that you entered in response to
# the "Enter key password for <my_alias>" prompt.
key.alias.password=alias_password

~~~