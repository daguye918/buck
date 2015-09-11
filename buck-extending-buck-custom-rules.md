# 给 Buck 添加自定义规则

在本文档的编写过程中,往 buck 中添加规则唯一的官方方法是 fork 项目并修改源代码。在某些时候,我们将构建一个美丽和优雅的扩展 API 。在那之前....
通过编辑 `KnownBuildRuleTypes` 开始。您需要编辑 `createBuilder()` 并找到大量调用 `builder.register()` 那一代码块。

```
builder.register(new YourDescription());
```

 `YourDescription` 是什么?是  [`Description`][1] 接口的一个实现。这个接口在 Buck 中有三个作用:


1. 它提供了规则类型的名称,如: `java_library`


2. 它表明该规则接受的参数


3. 它扮演类似一个 [`BuildRule`][2] 工厂实例的角色。

## 定义规则参数
通过实现 `Description.createUnpopulatedConstructorArg()` 可定义规则参数。这应该返回一个java对象,其中公共领域是 camelCased ，名字由 Buck 文件中规则里参数的名称决定。当引用 Buck 文件,这个公共领域名格式为 snake_cased 。意思是“someParam”将变成“some_param”。

您可以使用原语，枚举类型，泛型类型,集合(包括泛型集合),和构造函数参数领域的自定义类型。如果您正在使用一个自定义类型, Buck 不知道如何从JSON整理,您必须实现 [`TypeCoercer`][3] 。至于如何做到这一点，超出了本文档的范围。

注意:如果一个参数既不是一个本地文件也不是 `BuildRule` 的输出,那么您可以使用 [`SourcePath`][4] 代表参数。

如果一个参数被认为是可选的,那么这个领域应该宣布使用 Guava 的 `Optional` 类(例. `public Optional &lt;String&gt;  name;`）

##构建新的 BuildRule 实例
当 Buck 需要构造一个 `BuildRule` 将调用 `Description.createBuildRule()` ,通过一个构造函数参数实现。该参数集合类型将被实例化成一个空集合,即使他们被声明为可选的。

 `createBuildRule` 方法返回一个实例化的 `BuildRule` ，可以用来构造我们添加的任何规则。当前实例通常继承 [`AbstractBuildRule`][5] ,我们建议你也这样做。
 
 创建的 `BuildRule` 将被自动注册在 `BuildRuleResolver` 中。然而,很多情况下 `BuildRule` 取决于现有的功能，已经用 `BuildRule` 表示。在这种情况下,它可以在 `createBuildRule()` 方法中创建中间规则,将它添加到解析器,然后添加新创建的规则，并依赖一个最终返回的方法。唯一的警告:每个规则必须有它自己的,独一无二的, `BuildTarget` ,这是它所指向操作图的名字。


强烈建议你在 `BuildRule` 中，执行 `getBuildSteps()` 方法后再做任何工作。特别是,不在构造函数中分配任何额外的领域!例外情况：写出输出规则的路径。


你可以认为 `Description` 是一个工厂 `BuildRule`实例。同样,你可以认为创造一个特定输出工厂， `BuildRule`  作为 [`Step`][6] 的工厂是必须执行的步骤。与规则不同的是,在返回的顺序中，步骤是按顺序执行的。

## 确保您的规则正确地重建
如果您的规则扩展自 `AbstractBuildRule` ,那么简单地注释规则中的任何字段,有助于显示 [`@AddToRuleKey`][7] 的独特性。这是 Buck 用来计算规则的键,如果该键的值在构建时变化,将导致 Buck 重新执行 `BuildRule` 。

另一个提示,为了正确地重建规则，需确保每一次建立都清除输出目录。通常,您将看到规则的第一步类似这样:

```
Path outputDir = BuildTargets.getGenPath(target, "%s-output"); steps.add(new MakeCleanDirectoryStep(outputDir));
```

在您的 IDE 中,看看键的接口和类，你会发现更多可以用 Buck 做的事情：

 - [`Description`][8]
 - [`AbstractBuildRule`][9]
 - [`Step`][10]


  [1]: https://buckbuild.com/javadoc/com/facebook/buck/rules/Description.html
  [2]: https://buckbuild.com/javadoc/com/facebook/buck/rules/BuildRule.html
  [3]: https://buckbuild.com/javadoc/com/facebook/buck/rules/coercer/TypeCoercer.html
  [4]: https://buckbuild.com/javadoc/com/facebook/buck/rules/SourcePath.html
  [5]: https://buckbuild.com/javadoc/com/facebook/buck/rules/AbstractBuildRule.html
  [6]: https://buckbuild.com/javadoc/com/facebook/buck/step/Step.html
  [7]: https://buckbuild.com/javadoc/com/facebook/buck/rules/AddToRuleKey.html
  [8]: https://buckbuild.com/javadoc/com/facebook/buck/rules/Description.html
  [9]: https://buckbuild.com/javadoc/com/facebook/buck/rules/AbstractBuildRule.html
  [10]: https://buckbuild.com/javadoc/com/facebook/buck/step/Step.html