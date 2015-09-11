# glob()
<code>glob()</code>函数用于指定一组含有匹配符的文件。一个匹配符按不带重复或尾随分隔符（/）的相对路径结构化生成。匹配符可以包含 shell 类似的通配符 <code>*，?，[charset]</code>。此外，路径元素<code>**</code>会匹配任何子路径。例如<code>foo/**/bar</code>等效于<code>["foo/bar", "foo/*/bar", "foo/*/*/bar", ...]</code>。


## 参数

+ 第一个参数是一个字符串列表，确定用来匹配文件的当前目录下的文件名匹配符。
+ excludes（默认为[]）匹配符列表，匹配符用来标识第一个参数指定要删除的文件。
+ include_dotfiles（默认为 False）<code>*</code><code> **</code>匹配符以一个点查找文件或目录名。

## 示例

为了示例的目的，我们称一个"常规"文件不是一个“dotfile”。，如果它的相对路径（相对于当前目录）包括至少一个以一个点开始开始的路径元素，则该文件文件是一个 'dotfile'。所有这些常规的.java文件都在这个目录下：
```
glob(['*.java'])
```

所有在该目录下的 .java 文件（包含 dotfiles）
```
glob(['*.java'], include_dotfiles=True)
```

该目录下的所有文件：
```
glob(['**/'], include_dotfiles=True)
```

在目录 '.git'下的所有常规文件
```
glob(['.git/**/'])
```

在一个常规（非点）目录下的所有以 '.'开始文件名的文件
```
glob(['**/.*'])
```

该目录下的所有 .java 和 .aidl 常规文件
```
glob(['*.java', '*.aidl'])
```

该目录下的所有 .java  常规文件
```
该目录下的所有 .java 和 .aidl 常规文件
```

目录下所有文件名以 Test.java 结尾的文件
```
glob(['**/*Test.java'])
```

目录下所有文件名以 Test.java 或者 StringTests.java: 结尾的文件
```
glob(['**/*Test.java', 'StringTests.java'])
```
目录下所有文件名以 Test.java 结尾(除了 HaltingProblemTest.java)的文件
```
glob(['**/*Test.java'], excludes = ['HaltingProblemTest.java'])
```

目录下所有常规的 .java 文件，除了以 Test.java 结尾的文件
```
glob(['**/*.java'], excludes = ['**/*Test.java'])
```


