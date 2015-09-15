#cxx_test()


##参数

- `name`（这个参数是必需的）规则的名称。 

- `srcs`（默认为 ［］）整套 C, C++ ，汇编源文件预处理,编译都遵循这个规则。我们通过它们的文件扩展名确定哪些阶段运行在哪个输入源，通过 [GCC文档](https://gcc.gnu.org/onlinedocs/gcc/Overall-Options.html) 可以了解到解读文件扩展名上的更多细节。每个元素可以是一个指定的文件名(如 ‘foo / bar.c’ )或者是一个可以用来指定编译方式的标志（如 （'foo / bar.c ',[' - wall ',' -Werror ']））。在后一种情况下，在预处理和编译该文件（如适用）时，指定的标志将被使用在规则的其他标志上。

- `headers` （默认是 [] ）包含了目标源文件和其依赖之间关系的头文件。它的值应该是一个头文件列表或一个头文件的字典。文件名可以包含正斜杠（/）。头里面可以是 `#include "$HEADER_NAMESPACE/$HEADER_NAME"` 或者是 `#include <$HEADER_NAMESPACE/$HEADER_NAME>` ，其中 `$HEADER_NAMESPACE` 是变量 header_namespace 的值，`$HEADER_NAME` 是头的名字。请查看 `header_namespace` 获取更多信息。

- `preprocessor_flags`（默认为 [] ）当预处理任何上述来源文件时标记使用(需要预处理)。  

- `compiler_flags`（默认为 [ ] ）当编译任何上述来源时使用的标记(需要预处理)。 

- `linker_flags`（默认为 [ ] ）无论这个规则是否包络一个连接操作都标记使用（比如链接进一个可执行程序或共享库）。  

- `framework` （默认为 "gtest" ）构建和运行的测试框架。我们目前支持 gtest 和 boost 。 

- `env` （默认为 {} ） 运行测试时设置的环境名称和值的映射 (Map)。

- `run_test_separately` （默认为 false） 如果设置为 true，那么此规则中的测试是单独运行于所有其他测试中的。（这对于集成测试，访问物理设备或其他有限的资源是很有用的）
如果不设置，与其他测试一样。

- `visibility` （默认是 [] ）参数是[build target patterns](https://buckbuild.com/concept/build_target_pattern.html)给出的列表


##例子

````
# 一个使用 gtest 来构建和运行 C/C++ 的规则

cxx_test(
  name = 'echo_test',
  srcs = [
    'echo_test.cpp',
  ],
)
````