# easyprint.js

#### 介绍
easyprint.js，比webprinter.js更快速接入WebPrinter打印控件的客户端
easyprint.js需要依赖webprinter.js，关于webprinter.js的使用请[https://www.webprinter.cn/doc/jsapi](参考这里)。

#### 关于WebPrinter和easyprint
WebPrinter是面向互联网的浏览器打印控件，满足多种场景下的网页打印需求。为电商、物流及服务型机构等众多行业提供一站式打印解决方案。[https://www.webprinter.cn/](WebPrinter官网)
使用webprinter.js已拥有了WebPrinter的JSAPI能力，easyprint.js在webprinter.js基础上做了友好型封装，可使用easyprint API快速开发打印程序。
相比于webprinter.js，easyprint.js在如下方面做了增强：  
1. 新的Fluent API具备更高的可读性
2. 简化了控件生命周期的管理，开发者可聚焦于打印内容和配置本身，而无需关注控件的启停状态
3. 基于队列的任务管理，确保页面并发发送任务至控件的先后顺序

#### 引入
WebPrinter已提供js的cdn地址，可在页面中直接引入：  
```html
<script type="text/javascript" src="https://get.webprinter.cn/default/5.2.0/webprinter.js">
<script type="text/javascript" src="https://get.webprinter.cn/default/5.2.0/easyprint.js">
```

说明：
5.0.2为版本号，请移步[https://www.webprinter.cn/](官网)查看最新版本号。

1. 获取easy实例
可通过
```javascript
webprinter.easy()
```
随时获取easyprint实例，且easyprint是单例的

2. 创建Paper、Config、Task
```javascript
//创建paper
var paper=webprinter.easy().paper().with_width(97).with_height(24);
//创建config
var config=webprinter.easy().config().with_orientation(CONSTANTS.CONFIG_ORIENTATION.PORTRAIT).with_color(CONSTANTS.CONFIG_COLOR.COLOR).with_side(CONSTANTS.CONFIG_SIDE.ONESIDE).with_collate(CONSTANTS.CONFIG_COLLATE.UNCOLLATE).with_copies(1).with_zero_margins().with_paper(paper);
//创建task
var task=webprinter.easy().task().with_printer(printer).with_config(config);
//html任务
task.with_html(html);
//url任务
task.with_url(url);
//发送至WebPrinter
webprinter.easy().print(task);
```

#### 关键方法
1. webprinter.easy().paper()
创建页面设置
2. webprinter.easy().config()
创建一个打印任务设置
3. webprinter.easy().task()
创建打印任务
4. webprinter.easy().print(task)
发送任务至WebPrinter
5. webprinter.easy().enableDebug()
启用调试日志输出(至console)
6. webprinter.easy().disableDebug()
禁用调试日志输出(至console)
7. webprinter.easy().wpInstance()
得到Strato.WebPrinter.getInstance()单例对象。参考[https://www.webprinter.cn/doc/jsapi](WebPrinter JSAPI)。
8. webprinter.easy().constants()
得到easyprint的所有常量。
可通过
```javascript
webprinter.easy().constants().dump()
```
打印到console。
9. webprinter.easy().ready(callback)
当Strato.WebPrinter首次连接至控件时调用。
callback的入参为wp实例(等同于 webprinter.easy().wpInstance() )。
例如：
```javascript
webprinter.easy().ready(function(wp){
    console.log("wp",wp);
})
```
10. webprinter.easy().observePrinters(callback)
监听打印机列表的变化。
callback的入参为打印机名称列表和默认打印机名称。
例如：
```javascript
webprinter.easy().observePrinters(function(printers,defaultPrinter){
    console.log("All printers",printers);
    console.log("Default printer",defaultPrinter)
})
```
11. webprinter.easy().observeConnection(callback)
监听Web页面和打印控件的连接状态，callback参数为是否已连接connected和控件信息info，当connected==false时，info为null。
例如：
```javascript
webprinter.easy().observeConnection(function(connected,info){
    console.log("Connected",connected);
    if(connected){
        console.log("WebPrinter Information",info);
    }
})
```
12. webprinter.easy().observeTasks(callback)
监听任务列表变化情况。callback参数为任务列表。
例如：
```javascript
webprinter.easy().observeTasks(function(tasks){
    console.log("Tasks",tasks)
})
```
注：目前采用轮询方式监听。

#### 示例
具体实现，请参阅[webprinter-easyprint.js](webprinter-easyprint.js源码)。
参考[webprinter-easyprint-test.html](webprinter-easyprint-test.html)

