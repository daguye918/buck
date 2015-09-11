# 共同参数

一些命令行参数影响 Buck 本身的操作,无论哪个子命令正在运行。

```
buck targets --verbose 10
```

## 参数

- `--verbose` 设置控制台输出的详细程度。
- `--no-cache` 禁用通过构建生成的高速缓存。
- `--buildfile:includes` 覆盖.buckconfig中指定的常见includes。
请注意，通过命令行覆盖 `.buckconfig` 的值使重新构建非常困难。首选方法是通过 `.buckconfig.local`  来覆盖 `.buckconfig` 的值。
- `--config `覆盖 .buckconfig 中指定的设置

```
buck build --config cache.mode=dir ...
```

请注意，通过命令行覆盖 `.buckconfig` 的值使重新构建非常困难。首选方法是通过 `.buckconfig.local`  来覆盖 `.buckconfig` 的值。


