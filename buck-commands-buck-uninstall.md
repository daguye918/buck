# 卸载 buck

卸载 `[android_binary][1]` 目标的 APK。

需要一个 `[android_binary][2]` 目标,确定应用程序的id,这就意味着要构建，通过运行  `adb uninstall <application_id>` 来卸载：

```
buck uninstall //java/com/myawesomeapp
```

## 参数
无。


  [1]: https://buckbuild.com/rule/android_binary.html
  [2]: https://buckbuild.com/rule/android_binary.html