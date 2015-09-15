#rust_library()

未来可能被改变

rust_library() 规则可以将 Rust 源代码和其依赖构建成原生库。

注：Buck 目前正在测试（因此支持）版本 1.1.0 的 Rust 。


#参数

- `name` （必须要有的）规则的名字。

- `srcs` （必须要有的）被编译的 Rust 源文件。

这些文件的其中一个必须要被命名为 {name}.rs 或者 main.rs 。作为程序的入口。


- `deps` （默认为 []）这个规则的依赖列表，目前，这只支持 rust_library 的规则。

- `features` （默认为 []）

为该规则启用的功能的集合。

这些都应该通过 rustc 的 --cfg feature="{feature}" 验证，可在代码中使用 #[cfg(feature = "{feature}")]。

- `rustc_flags` （默认为 [] ）未来通过 rustc 的额外的编译器标识。







#例子

获取更多例子，请到[这里](https://github.com/facebook/buck/tree/master/test/com/facebook/buck/rust/testdata/)

````
rust_library(
  name='greeting',
  srcs=[
    'greeting.rs',
  ],
  deps=[
    ':join',
  ],
)
````