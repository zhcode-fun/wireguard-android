## Endpoint 添加srv记录和txt记录支持

下载：[Release](https://github.com/zhcode-fun/wireguard-android/releases)

+ srv记录：设置端口号为0，例如
  
```
Endpoint = srv.wg.xxxx.com:0
```

+ txt记录：设置端口号为1，例如

```
Endpoint = txt.wg.xxxx.com:1
```
text记录值可以是纯`ip:port`，也可以在后面添加其他备注，以`||`分割，如`1.1.1.1:6789 || 更新于2024-09-12 20:30:00`


---
---

## 自编译

> 前提：
> 只能在linux或macos下构建，建议linux，macos没试过<br/>
> 推荐JDK版本：jdk-17

1. 拉代码

```bash
git clone -b dev-v1.0.20231018.1 https://github.com/zhcode-fun/wireguard-android.git
cd wireguard-android
# 拉取子仓库代码
git submodule init
git submodule update
```

2. 创建jks签名文件，比如创建`buildsign.jks`，放在ui文件夹下：

```bash
keytool -genkey -v -keystore ui/buildsign.jks -keyalg RSA -keysize 2048 -validity 10000 -alias wireguard
```

3. 修改`ui/build.gradle.kts`中的`storePassword`和`keyPassword`

4. 打包命令

```bash
./gradlew clean assembleRelease
```

5. 构建成功后apk路径

```
ui/build/outputs/apk/release/ui-release.apk
```

---
---


# Android GUI for [WireGuard](https://www.wireguard.com/)

**[Download from the Play Store](https://play.google.com/store/apps/details?id=com.wireguard.android)**

This is an Android GUI for [WireGuard](https://www.wireguard.com/). It [opportunistically uses the kernel implementation](https://git.zx2c4.com/android_kernel_wireguard/about/), and falls back to using the non-root [userspace implementation](https://git.zx2c4.com/wireguard-go/about/).

## Building

```
$ git clone --recurse-submodules https://git.zx2c4.com/wireguard-android
$ cd wireguard-android
$ ./gradlew assembleRelease
```

macOS users may need [flock(1)](https://github.com/discoteq/flock).

## Embedding

The tunnel library is [on Maven Central](https://search.maven.org/artifact/com.wireguard.android/tunnel), alongside [extensive class library documentation](https://javadoc.io/doc/com.wireguard.android/tunnel).

```
implementation 'com.wireguard.android:tunnel:$wireguardTunnelVersion'
```

The library makes use of Java 8 features, so be sure to support those in your gradle configuration with [desugaring](https://developer.android.com/studio/write/java8-support#library-desugaring):

```
compileOptions {
    sourceCompatibility JavaVersion.VERSION_17
    targetCompatibility JavaVersion.VERSION_17
    coreLibraryDesugaringEnabled = true
}
dependencies {
    coreLibraryDesugaring "com.android.tools:desugar_jdk_libs:2.0.3"
}
```

## Translating

Please help us translate the app into several languages on [our translation platform](https://crowdin.com/project/WireGuard).
