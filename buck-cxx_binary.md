# cxx_binary() 
在未来有更改的可能  
  
cxx_binary() 规则可以将 C/C++ 源文件和其依赖构建成一个二进制可执行文件。如果 C/C++ 的依赖库是链式的，那么所生成的本地可执行程序将链接其的静态链接库（一种不使用 [PIC](https://en.wikipedia.org/wiki/Position-independent_code) 的方式）。  
  
## 参数  

  
- `name`（这个参数是必需的）规则的名称。  
  
- `srcs`（默认为 ［］）整套 C, C++ ，汇编源文件预处理,编译都遵循这个规则。我们通过它们的文件扩展名确定哪些阶段运行在哪个输入源，通过 [GCC文档](https://gcc.gnu.org/onlinedocs/gcc/Overall-Options.html) 可以了解到解读文件扩展名上的更多细节。每个元素可以是一个指定的文件名(如 ‘foo / bar.c’ )或者是一个可以用来指定编译方式的标志（如 （'foo / bar.c ',[' - wall ',' -Werror ']））。在后一种情况下，在预处理和编译该文件（如适用）时，指定的标志将被使用在规则的其他标志上。

- `platform_srcs`（默认为 ［］）平台相关的源文件。这里应该是一个参数列表，第一个参数是一个与平台名称匹配的，不违反安全性的正则化表达式（在 java.util.regex.Pattern syntax 包中）。 第二个参数是源文件的列表或是要预处理的编译标识列表中的一个。在编译和装配时，如果符合正则表达式，请参见 `srcs` 获得更多信息。


- `headers` （默认是 [] ）包含了目标源文件和其依赖之间关系的头文件。它的值应该是一个头文件列表或一个头文件的字典。文件名可以包含正斜杠（/）。头里面可以是 `#include "$HEADER_NAMESPACE/$HEADER_NAME"` 或者是 `#include <$HEADER_NAMESPACE/$HEADER_NAME>` ，其中 `$HEADER_NAMESPACE` 是变量 header_namespace 的值，`$HEADER_NAME` 是头的名字。请查看 `header_namespace` 获取更多信息。

- `platform_headers` （默认是 [] ） 平台相关的头文件。这里应该是一个参数列表，第一个参数是一个与平台名称匹配的，不违反安全性的正则化表达式（在 java.util.regex.Pattern syntax 包中）。第二个参数是源文件的列表或是要预处理的编译标识列表中的一个。请参见 `srcs` 获得更多信息。

  
- `header_namespace`（默认为 `name` ）包含此目标的头的路径前缀。默认为目标的名称。可以包含斜杠(/),但不能从一开始就使用。有关更多信息,请参见`headers`。  
  

- `preprocessor_flags`（默认为 [ ] ）当预处理任何上述来源文件时标记使用(需要预处理)。  
  
- `platform_preprocessor_flags`（默认为 [ ] ）平台特定的预处理器标记。这些应该指定为一组对,第一个元素是一个平台名称匹配的，不违反安全性的正则化表达式（在 java.util.regex.Pattern syntax 包中）。第二个元素是列表标记，当预处理目标的来源时使用。有关更多信息,请参见`preprocessor_flags`。  
  
- `compiler_flags`（默认为 [ ] ）当编译任何上述来源时标记使用(需要预处理)。  
  
- `platform_compiler_flags`（默认为 [ ] ）平台特定的编译器标志。这些应该指定为一组对,第一个元素是一个平台名称匹配的，不违反安全性的正则化表达式（在 java.util.regex.Pattern syntax 包中）。第二个元素是列表标记，当编译目标的来源时使用。有关更多信息,请参见`compiler_flags`。  
  
- `linker_flags`（默认为 [ ] ）无论这个规则是否包络一个连接操作都标记使用（比如链接进一个可执行程序或共享库）。  
  
- `platform_linker_flags `（默认为 [ ] ）平台特定的编译器标志。这些应该指定为一组对,第一个元素是一个平台名称匹配的，不违反安全性的正则化表达式（在 java.util.regex.Pattern syntax 包中）。第二个元素是列表标记，当这个目标包含一个连接操作时使用。有关更多信息,请参见`linker_flagss`。  
  
- `link_style`（默认为 `static`）决定是否建立并且静态或动态链接这个二进制文件的依赖关系。可以是 `static` 或 `dynamic` 的。  
  
- `tests`（默认为 [ ] ）确定执行此目标的测试规则的生成目标列表。
  
- `visibility `（默认为 [ ] ）确定构建规则的目标模式列表,可以包括`deps`这个规则。  
  
##例子    
    

````

# 从一个.cpp文件构建了一个 C/C++ 本地可执行的程序 
# 其相应的头和 C/C++ 库的依赖。

cxx_binary(
  name = 'echo',
  srcs = [
    'echo.cpp',
  ],
  headers = [
    'echo.h',
  ],
  deps = [
    ':util',
  ],
)

cxx_library(
  name = 'util',
  srcs = [
    'util.cpp',
  ],
  headers = [
    'util.h',
  ],
)
````

