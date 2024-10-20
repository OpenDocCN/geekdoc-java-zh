# Android:与 adb 的连接中断，出现严重错误。

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/android/android-the-connection-to-adb-is-down-and-a-severe-error-has-occurred/>

## 问题

使用 Eclipse IDE 进行 Android 开发时，有时会出现以下“ **adb** ”错误消息，导致应用程序无法启动？

```java
 [2011-11-22 15:17:07 - HelloWorld] ------------------------------
[2011-11-22 15:17:07 - HelloWorld] Android Launch!
[2011-11-22 15:17:07 - HelloWorld] The connection to adb is down, and a severe error has occured.
[2011-11-22 15:17:07 - HelloWorld] You must restart adb and Eclipse.
[2011-11-22 15:17:07 - HelloWorld] Please ensure that adb is correctly located at 
   'C:\Android\android-sdk\platform-tools\adb.exe' and can be executed. 
```

*P.S 重启 Eclipse 不会摆脱错误信息。*

 ## 解决办法

在某些情况下，Android 项目可能会崩溃或异常关闭，并导致“**adb.exe**功能异常。

要解决这个问题，只需重启“**adb.exe**”进程。重启 Eclipse 不会自动关闭“【adb.exe】”进程，你需要手动杀死它。

比如，在 Windows 中，只需使用任务管理器来终止进程。

![adb.exe error](img/2c8a9c1f7311f2f79a055fc23b3955e3.png "android-adb-error")[android](http://web.archive.org/web/20190310101843/http://www.mkyong.com/tag/android/)![](img/d39643de095d3a63c3fec4b9aae2cca5.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190310101843/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="10197">







