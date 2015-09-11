# python_test()
该规则在未来可能会有所改变

一个<code>python_test()</code>规则是用来定义一组包含测试通过 [Python的单元测试框架](https://docs.python.org/2/library/unittest.html).py 文件集合。


## 参数

+ name（必需）规则的名称。
+ srcs（默认为[]）包含测试的 .py 文件的集合。
+ resources（默认为[]）和 python 源打包在一起的静态文件。这些资源可以在运行时通过 [pkg_resources](https://pythonhosted.org/setuptools/pkg_resources.html) API访问使用。
+ base_module（默认为 None）该规定给出来源和资源应该驻留在它们的顶层二进制文件的固定位置。如果不设置，即使用包含BUCK文件的项目相对目录。
+ deps（默认为[]）其它 [python_library()](https://buckbuild.com/rule/python_library.html) 规则，其中列出了该规则会从中导入模块的<code>srcs</code>。
+ labels(默认为[])要进行这些测试的标签的列表，这些标签是任意的文本字符串，并在 Buck 本身没有任何意义。但是，它们对于作为测试人员（如, smoke or fast）可以拥有含义。当执行 <code>buck test</code>时标签会被用来过滤或者包含一个指定的 <code>python_test()</code>规则。
+ source_under_test（默认 []）<code>python_test()</code>测试的 <code>python_library()</code>规则的。这些必须是在 <code>python_test</code>的传递依赖中。
+  visibilty（默认为[]）[构建目标模式（build target patterns）](https://buckbuild.com/concept/build_target_pattern.html)的列表，该模式标识可以在本身的 <code>deps</code>中包含其他规则的编译规则。



## 例子

```
# A rule that includes a single .py file containing tests.
python_test(
  name = 'fileutil_test',
  srcs = ['fileutil_tests.py'],
  deps = [
    ':fileutil',
  ],
  source_under_test = [
    ':fileutil',
  ],
)

# A rule that uses glob() to include all sources in the directory which the
# rule is defined.  It also lists a resource file that gets packaged with
# the sources in this rule.
python_library(
  name = 'fileutil',
  srcs = glob(['fileutil/**/*.py'],
  resources = [
    'testdata.dat',
  ],
)
```