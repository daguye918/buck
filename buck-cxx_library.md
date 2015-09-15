#cxx_library()

在未来有更改的可能 

一个 cxx_library() 规则控制一组 C/C++ 源文件构建成可执行程序。除此之外，C/C++ 库是相对自身来说抽象的，她们各自依赖与他们顶层的结构 （例如 C/C++ 二进制文件 ， Android APK 安装包）。


## 参数

- `name`（这个参数是必需的）规则的名称。 

- `srcs`（默认为 ［］）整套 C, C++ ，汇编源文件预处理,编译都遵循这个规则。我们通过它们的文件扩展名确定哪些阶段运行在哪个输入源，通过 [GCC文档](https://gcc.gnu.org/onlinedocs/gcc/Overall-Options.html) 可以了解到解读文件扩展名上的更多细节。每个元素可以是一个指定的文件名(如 ‘foo / bar.c’ )或者是一个可以用来指定编译方式的标志（如 （'foo / bar.c ',[' - wall ',' -Werror ']））。在后一种情况下，在预处理和编译该文件（如适用）时，指定的标志将被使用在规则的其他标志上。

- `platform_srcs`（默认为 ［］）平台相关的源文件。这里应该是一个参数列表，第一个参数是一个与平台名称匹配的，不违反安全性的正则化表达式（在 java.util.regex.Pattern syntax 包中）。 第二个参数是源文件的列表或是要预处理的编译标识列表中的一个。在编译和装配时，如果符合正则表达式，请参见 `srcs` 获得更多信息。

- `headers` （默认是 [] ）包含了目标源文件和其依赖之间关系的头文件。它的值应该是一个头文件列表或一个头文件的字典。文件名可以包含正斜杠（/）。头里面可以是 `#include "$HEADER_NAMESPACE/$HEADER_NAME"` 或者是 `#include <$HEADER_NAMESPACE/$HEADER_NAME>` ，其中 `$HEADER_NAMESPACE` 是变量 header_namespace 的值，`$HEADER_NAME` 是头的名字。请查看 `header_namespace` 获取更多信息。

- `platform_headers` （默认是 [] ） 平台相关的头文件。这里应该是一个参数列表，第一个参数是一个与平台名称匹配的，不违反安全性的正则化表达式（在 java.util.regex.Pattern syntax 包中）。第二个参数是源文件的列表或是要预处理的编译标识列表中的一个。请参见 `srcs` 获得更多信息。


- `exported_headers` （默认是 [] ） 他包含在目标源文件和依赖于它的所有头文件的关系信息，这些参数应该被指定为一个头文件列表或一个头文件的字典。头里面可以是 `#include "$HEADER_NAMESPACE/$HEADER_NAME"` 或者是 `#include <$HEADER_NAMESPACE/$HEADER_NAME>` ，其中 `$HEADER_NAMESPACE` 是变量 header_namespace 的值，`$HEADER_NAME` 是头的名字。需要注意的是，文件名可以包含正斜杠（/）。查看 `header_namespace` 获取更多信息。

- `platform_exported_headers` （默认是 [] ） 平台相关的头文件。这里应该是一个参数列表，第一个元素是一个不符合正则表达式（在 java.util.regex.Pattern 包中） 的平台名称，第二个参数是一个头文件的列表或者是一个列表的字典，可能包含在目标源文件和依赖它的平台相关的所有目标文件。查看 `headers` 获取更多信息。


- `header_namespace`（默认为 `name` ）包含此目标的头的路径前缀。默认为目标的名称。可以包含斜杠(/),但不能从一开始就使用。有关更多信息,请参见`headers`。  

- `preprocessor_flags`（默认为 [] ）当预处理任何上述来源文件时标记使用(需要预处理)。  
  
- `platform_preprocessor_flags`（默认为 [] ）平台特定的预处理器标记。这些应该指定为一个参数列表,第一个元素是一个平台名称匹配的，不违反安全性的正则化表达式（在 java.util.regex.Pattern syntax 包中）。第二个元素是列表标记，当预处理目标的来源时使用。有关更多信息,请参见`preprocessor_flags`。  

- `exported_preprocessor_flags`（默认为 [] ） 和 `preprocessor_flags` 一样，在预处理任何上述源（需要预处理）时使用的标志。然而，和 `preprocessor_flags` 不同的是，处理器也会用到这些的标志，这个规则的作用取决于其来源。

- `exported_platform_preprocessor_flags`（默认为 [] ） 平台相关的 `preprocessor flags` 。这里应该是一个参数列表，第一个元素是一个不符合正则表达式（在 java.util.regex.Pattern 包中） 的平台名称，第二个元素是一个预处理时要用到的平台信息的列表。查看 `exported_preprocessor_flags` 获取更多信息。


- `compiler_flags`（默认为 [ ] ）当编译任何上述来源时使用的标记(需要预处理)。 

- `platform_compiler_flags`（默认为 [ ] ）平台特定的编译器标志。这些应该指定为一组对,第一个元素是一个平台名称匹配的，不违反安全性的正则化表达式（在 java.util.regex.Pattern syntax 包中）。第二个元素是列表标记，当编译目标的来源时使用。有关更多信息,请参见`compiler_flags`。  


- `linker_flags`（默认为 [ ] ）无论这个规则是否包络一个连接操作都标记使用（比如链接进一个可执行程序或共享库）。  
  
- `platform_linker_flags `（默认为 [ ] ）平台特定的编译器标志。这些应该指定为一组对,第一个元素是一个平台名称匹配的，不违反安全性的正则化表达式（在 java.util.regex.Pattern syntax 包中）。第二个元素是列表标记，当这个目标包含一个连接操作时使用。有关更多信息,请参见`linker_flagss`。  

- `supported_platforms_regex` （默认为空） 如果不为空，一个不符合正则表达式的的类库支持，将不会被任何一个平台构建。（目前，这仅适用于 android_binary 和 ndk_library .）


- `force_static` （默认为 false ） 如果为 true ，库将会使用静态链接，即使目标指定 link_style 是不是静态的。需要注意的是，如果多个依赖于库的目标连接在一起，会导致重复。

- `tests`（默认为 [ ] ）确定执行此目标的测试规则的生成目标列表。
  
- `visibility `（默认为 [ ] ）确定构建规则的目标模式列表,可以包括`deps`这个规则。  


##例子

````
# 包含一个 .cpp 文件和他的头的规则，也有额外的标记


cxx_library(
  name = 'fileutil',
  srcs = [
    'fileutil.cpp',
  ],
  exported_headers = [
    'fileutil.h',
  ],
  compiler_flags = [
    '-fno-omit-frame-pointer',
  ],
)

# 定义了其头文件的显式名称的规则

cxx_library(
  name = 'mathutils',
  header_namespace = 'math',
  srcs = [
    'trig/src/cos.cpp',
    'trig/src/tan.cpp',
  ],
  exported_headers = {
    # These are included as <math/trig/cos.h> and <math/trig/tan.h>
    'trig/cos.h': 'trig/include/cos.h',
    'trig/tan.h': 'trig/include/tan.h',
  },
  compiler_flags = [
    '-fno-omit-frame-pointer',
  ],
)

# 一种使用不同的头和源文件的规则

cxx_library(
  name = 'vector',
  # Because of platform_headers, this file can include "config.h"
  # and get the architecture specific header
  srcs = ['vector.cpp'],
  platform_srcs = [
    ('.*armv7$', 'armv7.S'),
    ('.*x86_64$', 'x86_64.S'),
  ],
  exported_headers = [
    'vector.h',
  ],
  platform_headers = [
    (
      '.*armv7$',
      {
        'config.h': 'config-armv7.h',
      }
    ),
    (
      '.*x86_64$',
      {
        'config.h': 'config-x86_64.h',
      }
    ),
  ],
)
````