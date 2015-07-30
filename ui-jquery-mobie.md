# Cordova 3.x 基础（9） -- UI 框架 jQuery Mobile

目前 Version 1.4.1，这里只是做个摘要，官方的 Demos 有很详细的使用说明。   
[http://demos.jquerymobile.com/1.4.1/](http://demos.jquerymobile.com/1.4.1/)   
[http://api.jquerymobile.com/](http://api.jquerymobile.com/)  

## （1）引入 

Html **代码 ** 

```
<!DOCTYPE html>

<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />

<link rel="stylesheet" type="text/css" href="lib/jquery.mobile/jquery.mobile-1.4.1.min.css" />
<script type="text/javascript" src="lib/jquery/jquery-1.11.0.min.js"></script>
<script type="text/javascript" src="lib/jquery.mobile/jquery.mobile-1.4.1.min.js"></script>
```

## （2）基本构造 

单页面 

Html **代码**

```
<div data-role="page"> 
  <div data-role="header">...</div> 
  <div role="main" class="ui-content">...</div> 
  <div data-role="footer">...</div> 
</div>
```

***1.4之前主体部分使用「data-role="content"」 

多页面 

**Html 代码**

```
<div data-role="page" id="first">
  <div data-role="header">...</div> 
  <div role="main" class="ui-content">...</div> 
  <div data-role="footer">...</div> 
</div>
<div data-role="page" id="second"> 
  <div data-role="header">...</div> 
  <div role="main" class="ui-content">...</div> 
  <div data-role="footer">...</div> 
</div>
```

在早期版本中为了提高页面跳转的效率，在一个文件中定义多个页面，通过「href="#ID名"」页面跳转。默认显示文件中定义的第一个 page，如果要自定义初期显示其他 page 的话 

Js **代码**  

```
if (document.location.hash == "")
     document.location.hash = "#second";
```

但是这样也就加重了 HTML 的 load 速度，所以现在基本都是1个文件1个页面。 

## （3）主题 Theme 

除过一些特殊的 Widget（比如 ListView）需要特殊设置外，大部分 Widget 都可以通过 data-theme 来修改主题。1.4之前提供 a、b、c、d、e 五种主题，从1.4开始做了简化。官方还提供了自定义主题的 ThemeRoller for jQuery Mobile：http://themeroller.jquerymobile.com/ 

## （4）Header/Footer 

首先 Header 和 Footer 都不是必须元素，在需要的是时候添加即可。 

Header   
只有标题 

Html **代码**

```
<div data-role="header">
    <h1>Title</h1>
</div>
```

左按钮 

Html **代码**

```
<div data-role="header" data-theme="b">
  <a href="#" class="ui-btn ui-btn-a ui-btn-left">Left</a>
  <h1>Title</h1>
</div>
```

右按钮 

Html **代码**

```
<div data-role="header" data-theme="b">
  <h1>Title</h1>
  <a href="#" class="ui-btn ui-btn-a ui-btn-right">Right</a>
</div>
```

1.4之前按钮是先左后右放置，跟代码写在什么地方没有关系。现在的版本希望 class 属性明确指定位置。 

左右按钮 

Html **代码**

```
<div data-role="header" data-theme="b">
  <a href="#" class="ui-btn ui-btn-a ui-btn-left">Left</a>
  <h1>Title</h1>
  <a href="#" class="ui-btn ui-btn-a ui-btn-right">Right</a>
</div>
```

回退按钮 

Html **代码**

```
<div data-role="header" data-add-back-btn="true" data-back-btn-text="Back">
    <h1>jQuery Mobile TIPS</h1>
    <a href="help.htmll" class="ui-btn ui-btn-a ui-btn-right">About</a>
</div>
```

1.4之前回退按钮需要通过&lt;a&gt;元素来实现。 

全局回退按钮有效设置：
 
Js **代码** 

```
$(document).on('mobileinit', function() {
  $.mobile.toolbar.prototype.options.addBackBtn = true;
  $.mobile.toolbar.prototype.options.backBtnText = 'Back';
});
```

data-position="fixed" 固定位置   
data-fullscreen="true" 页面 Tap 的时候隐藏 
data-id 属性 页面跳转的时候只会滑动 content 部分 

## （5）链接 Link 

Html **代码**

```
<a href="http://www.wings.msn.to/" class="ui-btn">...</a>
<a href="basic.html" target="_blank" class="ui-btn">...</a>
<a href="basic.html" rel="external" class="ui-btn">...</a>
<a href="basic.html" data-ajax="false" class="ui-btn">...</a>
```

同一 Domain 下，link 默认使用 ajax 加载。 

data-transition="slide" 跳转动画 

默认跳转动画 

Js **代码**

```
$(document).bind("mobileinit", function(){ 
   $.mobile.defaultTransition = "slidedown";
})
```

## （6）按钮 Button 

显示一个按钮，有&lt;button&gt;、&lt;a&gt;、&lt;input&gt;三种方式。 

对于&lt;button&gt;和&lt;a&gt 

Html **代码**

```
<button class="ui-btn">...</button>
<a href="#" class="ui-btn">...</a>
```

1.4以前使用「data-role="button"」 

ui-btn-inline 紧凑   
ui-corner-all 圆角   
ui-shadow 阴影   
ui-btn-* 变更主题   
ui-mini 最小化   

对于&lt;input&gt;
<br />&lt;input type="button" value="..." /&gt;
<br />&lt;input&gt;稍微不同，需要通过data-xxxxx属性来设置样式。
<br />data-inline、data-corners、data-shadow、data-theme、data-mini
   

按钮组
 
Html 代码 

```
<div data-role="controlgroup">
  <button class="ui-btn">...</button>
  <a href="index.html" class="ui-btn">...</a>
  <input type="button" value="..." />
</div>
```

水平放置按钮 

Html **代码**

```
<div data-role="controlgroup" data-type="horizontal">...</div>
```

## (7)导航 Navbar 

Html **代码**

```
<div data-role="navbar">
<ul>
  <li><a href="#" data-icon="grid">Summary</a></li>
  <li><a href="#" data-icon="star" class="ui-btn-active">Favs</a></li>
  <li><a href="#" data-icon="gear">Setup</a></li>
</ul>
</div>
```

## （8）列表 Listview 

一般 
  
Html **代码**

```
<ul data-role="listview">
    <li><a href="#">Acura</a></li>
    <li><a href="#">Audi</a></li>
    <li><a href="#">BMW</a></li>
    <li><a href="#">Cadillac</a></li>
    <li><a href="#">Ferrari</a></li>
</ul>
```

分组 

Html  **代码**

```
<ul data-role="listview" data-inset="true">
    <li data-role="list-divider">Mail</li>
    <li><a href="#">Inbox</a></li>
    <li><a href="#">Outbox</a></li>
    <li data-role="list-divider">Contacts</li>
    <li><a href="#">Friends</a></li>
    <li><a href="#">Work</a></li>
</ul>
```

## （9）左右滑动菜单 Panel 

Html **代码**

```
<div data-role="page">
    <div data-role="header" data-position="fixed">
        <h1>Fixed header</h1>
        <a href="#nav-panel" data-icon="bars" data-iconpos="notext">Menu</a>
        <a href="#add-form" data-icon="gear" data-iconpos="notext">Add</a>
    </div>
    <div role="main" class="ui-content jqm-content jqm-fullwidth">
       //......
    </div>
    <div data-role="footer" data-position="fixed">
        <h4>Fixed footer</h4>
    </div>
    <div data-role="panel" data-position-fixed="true" data-display="push" id="nav-panel">
        //......
    </div>
    <div data-role="panel" data-position="right" data-position-fixed="true" data-display="overlay" id="add-form">
       //......
    </div>
</div>
```




