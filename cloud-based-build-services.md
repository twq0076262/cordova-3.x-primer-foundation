# Cordova 3.x 基础（15） -- 云端 Cordova 
通过 Web 界面上传 HTML/CSS/Javascript 源代码后，在云环境（ICE）中把这些代码转换成不同平台的 app。以下简单试用了一下 PhoneGap Build、AppBuilder、Appery.io、Monaca、SAE 云窗调试器等5个服务。 

- 编译出来的 APK 文件除过 Monaca 获取的权限太多无法安装、SAE 云窗调试器只是调试工具，基本都能够很好的实现自动化编译。
- Appery.io 编译出来的 APK 文件最大2.3 M、PhoneGap Build 编译出来的最小210 K。
- PhoneGap Build 只是打包所以操作最简单。
- AppBuilder（Icenium）在各个方面都表现良好。

**（1）PhoneGap Build [https://build.phonegap.com/](https://build.phonegap.com/)** 

①可以通过 Git 地址或者 zip 文件上传代码 

![picture15.1](images/15.1.png)

②上传代码 

![picture15.2](images/15.2.png)

③点击“Ready to Build” 

![picture15.3](images/15.3.png)

④左下角各个平台开始编译 

![picture15.4](IMAGES/15.4.PNG)

⑤红色代表有错误，蓝色代表编译成功，点击成功的图标即可下载 app 

![picture15.5](images/15.5.png)

⑥点击项目名“PG Build App”进入应用详细 

![picture15.6](images/15.6.png)

同时 Phonegap Build 被集成到了 Adobe 的其他产品中，比如 Dreamweaver CC、Edge Code CC 中。相关的 Key 需要提前设置，比如：在 Apple 的开发中心创建认证文件，Cert File->p12 File->App ID->Device ID->Provisioning profile。然后在 Phonegap 的 Account 中，在 Signing Keys 中“add a key”选择 p12和 mobileprovision 文件。具体参考：[http://docs.build.phonegap.com/en_US/signing_signing-ios.md.html](http://docs.build.phonegap.com/en_US/signing_signing-ios.md.html) 


**（2）AppBuilder（Icenium）  http://www.telerik.com/appbuilder** 

①创建项目 

![picture15.7](images/15.7.png)

②IDE 主界面 

![picture15.8](images/15.8.png)

③选择菜单 Add > Add from Archive 

![picture15.9](images/15.9.png)

④确认上传文件的位置及内容 

![picture15.10](images/15.10.png)

⑤选择菜单 Run > build 

![picture15.11](images/15.11.png)

⑥build 成功后即可下载 app 

![picture15.12](images/15.12.png)

**（3）Appery.io（Tiggzi）  [http://appery.io/](http://appery.io/)** 

①Dashboard 主界面

![picture15.13](images/15.13.png)

②创建项目 

![picture15.14](images/15.14.png)

③IDE 主界面 

![picture15.15](images/15.15.png)

④App 设置中可以看到使用的 PhoneGap3.3.0 

![picture15.16](images/15.16.png)

⑤不能上传文件，所以可以通过"Create New"来新建页面

![picture15.17](images/15.17.png)

⑥从菜单“Export”导出 app 

![picture15.18](images/15.18.png)

⑦导出中 

![picture15.19](images/15.19.png)

⑧导出完成后即可下载 app 

![picture15.20](images/15.20.png)

**（4）Monaca [http://monaca.mobi/](http://monaca.mobi/)** 

①Dashboard 主界面 

![picture15.21](images/15.21.png)

②创建项目（有很多模板可以选择）

![picture15.22](images/15.22.png)

③选择一个模板后输入项目名称 

![picture15.23](images/15.23.png)

④项目创建完成 

![picture15.24](images/15.24.png)

⑤点击“Launch IDE”进入 IDE 主界面

![picture15.25](images/15.25.png)

⑥选择 Build  

![picture15.26](images/15.26.png)

⑦Build 中

![picture15.27](images/15.27.png)

⑧Build 完成后可下载 app 

![picture15.28](images/15.28.png)

**（5）SAE 云窗调试器 [http://sae.sina.com.cn/?m=mobile](http://sae.sina.com.cn/?m=mobile)** 

①需要在 SAE 应用管理中心新建一个移动应用的托管后编辑代码 

![picture15.29](images/15.29.png)

②修改完代码后可以在 移动应用 > 应用打包 处 build 

![picture15.30](images/15.30.png)

③Build 成功后可下载 app 

![picture15.31](images/15.31.png)

④安装的云窗户调试器 

![picture15.32](images/15.32.png)

[Popular ICEs for mobile hybrid app development](http://www.developereconomics.com/popular-ices-mobile-hybrid-app-development/) 





























