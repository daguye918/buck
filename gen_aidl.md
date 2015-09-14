# gen_aidl()

gen_aidl() 规则用于从 .aidl 文件中生成 .java。

## 参数

- name（必要参数）- 规则的名称。
- aidl（必然参数）- 要转换为 .java 文件的 .aidl 文件路径。
- import_path（必然参数）- aidl 命令中 import 语句的搜索路径。（这是当从命令行中调用 aidl 时 -I 参数所对应的内容）
- deps（默认为 []）- 在该规则之前应当构建的规则。
- visibility（默认为 []）- [构建目标样式](https://buckbuild.com/concept/build_target_pattern.html)列表，用于标识能够包含 deps 内规则的构建规则。