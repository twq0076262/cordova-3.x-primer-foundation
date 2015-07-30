# Cordova 3.x 基础（13） -- 为 Android APK 签名

Cordova 编译 Android工程的时候，调用的Android SDK的默认build过程，所以是基于Ant的。 

（1）调试用 APK 

**引用**

```
cordova build android
```

默认是 debug 模式，使用 debug.keystore 来生成以下两个文件： 

- XXXX-debug.apk(signed, unaligned)
- XXXX-debug-unaligned.apk(signed, aligned)


debug.keystore 的位置：   
C:\Documents and Settings\RenSanNing\.android\debug.keystore 

（2）发布用 APK 

**引用**

```
cordova build android --release
```

生成以下三个文件： 

- XXXX-release.apk (signed, aligned)
- XXXX-release-unaligned.apk (signed, unaligned)
- XXXX-release-unsigned.apk (unsigned, unaligned)


如果只生成了 XXXX-release-unsigned.apk，会提示以下错误： 

**引用**

```
[echo] No key.store and key.alias properties found in build.properties. 
[echo] Please sign E:\projects\simpleApp\platforms\android\ant-build\SimplApp-release-unsigned.apk manually
```

使用 JDK 的 keytool 工具生成 keystore 文件： 

**引用**

```
keytool -genkey -v -keystore c:/key/my-release-key.keystore -alias release_alias -keyalg RSA -validity 365
```

参考：[http://rensanning.iteye.com/blog/1462433](http://rensanning.iteye.com/blog/1462433) 

查看 platforms\android\build.xml 文件可知，Cordova 为 build 过程提供了 ant.properties 的接口来变更设置，所以新建 platforms\android\ant.properties 文件后重新 build 即可。 

**引用**

```
key.store=c:/key/my-release-key.keystore 
key.alias=release_alias 
key.store.password=123456 
key.alias.password=123456
```

platforms 下的代码会被生成工程的时候全部删掉，所以要注意保存 ant.properties 文件！