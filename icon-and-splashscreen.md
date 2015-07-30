# Cordova 3.x 基础（2） -- 应用图标 icon 和启动页面 SplashScreen


========================================================== 

最新版 Cordova CLI 已经支持在 config.xml 中配置<splash> 和 <icon>，CB-2606, CB-3571 Add support for <icon>, <splash>。设置如下： 

Xml **代码** 

```
<platform name="android">
    <icon src="res/android/icon-36-ldpi.png" density="ldpi" />
    <splash src="res/android/screen-xhdpi-portrait.png" density="port-xhdpi"/>
</platform>
```

具体可以参考官方文档：[Icons and Splash Screens](http://cordova.apache.org/docs/en/edge/config_ref_images.md.html#Icons%20and%20Splash%20Screens) 

如果你本地安装了 ImageMagick，CLI 还会自动调用 ImageMagick 来做成不同尺寸的图像。 
需要注意的是：   
（1）图像的路径是相对于工程根目录不是根据 www   
（2）不要和 PhoneGap CLI 的设置混淆   

========================================================== 

##（1）创建以下图像 

以 Android 为例： 

icon 图像： 

- www/res/icon/android/icon-36-ldpi.png 36px x 36px
- www/res/icon/android/icon-48-mdpi.png 48px x 48px
- www/res/icon/android/icon-72-hdpi.png 72px x 72px
- www/res/icon/android/icon-96-xhdpi.png 96px x 96px

splashscreen 图像： 

- www/res/screen/android/screen-ldpi-landscape.png 320px x 200px
- www/res/screen/android/screen-mdpi-landscape.png 480px x 320px
- www/res/screen/android/screen-hdpi-landscape.png 800px x 480px
- www/res/screen/android/screen-xhdpi-landscape.png 1280px x 720px
- www/res/screen/android/screen-ldpi-portrait.png 200px x 320px
- www/res/screen/android/screen-mdpi-portrait.png 320px x 480px
- www/res/screen/android/screen-hdpi-portrait.png 480px x 800px
- www/res/screen/android/screen-xhdpi-portrait.png 720px x 1280px


## （2）给工程添加 splashscreen 插件 

**引用**

> cordova plugin add org.apache.cordova.splashscreen

## （3）配置 config.xml 

Xml **代码** 

```
<preference name="SplashScreen" value="splash" /> <!-- 不带后缀png的文件名，默认是screen-->
<preference name="SplashScreenDelay" value="10000" /> <!-- Splash显示时间，默认是3000ms-->

<feature name="SplashScreen">
  <param name="android-package" value="org.apache.cordova.splashscreen.SplashScreen" />
</feature>
```

相比于在 config.xml 中设置延迟时间，更应该在设备初始化完成后隐藏 Splash 画面(index.html)

Js **代码** 

```
document.addEventListener("deviceready", onDeviceReady, false);
function onDeviceReady() {
  navigator.splashscreen.hide();
}
```

## （4）Copy 文件 

phonegap 是可以在 config.xml 中配置 icon 和 splash 的，但是 cordova 不支持，需要在 cordova build 之后 Copy 文件，也可以通过创建 hook 来实现。   
***phonegap 的配置也局限于 remote build 的时候 Phonegap Build 起作用，local build 也不起作用，需手动处理。参考这里。 

icon.png：   
把 www/res/icon/android 下的文件 Copy 到相应的 platforms/android/res/drawable*/下。 

splash.png：   
把 www/res/screen/android 下的文件 Copy 到相应的 platforms/android/res/drawable*/下。 

完成以后启动“cordova emulate android”即可。 

**phonegap 的 config.xml **

Xml **代码**

```
<!-- Define app icon for each platform. -->
<icon src="icon.png" />
<icon src="res/icon/android/icon-36-ldpi.png"   gap:platform="android"    gap:density="ldpi" />
<icon src="res/icon/android/icon-48-mdpi.png"   gap:platform="android"    gap:density="mdpi" />
<icon src="res/icon/android/icon-72-hdpi.png"   gap:platform="android"    gap:density="hdpi" />
<icon src="res/icon/android/icon-96-xhdpi.png"  gap:platform="android"    gap:density="xhdpi" />
<icon src="res/icon/blackberry/icon-80.png"     gap:platform="blackberry" />
<icon src="res/icon/blackberry/icon-80.png"     gap:platform="blackberry" gap:state="hover"/>
<icon src="res/icon/ios/icon-57.png"            gap:platform="ios"        width="57" height="57" />
<icon src="res/icon/ios/icon-72.png"            gap:platform="ios"        width="72" height="72" />
<icon src="res/icon/ios/icon-57-2x.png"         gap:platform="ios"        width="114" height="114" />
<icon src="res/icon/ios/icon-72-2x.png"         gap:platform="ios"        width="144" height="144" />
<icon src="res/icon/webos/icon-64.png"          gap:platform="webos" />
<icon src="res/icon/windows-phone/icon-48.png"  gap:platform="winphone" />
<icon src="res/icon/windows-phone/icon-173.png" gap:platform="winphone"   gap:role="background" />

<!-- Define app splash screen for each platform. -->
<gap:splash src="res/screen/android/screen-ldpi-portrait.png"  gap:platform="android" gap:density="ldpi" />
<gap:splash src="res/screen/android/screen-mdpi-portrait.png"  gap:platform="android" gap:density="mdpi" />
<gap:splash src="res/screen/android/screen-hdpi-portrait.png"  gap:platform="android" gap:density="hdpi" />
<gap:splash src="res/screen/android/screen-xhdpi-portrait.png" gap:platform="android" gap:density="xhdpi" />
<gap:splash src="res/screen/blackberry/screen-225.png"         gap:platform="blackberry" />
<gap:splash src="res/screen/ios/screen-iphone-portrait.png"    gap:platform="ios"     width="320" height="480" />
<gap:splash src="res/screen/ios/screen-iphone-portrait-2x.png" gap:platform="ios"     width="640" height="960" />
<gap:splash src="res/screen/ios/screen-ipad-portrait.png"      gap:platform="ios"     width="768" height="1024" />
<gap:splash src="res/screen/ios/screen-ipad-landscape.png"     gap:platform="ios"     width="1024" height="768" />
<gap:splash src="res/screen/windows-phone/screen-portrait.jpg" gap:platform="winphone" />
```

