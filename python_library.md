# python_library()
改规则在将来容易改变。

一个<code>python_library()</code>规则用于将 Python 源和资源组合起来打包成一个顶级 [python_binary](https://buckbuild.com/rule/python_binary.html) 规则。

## 参数

+ name（必需）规则的名称。
+ src（默认为[]）包含在该库 .py 文件的集合。
+ resources（默认为[]）和 python 源打包在一起的静态文件。这些资源可以在运行时通过 [pkg_resources](https://pythonhosted.org/setuptools/pkg_resources.html) API访问使用。
+ base_module（默认为 None）该规定给出来源和资源应该驻留在它们的顶层二进制文件的固定位置。如果不设置，即使用包含BUCK文件的项目相对目录。
+ deps（默认为[]）其它 [python_library()](https://buckbuild.com/rule/python_library.html)规则，其中列出了该规则会从中导入模块的<code>srcs</code>。
+  visibilty（默认为[]）[构建目标模式（build target patterns）](https://buckbuild.com/concept/build_target_pattern.html)的列表，该模式标识可以在本身的 <code>deps</code>中包含其他规则的编译规则。

## 例子

```
# A rule that includes a single .py file.
python_library(
  name = 'fileutil',
  srcs = ['fileutil.py'],
  deps = [
    '//third_party/python-magic:python-magic',
  ],
)

# A rule that uses glob() to include all sources in the directory which the
# rule is defined.  It also lists a resource file that gets packaged with
# the sources in this rule.
python_library(
  name = 'testutil',
  srcs = glob(['testutil/**/*.py'],
  resources = [
    'testdata.dat',
  ],
)
```