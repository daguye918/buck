# android_manifest()

这个特性在以后可能会有所改动。

android_manifest() 规则用于生成 [android_binary](https://buckbuild.com/rule/android_binary.html) 以及 [android_arr](https://buckbuild.com/rule/android_aar.html) 规则所需要使用的 [Android Manifest](http://developer.android.com/guide/topics/manifest/manifest-intro.html) 文件。该规则使用了 manifest 框架，并且将其与所有 deps 中的 manifest 合并。

## 参数

- name（必要参数）- 规则的名称。
- skeleton （必要参数）- 表明一个[构建目标](https://buckbuild.com/concept/build_target.html)或者表示 manifest 文件的路径是否会合并到与这个规则的 deps 相关的 manifest 中去。
- deps （默认为 []）- 一个依赖的集合，包括 anroid_library  规则等。[android_library](https://buckbuild.com/rule/android_library.html) 规则的 manifest 文件会被筛选出来，并且会作为 manifest 的源依赖文件。
- visibility （默认为 []）- [构建目标样式](https://buckbuild.com/concept/build_target_pattern.html)列表，用于标识能够包含 deps 内规则的构建规则。

## 示例

这是一份没有 deps 的 android_manifest() 示例：

~~~

android_manifest(
  name = 'my-manifest',
  skeleton = 'AndroidManifestSkeleton.xml',
)

~~~

这是 AndroidManifestSkeleton.xml 的格式：

~~~

<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
          package="com.example"
          android:versionCode="1"
          android:versionName="1.0">

    <uses-sdk targetSdkVersion="19" minSdkVersion="17"/>
    <application
        android:label="@string/app_name"
        android:icon="@drawable/ic_launcher">
        <activity
            android:name="MyActivity"
            android:label="@string/app_name">
            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>
                <category android:name="android.intent.category.LAUNCHER"/>
            </intent-filter>
        </activity>
    </application>
</manifest>

~~~

你也可以使用 [genrule](https://buckbuild.com/rule/genrule.html) 来生成 manifest 文件并且在 skeleton 参数中引用 [构建目标](https://buckbuild.com/concept/build_target.html)。