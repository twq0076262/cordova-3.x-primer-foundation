# Cordova 3.x 基础（11） -- JS 是如何调用本地 API 的？

Cordova 应用基于 Webview，所以后台代码和 js  交互都是基于 Webview（Webkit）的接口的。 

Android：WebView（WebKit-based） WebView(4.4 Chromium-based)  Updatable-WebViews（5+） 
@JavascriptInterface/WebView#addJavascriptInterface()   
参考源码 [ExposedJsApi.java](https://github.com/apache/cordova-android/blob/master/framework/src/org/apache/cordova/ExposedJsApi.java) 

iOS：UIWebView（iOS 4+） WKWebView(iOS 8.1+) 
UIWebViewDelegate/UIWebView#stringByEvaluatingJavaScriptFromString()   
参考源码 [CDVWebViewDelegate.m](https://github.com/apache/cordova-ios/blob/master/CordovaLib/Classes/CDVWebViewDelegate.m)


![picture11.1](images/11.1.png)

以下以 Android 调用照相机为例，简单说明一下调用及回调过程。 

**(1)创建的过程**

①添加插件 

**引用**

```
cordova plugin add org.apache.cordova.camera
```

在 plugins 的目录下创建 org.apache.cordova.camera 文件夹，并将该 Plugin 的所有代码 Copy 进去，具体代码依赖关系都记录在 plugin.xml 里。 

②创建 Android 工程 

**引用**

```
cordova platform add android
```

从上边的 Plugin 文件夹中把 Java 文件和 js 文件 Copy 到 Android 工程的相应的文件夹下。 

- platforms\android\src\org\apache\cordova\camera\CameraLauncher.java 等  
- platforms\android\assets\www\plugins\org.apache.cordova.camera\www\Camera.js 等

其中 CameraLauncher 扩展自 CordovaPlugin，而 CordovaPlugin 定义在 platforms\android\CordovaLib 中，它是 Cordova 的基础框架代码。 
 
**（2）调用的过程（JS->Native）** 

①HTML 中引入 cordova.js 

**引用**

```
<script type="text/javascript" src="cordova.js"></script>
```

先做初始化处理，后根据 cordova_plugins.js 加载所有 plugin 的 js 文件。 

②在 deviceready 事件中调用 Camera 

Js **代码**

```
navigator.camera.getPicture(onSuccess, onFail,   
          { quality: 50,   
            allowEdit: true,   
            destinationType: destinationType.DATA_URL });
```


③调用 Camera.js 的 getPicture 方法 

```
assets\www\plugins\org.apache.cordova.camera\www\Camera.js 

getPicture() 
-> 
exec(successCallback, errorCallback, "Camera", "takePicture", args) 
-> 
function androidExec(success, fail, service, action, args) ***cordova.js 
-> 
var messages = nativeApiProvider.get().exec(service, action, callbackId, argsJson); 
```

④调入 Java 的 exec()方法 

在 CordovaWebView 初期化的时候会根据 Android 的版本，将 ExposedJsApi 对象添加到 CordovaWebView 中。 
this.addJavascriptInterface(exposedJsApi, "_cordovaNative");   
所以 nativeApiProvider.get()的时候会根据 _cordovaNative 对象是否存在来判断是使用JavascriptInterface 方式，还是使用 prompt 方式。 

如果使用 JavascriptInterface 方式（Android 4.2以上版本），直接进入 ExposedJsApi.java 中定义了@JavascriptInterface 标示的 exec()方法 。 

如果使用 prompt 方式，CordovaChromeClient.java 中重写了 onJsPrompt()方法，来调用 exposedJsApi.exec（）。  
prompt(argsJson, 'gap:'+JSON.stringify([service, action, callbackId])); 

总之入口都是 exposedJsApi.exec(). 

@JavascriptInterface 

```
public String exec(String service, String action, String callbackId, String arguments) 
-> 
pluginManager.exec(service, action, callbackId, arguments); 
```

PluginManager 根据 service 调用获取到相应的 CordovaPlugin 

```
-> 
CameraLauncher.execute(String action, JSONArray args, CallbackContext callbackContext)
```
 
CameraLauncher 根据 action，这里是“takePicture”做本地 API 调用。 

```
-> 
takePicture() 
-> 
new Intent("android.media.action.IMAGE_CAPTURE"); 
cordova.startActivityForResult() 
```

调用 CordovaInterface(CordovaActivity->Activity)的 startActivityForResult 

（３）回调的过程（Native->JS） 

①上述 API 调用成功后，在 onActivityResult(CameraLauncher.java)设置结果   
onActivityResult(int requestCode, int resultCode, Intent intent) 

// Send Uri back to JavaScript for viewing image 
this.callbackContext.success(uri.toString()); 

②退回到 ExposedJsApi 的 exec()方法 

```
jsMessageQueue.setPaused(false); 
-> 
NativeToJsMessageQueue的onNativeToJsMessageAvailable() 
-> 
sendMessageMethod = webViewCore.getClass().getDeclaredMethod("sendMessage", Message.class) 
-> 
sendMessageMethod.invoke(webViewCore, execJsMessage) 
```

③cordova.js 中接收消息 

```
androidExec.processMessages(messages) 
-> 
processMessage(message) 
-> 
cordova.callbackFromNative(callbackId, success, status, [payload], keepCallback);
```
 
调用定义好的成功或者失败的 JS 回调函数。（payload 为回传值） 

以上就是完整的过程。 

参考：   
[http://blog.devtang.com/blog/2012/03/24/talk-about-uiwebview-and-phonegap/](http://blog.devtang.com/blog/2012/03/24/talk-about-uiwebview-and-phonegap/)