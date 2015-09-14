# Dart 编码风格指南

*本指南由 Bob Nystrom 与 2011 年八月编写（2015 年二月更新）*

当我们构建好了 Dart 编码系统时，使用一致的编码风格是很重要的。本教程是精心编写的 Dart 风格指南，旨在帮助大家了解该语言独有的特性，并且让 Dart 开发者之间的协作更加容易。

也许在本教程中有些内容您并不认同。即使作为作者，也有一些事情是我所不认同的。所以，我希望各位读者能够先认同一点，那就是通常情况下，一致性远比个人喜好要有价值的多。

对于很多东西，比如 Dart 来说，教程并不是死板的，这一点一定要牢记在心。随着语言的发展，我们将会从中吸取许多经验，而我们的编码风格也将随之变化。这也就是必然会出现没有遵循最新风格的代码，也可能是由于指南中存在二义性的部分或者没有涉及的地方而使得读者按照自己的喜好编写了代码。这些疏漏之处还请读者们和我们一起承担，当 Dart 及其库逐渐稳定的时候我们的指南也会变得更好。

你也可以看一下相关文档：

- [API Naming Guide](https://www.dartlang.org/articles/api-naming-guide/)
- [Guidelines for Doc Comments](https://www.dartlang.org/articles/doc-comment-guidelines/)

## 如何阅读本指南

本指南按照从宏观到微观的顺序大致分为了几个部分。每个部分又包含一系列的指导准则。每个准则都包括下面这些词：

- **做** - 该准则形容了应始终遵循的做法。你几乎找不到理由不去遵循这些做法。
- **不要做** - 这些准则则是相反的：该部分说明的内容往往不是个好的选择。看完本指南，你将会注意到这块有不少的内容。其他语言中的这类准则有助于避免因时间推移而出现的错误。Dart 语言是全新的一门语言，使用 Dart 时我们可以直接修复这些陷阱而不需要总是小心翼翼的。
- **最好** - 这些是你应该遵循的做法。但是在某些情况下采取其他做法将会更好。你只需要理解这些暗示就好，当你实际动手时，应该忽略这些准则。
- **避免** - 这是和“最好”相对的：一般情况下你不应该做的事，但是在很少的情况下它又是一种很好的选择。
- **考虑** - 这些可能是你不希望遵循的，对于这些准则，你可以考虑一下具体情况、一些先例甚至是你个人的喜好再决定要不要遵循。

看完上面这些以后，你可能感觉如果没有一定的准备就会被编码风格打败。不用担心，本指南中的大多数准则都是常识，而我们都是明智的人。我们最终的目标，就是写出优美的、高可读性、高维护性的代码。

## 类型

如果一个简单的函数就能满足你的需求，就应当**避免**声明一个只有单个成员的抽象类。和 Java 不同，Dart 有着顶级函数，闭包以及非常美观的语法高亮。如果你需要的和回调是类似的，那么只需要用一个函数即可。如果你定义的类只有一个成员并且其名称没什么含义，就像是`call` 或者 `invoke` 一样。那么直接定义一个函数将会是更好的选择：

~~~

typedef bool Predicate(item); // good

~~~

~~~
abstract class Predicate {
    bool test(item);  // bad
}

~~~

## 成员

在创建实例的时候最好是使用构造函数而不是静态方法。

构造函数通常由 `new` 或者 `const` 调用，其主要目的就是返回该类的一个实例（或者至少实现了其接口）。

在 Dart 中你将不再*需要*用静态方法来创建一个实例。已命名的构造函数使你能够说明对象是如何构建的，并且工场构造函数允许你在合适的情况下构建子类以及子接口的实例。

尽管如此，一些采取一定技巧创建新对象的方法看起来“并不像构造函数”。比如，尽管 [Uri.parse()](https://api.dartlang.org/apidocs/channels/stable/dartdoc-viewer/dart-core.Uri#id_parse) 是一个静态方法，它却通过给定的参数创建了一个 Uri 对象。相应地，实现了 [Builder pattern](http://en.wikipedia.org/wiki/Builder_pattern) 的类看起来可能比使用静态方法的类要好一些。

不过，在大多数情况下，即使会显得程序冗长你也应该使用构造函数而不是静态方法。当其他人使用你的类来创建对象时，他们更希望能够使用构造函数按照常规方式来创建实例。

~~~

class Point {    // good
  num x, y;
  Point(this.x, this.y);
  Point.polar(num theta, num radius)
      : x = radius * math.cos(theta),
        y = radius * math.sin(theta);
}

~~~

~~~

class Point {    // bad
  num x, y;
  Point(this.x, this.y);
  static Point polar(num theta, num radius) {
    return new Point(radius * math.cos(theta),
        radius * math.sin(theta));
  }
}

~~~

对于空的构造函数体，**用 `;` 来代替 `{}`**。

在 Dart 中，一个函数体为空的构造函数可以由分号来结尾。这对于 const 类型的构造函数来说是*必要的*。考虑到一致性以及简洁性，其他构造函数也应该做到这点。

~~~

class Point {    // good
  int x, y;
  Point(this.x, this.y);
}

~~~

~~~

class Point {    // bad
  int x, y;
  Point(this.x, this.y) {}
}

~~~

请**务必**将 `super()` 的调用放在构造函数初始化列表的最后。

字段的初始化将会按照它们在构造函数初始化列表中出现的顺序来进行。如果你把对 `super()` 的调用放在初始化列表的中间，那么父类中的初始化将会在子类初始化列表中剩余内容之前被初始化。

这*并不是*说父类构造函数体中的内容将会被执行。这往往是在所有的初始化都完成之后才执行，而与 `super()` 的位置无关。初始化列表的影响正在逐渐降低，所以 `super()` 在列表中的位置几乎没有什么影响。

最好养成将父类构造函数放在子类构造函数最后的习惯，这有利于提升代码的连贯性。并且当父类构造函数体运行的时候，其可读性也会增加，甚至对运行的效果也会有提升。

~~~

View(Style style, List children)    // good
    :_children = children,
     super(style){

~~~

~~~

View(Style style, List children)    // bad
    :super(style),
     -children = children {
     
~~~

对于更改属性值的操作，**请**调用 setter 来执行。

如果函数的名称是以 `set` 开头的，这通常意味着该函数可能是 setter 函数。更准确的说，对于下面这些情况使用 setter 来代替函数是比较好的选择：

- 采用单个参数。
- 改变了对象中的某个状态。
- 有一个相应的 getter。对于使用者来说，有一些可以修改但是却无法看见的状态是有些奇怪的。（该准则反过来则是不对的，设置了 getter 而不为它定义 setter 也是可以的）
- 是幂等的。使用同一个值来调用两次 setter 时，第二次调用应当不含有其他操作。
- 为了快速运行。使用者们期待像 `foo.bar = value` 这样的表达式，因为它们执行很快。

~~~

rect.width = 3;    // good
button.visible = false;

~~~

你应当**避免**为了“安全”而将字段封装在 getter 和 setter 中。

在 Java 以及 C# 中，即便我们所需要的操作是直接对应字段的，通常情况下我们还是会将所有的字段都封装在 getter 和 setter 中（或者是 C# 的属性中）。事实上，如果你需要在成员上做更多操作，你可以不触及调用点。这是因为，在 Java 中调用一个 setter 函数和直接操作字段是不同的。而在 C# 中使用属性和直接操作字段是互不兼容的。

Dart 中并没有这种限制。字段以及 getter/setter 是完全相同的。你可以先使用类中的字段，然后在不改动任何使用了该字段的代码的情况下将其封装在 getter 以及 setter 内。

~~~ 

class Box {    // good
  var contents;
}

~~~

~~~

class Box {    // bad
  var _contents;
  get contents => _contents;
  set contents(value) {
    _contents = value;
  }
}

~~~

**最好**使用 public final 字段来而不是定义一个 private 字段然后为之设置一个 public setter。

如果你有一个代码之外可见的字段并且不会为它赋值（并且在构造函数之外你不会更改它），多数情况下最简单的方法就是直接让它作为 `fianl` 变量。

~~~

class Box {    // good
  final contents = [];
}

~~~

~~~

class Box {    // bad
  var _contents;
  get contents => _contents;
}

~~~

当某个成员的函数体返回的是单个表达式的时候，请**考虑**一下使用 `=>` 来定义这些成员。

除了函数表达式之外，Dart 也允许你使用 `=>` 来定义类的成员。这对于定义一些计算并返回单值的简单成员来说是个好习惯。

~~~

get width => right - left;    // good
bool ready(num time) => minTime == null || minTime <= time;
containsValue(String value) => getValues().contains(value);

~~~

即便函数体并不是单行的也可以使用 `=>`，但是不过你觉得将当行表达式拆开成多行代码比较好，那么将整个函数体放在花括号中并显示声明 `return` 看起来会更好。

你应该**避免**为了使用常规接口而从函数中返回 `this`。

对于连续的函数调用，使用函数级联是种好习惯。

~~~

var buffer = new StringBuffer()    // good
  ..write("one")
  ..write("two")
  ..write("three");
  
~~~

~~~

var buffer = new StringBuffer();    // bad
buffer
  .write("one")
  .write("two")
  .write("three");
  
~~~

你应该**避免**使用布尔类型的参数，除非它们的含义是非常明显的。

和其他类型不同，布尔值通常直接使用字面值而不是布尔变量。像数字我们通常会将其封装在命名的常量中，但是在使用的时候我们通常直接使用`true` 和 `false`。这样其他人就很难明白你的调用语句中布尔值究竟意味着什么：

~~~

new Task(true);    // bad
new Task(false);
new ListBox(false, true, true);

~~~

相反，在调用的时候，如果你使用命名的参数，命名的构造函数或者是命名后的常量看起来就很舒服。

~~~

new Task.oneShot();    // bad
new Task.repeating();
new ListBox(scroll: true, showScrollbars: true);

~~~

## 类型注解

对于公有 API，**最好**提供类型注解。

类型注解是非常重要的文档，它说明了相应的库应当如何使用。为参数以及公有方法的返回类型注解有利于使用者了解 API 需要什么参数以及它能提供什么功能。

但是，如果有个 API 可以接收任何参数，或者是 Dart 中无法表示的值，那么该 APi 可以不用添加注解。

对于库内部的代码（即便是私有的，或者是嵌套的函数），请再你认为有帮助的地方添加注解，但是不要认为你*必须*提供这些注解。

~~~

install(id, destPath) {    // bad
  // ...
}

~~~

在上面的代码中，我们就不清楚 `id` 到底是什么。字符串？那么 `destPath` 又是什么呢？字符串还是文件对象？这个函数是同步的还是异步的？

~~~

Future<bool> install(PackageId id, String destPath) {     // good
  // ...
}

~~~

当你加上类型之后，这个函数的相关信息也就说明白了。

对于局部变量，**最好**是使用 `var` 而不是类型注解来声明。

现在，我们更加倾向于让函数体的代码尽可能的简洁，并且局部变量的类型在初始化表达式中是可以推测出来的，在这种情况下显示声明其类型是没有必要的。好点的编辑器可以推测出局部变量的类型，所以自动补全功能仍然是可用的，并且你所期望的其他功能也可以正常使用。

~~~

Map<int, List<Person>> groupByZip(Iterable<Person> people) {    // good 
  var peopleByZip = new Map<int, List<Person>>();
  for (var person in people) {
    peopleByZip.putIfAbsent(person.zip, () => <Person>[]);
    peopleByZip[person.zip].add(person);
  }
  return peopleByZip;
}

~~~

~~~

Map<int, List<Person>> groupByZip(Iterable<Person> people) {    // bad
  Map<int, List<Person>> peopleByZip = new Map<int, List<Person>>();
  for (Person person in people) {
    peopleByZip.putIfAbsent(person.zip, () => <Person>[]);
    peopleByZip[person.zip].add(person);
  }
  return peopleByZip;
}

~~~

如果对代码的运行性能有要求，那么在传递参数的时候，类型注解**最好**是用 `double` 或者 `int` 来代替 `num`。

单一调用点（有着稳定输入类型）在优化时要比多态调用点（其输入类型可能会发生变化）更加容易。

对于数字类型，在添加类型注解时你应该指明具体的数字类型。明确地声明 `double` 或者 `int` 有助于使用你方法的人为之传递相应的参数。 

在正式初始化的时候**不要**做类型注解。

如果一个构造函数的参数使用了 `this.` 来初始化字段，那么该参数的类型必然和这个字段相同，故而不必使用类型注解。

~~~

class Point {    // good
  int x, y;
  Point(this.x, this.y);
}

~~~

~~~

class Point {    // bad
  int x, y;
  Point(int this.x, int this.y);
}

~~~

你应该**避免**在函数表达式中使用类型注解。

函数表达式的一大优点就是它的简洁性。如果一个函数复杂到需要注解类型来理解它，它就应该是一个函数语句或者是一个方法。相应的，如果一个函数简洁到可以作为一个表达式，那么它就不需要类型注解。

~~~

var names = people.map((person) => person.name);    // good

~~~

~~~

var names = people.map((Person person) {    // bad
  return person.name;
});

~~~

应当**避免**在不需要的时候使用 `dynamic` 来添加类型注解。

在 Dart 中，多数情况下类型注解都是可以忽略的，因为它们会被自动当做 `dynamic` 类型。所以，不添加类型注解在语义上是完全没问题的，并且代码会显得更简洁。

~~~

lookUpOrDefault(String name, Map map, defaultValue) {    // good
  var value = map[name];
  if (value != null) return value;
  return defaultValue;
}

~~~

~~~

dynamic lookUpOrDefault(String name, Map map, dynamic defaultValue) {    // bad
  var value = map[name];
  if (value != null) return value;
  return defaultValue;
}

~~~

对于接收的任何对象,**应该**使用 `Object` 代替 `dynamic` 来表明参数是对象。

有些操作可能对于任何对象都适用。比如，`toString()` 用于输出信息，并且可以用于任何对象。在 Dart 中有两种类型可以匹配所有对象：`Object` 以及 `dynamic`。但是，它们表示的是两种不同的东西。

`Object` 注解表明 “我可以接收任何对象，只要它含有相关对象自身定义的方法。”

`dynamic` 类型注解表明没有注解可以说明你实际允许的对象是什么类型。（也可能有，但是你不在意是否要声明）

~~~

//good
// Accepts any object.
void log(Object object) {
  print(object.toString());
}

// Only accepts bool or String, which can't be expressed in a type annotation.
bool convertToBool(arg) {
  if (arg is bool) return arg;
  if (arg is String) return arg == 'true';
  throw new ArgumentError('Cannot convert $arg to a bool.');
}

~~~

## 名称

命名时**应该**采取驼峰大小写形式。

类、枚举以及自定义类型应该将每个单词的首字母大写（包括第一个词），并且不能使用分隔符。

~~~

// good
class SliderMenu {
  // ...
}

class HttpRequest {
  // ...
}

typedef num Adder(num x, num y);

~~~

**最好**使用 `lowerCamelCase` 的形式为常量命名。

一般使用 `lowerCamelCase` 的形式为常量命名，枚举类型中的值也应该采用这种形式。

如果已有的代码中采用全大写的方式为常量命名，那么你可以打开大写锁定以保持代码的一致性。

~~~

// good
const pi = 3.14;
const defaultTimeout = 1000;
final urlScheme = new RegExp('^([a-z]+):');

class Dice {
  static final numberGenerator = new Random();
}

~~~

~~~

// bad
const PI = 3.14;
const kDefaultTimeout = 1000;
final URL_SCHEME = new RegExp('^([a-z]+):');

class Dice {
  static final NUMBER_GENERATOR = new Random();
}

~~~

>变动：在以前的代码中我们建议常量命名使用全大写。由于现在 const 从 final 中分离出来了，所以我们更改了命名的建议。当然，也有人希望枚举采用全大写的形式，但是我们希望以本指南当前的状况为准。

对于其他的标识符，**应该**使用 `lowerCamelCase` 的形式命名。

类的成员、全局定义、变量、参数以及命名的参数都应该将除了第一个单词以外的词语大写，并且不能使用分隔符。

~~~

//good
var item;

HttpRequest httpRequest;

align(clearItems) {
  // ...
}

~~~

对于在元数据注解中使用的类，**应该**采用 `UpperCamelCase` 的形式命名。

如果注解不需要任何参数，你可能希望为其创建一个 `lowerCamelCase` 形式的常量。

~~~

// good 
@Foo(anArg)
class A { ... }

@Foo()
class B { ... }

@foo
class C { ... }

~~~

对于库以及源文件，**应该**采用 `lowercase_with_underscores` 的形式命名。

有些文件系统是不区分大小写的，所以很多项目要求文件名应该全部采用小写的形式。这种情况下使用下划线将文件名中的单词分隔开，这样就可以使文件名依旧具有可读性。使用下划线作为分隔符的名称依旧是有效的 Dart 标识符，如果以后 Dart 增添了对符号引用的支持，那么该特性将会非常有用。

好的例子：

- `slider_menu.dart`
- `file_system.dart`
- `library peg_parser`

坏的例子：

- `SliderMenu.dart`
- `filesystem.dart`
- `library peg-parser`

声明库的前缀的时候**应该**采取 `lowercase_with_underscores` 的形式。

~~~

// good
import 'dart:math' as math;
import 'dart:json' as json;
import 'package:js/js.dart' as js;
import 'package:javascript_utils/javascript_utils.dart' as js_utils;

~~~

~~~

// bad
import 'dart:math' as Math;
import 'dart:json' as JSON;
import 'package:js/js.dart' as JS;
import 'package:javascript_utils/javascript_utils.dart' as jsUtils;

~~~

库名称的前面**应该**加上包名并且用 `.` 来分隔路径。

本指南将会帮助你避免因为两个库有相同名称而产生的警告。以下使我们推荐的一些规则：

- 所有的库名称之前都加上包名作为前缀。
- 入口库的名称应当和包名一致。
- 对于包中的其他库，在包名后使用 `.` 来分隔库所对应的 Dart 文件所在路径。如果是 `lib` 下的库，名称中不用加入 lib。

举个例子，如果包名是 `my_package`。下面是该包中其他库文件的名称：

~~~

// good 
// In lib/my_package.dart
library my_package;

// In lib/other.dart
library my_package.other;

// In lib/foo/bar.dart
library my_package.foo.bar;

// In example/foo/bar.dart
library my_package.example.foo.bar;

// In lib/src/private.dart
library my_package.src.private;

~~~

对于超过两个字母的首字母缩略词以及缩写词，**应该**大写。

大写首字母缩略词可能有点难以阅读，并且当该词由相邻的多个单词构成时可能会导致歧义。像 `HTTPSFTPConnection` 这样的名称，你无法判断它究竟是 HTTPS FTP 连接还是 HTTP SFTP 连接。

为了避免出现这种情况，首字母缩略词以及缩写词还是像常规词语那样书写，如果只有两个字母的话，可以按照个人喜好命名。（两个字母的缩写词，像 ID 以及 Mr. 也可以想常规词语一样书写）

~~~

// good
HttpConnection
uiHandler
IOStream
HttpRequest
Id
id
Pt
DB

~~~

~~~

// bad
HTTPConnection
UiHandler
IoStream
HTTPRequest
ID
PT
Db

~~~

## 注释

在说明成员以及类型的时候**应该**使用 doc 注释。

尽管 Dart 支持两种类型的 doc 注释（`///` 以及 `/**`），我们推荐 `///`，因为它更加紧凑（`/**` 以及 `*/` 用于多行注释，并且有两行是空的）。在有些情况下，`///` 也更加容易阅读，比如在某个 doc 注释中包含了一个列表，并且该列表使用 `*` 来标记各个列表项。

~~~

// good
/// Parses a set of option strings.
///
/// For each option:
///
/// * If it is `null`, then it is ignored.
/// * If it is a string, then [validate] is called on it.
/// * If it is any other type, it is *not* validated.
void parse(List options) {
  // ...
}

~~~

在 doc 注释中，你可以使用 [markdown](http://daringfireball.net/projects/markdown/) 格式。

**应该**使用单行注释。

~~~

// good
greet(name) {
  // Assume we have a valid name.
  print('Hi, $name!');
}

~~~

~~~

// bad
greet(name) {
  /* Assume we have a valid name. */
  print('Hi, $name!');
}

~~~

你可以使用块注释（`/* ... */`）来在某段代码之外进行注释。

注释也**应该**像普通句子一样大小写并且添加标点。

这并不是说注释就应该是一个完整的句子，尽管多数情况下它应该是一个完整的句子。“返回条目的数量”就是一个可接受的注释。

~~~

// good
// Remove the last item from the collection.

~~~

~~~

// bad
// remove the last item from the collection

~~~

在注释中**应该**使用方括号把相应域中的标识符标记出来。

如果你把变量、方法或者类型的名称放在方括号内，那么文档生成器就可以查找到相应的名称，并且将其与代码连接起来。

~~~

// good
import 'dart:math';

/// Rolls both [Dice] and returns the highest rolled value.
num greatestRoll(Dice a, Dice b) => max(a.roll(), b.roll());

//

~~~

在文档注释中**应该**描述函数签名。

在其他语言中，要使用很多的标签以及各个部分来说明函数的参数是什么，返回值又是什么。

~~~

//bad
/// Defines a flag with the given name and abbreviation.
///
/// @param name The name of the flag.
/// @param abbr The abbreviation for the flag.
/// @returns The new flag.
/// @throws IllegalArgumentException If there is already an option with
///     the given name or abbreviation.
Flag addFlag(String name, String abbr) {
  // ...
}

~~~

使用 Dart 则相当便捷，你可以直接使用方括号把参数等信息整合到函数描述中。

~~~

// good
/// Defines a flag.
///
/// Throws an [IllegalArgumentException] if there is already an option named
/// [name] or there is already an option using abbreviation [abbr]. Returns
/// the new flag.
Flag addFlag(String name, String abbr) {
  // ...
}

~~~

注释**应该**放在元数据注解之前。

~~~

// good
/// _Deprecated: Use [newMethod] instead._
@deprecated
oldMethod();

~~~

~~~

// bad
@deprecated
/// _Deprecated: Use [newMethod] instead._
oldMethod();

~~~

## 空白符

和其他语言类似，Dart 会忽略空白。但是，人们却不会这样。在代码中加入空格调整样式后可以让人们看到内容和编译器看到的类似。

**不要**使用 Tab 缩进。

使用空白符来格式化代码可以保证每个人在编辑器中看到的内容是一样的。这也会和传送到博客上的样式一样，或者是一些代码站点，比如 [Github](https://github.com/)。

现在的编辑器在缩进的时候可以模拟 tab 键来进行缩进，以让你在编写代码的时候更加轻松，并且可以保证代码的一致性。

你应该**避免**在编写代码时一行的长度超过八十个字符。

可读性研究表明，过长的文本不适合阅读，因为当你看到下一行的时候眼睛移动的距离过大。这就是为什么新闻以及杂志的文本都是多行的。

如果你在编码时希望一行的代码可以超过八十个字符，那么对于阅读者而言你的代码过于冗长并且有点太紧凑了。你真的希望调用一个名为 `AbstractWidgetFactoryManagerBuilder` 的函数吗？

**应该**将二元操作符放在多行表达式的前一行。

各种样式的参数都是有效的，但是大多数情况下代码看起来都是下面这个样子的，并且这样更容易保证代码的一致性。

~~~

// good
if (isDeepFried ||
    (hasPieCrust && !vegan) ||
    containsBacon) {
  print('Bob likes it.');
}

~~~

注意这也可以直接使用 `=>` ：

~~~

// good
bobLikes() =>
    isDeepFried || (hasPieCrust && !vegan) || containsBacon;
    
~~~

~~~

// bad
bobLikes()
    => isDeepFried || (hasPieCrust && !vegan) || containsBacon;
 
~~~

**应该**将三元运算符放在多行表达式的下一行。

同样的，如果你在操作符之前换行了，那么全部操作符之前都要换行。

~~~

// good
return someCondition
    ? whenTrue
    : whenFalse;

~~~

~~~

// bad
return someCondition ?
    whenTrue :
    whenFalse;
return someCondition
    ? whenTrue : whenFalse;
    
~~~

在多行表达式中**应该**在下一行加入 `.` 操作符。

这条规则优先级高于上一条。可其他操作符不同，如果你使用 `.` 来分隔一个表达式，那就应该把它放在第二行的开头。

~~~

// good
someVeryLongVariable.withAVeryLongProperty
    .aMethodOnThatObject();

~~~

在代码块中**应该**缩进两格。

~~~

// good
if (condition) {
  print('hi');
}

~~~

如果代码过长需要换行，那么下一行**应该**缩进四格。

~~~

// good
someLongObject.aReallyLongMethodName(longArg, anotherLongArg,
    wrappedToNextLine);
   
~~~

你也可以按照你的喜好来进行缩进：

~~~

// good
someLongObject.aReallyLongMethodName(longArg, anotherLongArg,
                                     wrappedToNextLine);

~~~

注意使用 `=>` 也是可以的：

~~~

// good
bobLikes() =>
    isDeepFried || (hasPieCrust && !vegan) || containsBacon;
    
~~~

~~~ 

// bad 
bobLikes() =>
  isDeepFried || (hasPieCrust && !vegan) || containsBacon;
  
~~~

如果某一行的上一行是一个函数表达式，那么该行**不应该**缩进。

上面这个规则的一个例外就是函数表达式嵌套在另一个表达式中，比如是作为参数传递给一个方法。这些应该写成下面的格式：

~~~

// good
new Future.delayed(const Duration(seconds: 1), () {
  print('I am a callback');
});

~~~

~~~

// bad
new Future.delayed(const Duration(seconds: 1), () {
      print('I am a callback');
    });
    
~~~

花括号（{）**应该**放在它之前那一行的后面。

~~~

// good
class Foo {
  method() {
    if (true) {
      // ...
    } else {
      // ...
    }
  }
}

~~~

对于全部的控制语句，都**应该**使用括号括起来。

这样做有助于避免 [else 悬挂](http://en.wikipedia.org/wiki/Dangling_else) 的问题。

~~~

// good
if (true) {
  print('sanity');
} else {
  print('opposite day!');
}

~~~

~~~

// bad
if (true) print('sanity');
else
  print('opposite day!');
  
~~~

这种情况有个特例：单个 `if` 语句，如果没有相应的 `else` 语句就可以不实用括号。

~~~

// good
if (arg == null) return defaultValue;

~~~

switch 语句中的 case 语句**应该**缩进两个空格，case 语句的函数体**应该**缩进四个空格。

~~~

// good
switch (fruit) {
  case 'apple':
    print('delish');
    break;

  case 'durian':
    print('stinky');
    break;
}

~~~

在函数、操作符或者 setter 的名称与其参数列表间**不要**加入空格。

~~~

// good
bool convertToBool(arg) { ... }
bool operator ==(other) { ... }
set contents(value) { ... }

~~~

~~~

// bad
bool convertToBool (arg) { ... }
bool operator == (other) { ... }
set contents (value) { ... }

~~~

在 `operator` 关键字之后**应该**空一格。

~~~

// good
bool operator ==(other) => ...;

~~~

~~~

// bad
bool operator==(other) => ...;

~~~

二元、三元运算符的周围**应该**空一格，逗号之后也**应该**空一格，但是在一元运算符周围不应该留空格。

注意 `<` 以及 `>` 用在表达式中时是作为二元操作符的，但是作为泛型的时候则不是。`is` 以及 `!is` 只能作为二元运算符。另外，`.` 用于访问成员的时候绝对不能插入空格。

~~~

// good
a = 1 + 2 / (3 * -b);
c = !condition == a > b;
d = condition ? b : object.method(a, b, c);
if (obj is! SomeType) print('not SomeType');

~~~

~~~

// bad
a=1+2/(3* - b);
c= ! condition==a>b;
d= condition?b:object.method(a,b,c);
if (obj is !SomeType) print('not SomeType');

~~~

在循环语句中，`in` 的周围以及每个 `;` 之后都**应该**空一格。

~~~

// good
for (var i = 0; i < 100; i++) {
  // ...
}

for (final item in collection) {
  // ...
}

~~~

流控制的关键字之后**应该**空一格。

这和函数调用不同，函数调用中函数名和括号之间不应该有空格。

~~~

// good 
while (foo) {
  // ...
}

try {
  // ...
} catch (e) {
  // ...
}

~~~

在 `(`、`[`、`{` 之前，以及 `)`、`]`、`}` 之后**不应该**有空格。

同样的，如果 `<` 和 `>` 用作泛型的话也不应该在其中间留空格。

~~~

// good
var numbers = <int>[1, 2, (3 + 4)];

~~~

在函数体内的 `{` 前**应该**空一格。

上述规则有一个特殊情况。如果 `{` 是用在某个函数的参数列表之后，那么在 `)` 和它之间应该空一格。

~~~

// good
getEmptyFn(a) {
  return () {};
}

~~~

~~~ 

// bad
getEmptyFn(a){
  return (){};
}

~~~

构造函数中的初始化**应该**保证每个字段都单独占一行。

~~~

// good
MyClass()
    : firstField = "some value",
      secondField = "another",
      thirdField = "last" {
  // ...
}

~~~

注意 `:` 应该放在函数名称的下一行开头，并且应该缩进四格。而字段则应该对齐（也就是说第一个字段总共缩进六格）。

对于已命名的的实参和形参，在 `:` 之后**应该**空一格。

~~~

// good
class ListBox {
  bool showScrollbars;

  ListBox({this.showScrollbars: false});
}

main() {
  new ListBox(showScrollbars: true);
}

~~~

~~~

// bad
new ListBox(showScrollbars:true);
new ListBox(showScrollbars : true);

~~~

可选位置参数的 `=` 周围**应该**空一格。

~~~

// good
class HttpServer {
  static Future<HttpServer> listen([int port = 80]) {
    // ...
  }
}

~~~

~~~

// bad
import 'dart:async';

class HttpServer {
  static Future<HttpServer> listen([int port=80]) {
    // ...
  }
}

~~~

方法的级联**应该**缩进两格。

~~~

// good
list = new List()
  ..addAll([1, 2, 3])
  ..addAll([4, 5, 6]);
  
~~~

~~~

// bad
list = new List()
    ..addAll([1, 2, 3])
    ..addAll([4, 5, 6]);
    
~~~

>以前的时候这条规则是“缩进四格”，但是根据我们对于已有代码的调查结果来看，级联使用两格缩进更加常见。

多行列表以及映射的字面值的代码如果采取多行形式，那么**应该**缩进两行，并且把闭合的 `]` 或者 `}` 放在下一行。

多行列表以及映射的字面值最好是在逗号之后换行。

`]` 或者 `}` 应该根据上下文代码来进行缩进，以匹配上下文代码的格式。

~~~

// good
var simpleList = [1, 2, 3];
var simpleMap = {'a': 1, 'b': 2, 'c': 3};

var fooList = [
  'a',
  'b',
  'c'
];

var barMap = {
  'a': 'b',
  'c': 'd'
};

var listInsideMap = {
  'a': ['b', 'c', 'd'],
  'e': [
    'f',
    'g',
    'h'
  ]
};

var mapInsideMap = {
  'a': {'b': 'c', 'd': 'e'},
  'f': {
    'f': 'g',
    'h': 'i'
  }
};

var mapInsideList = [
  {
    'a': 'b',
    'c': 'd'
  },
  {
    'a': 'b',
    'c': 'd'
  },
];

~~~

~~~

// bad
var fooList = [
    1,
    2,
    3
];

var fooList = [
  1,
  2,
  3];

var fooList = [1, 2,
  3, 4];
  
  
~~~

## 字符串

在不必要的时候应该**避免**使用花括号。

如果你只是插入一个简单的标识符并且该标识符的结尾处没有其他的文本，那么 `{}` 可以删除掉。

~~~

// good
'Hi, $name!'
"Wear your wildest $decade's outfit."
'Wear your wildest ${decade}s outfit.'

~~~

~~~

// bad
'Hi, ${name}!'
"Wear your wildest ${decade}'s outfit."

~~~

## 顺序

**应该**先声明引用 Dart 库的 import 语句，然后是引用包的语句，最后是其他相关的引用。

每个不同的引用部分应该使用空行隔开。

在每个部分内部，应该按照字母顺序排列。如果你使用  `package:` 引用语句来引用你自己包中的内容，最好是将它放在相关引用中。

~~~

// good
import 'dart:async';
import 'dart:convert' show JSON;
import 'dart:html';

import 'package:bar/bar.dart'
import 'package:bar/foo.dart'
import 'package:foo/bar.dart'

import 'a.dart';

~~~

~~~

// bad
import 'dart:html';
import 'dart:async';
import 'dart:convert' show JSON;

import 'a.dart';
import 'package:bar/bar.dart'
import 'package:foo/bar.dart'
import 'package:bar/foo.dart'

~~~

**最好**是为每个部分中的所有引用语句声明其出口。

在出口部分之前应该空一行。

~~~

// good 
import 'src/error.dart';
import 'src/string_source.dart';

export 'src/error.dart';

~~~

~~~

// bad
import 'src/error.dart';
export 'src/error.dart';

import 'src/string_source.dart';

~~~