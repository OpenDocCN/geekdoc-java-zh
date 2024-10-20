# Mac OS X 上的 GAE + Python hello world

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/google-app-engine/gae-python-hello-world-on-mac-os-x/>

在本教程中，我们将向您展示如何在 Mac OS X 上使用 Python 创建一个简单的 GAE hello world web 项目，并通过 Google App Engine Launcher 运行它。

使用的工具:

1.  用于 Python 的谷歌应用引擎 SDK(Mac OS X)-1 . 7 . 0
2.  苹果 OS X 10.8
3.  Python 2.7

**Note**
By default, Mac OS X 10.8, has Python 2.7 installed, which makes Google App Engine development more easier.

## 1.谷歌应用引擎 SDK

访问这个[Google App Engine SDK for Python](http://web.archive.org/web/20221225035504/https://developers.google.com/appengine/downloads#Google_App_Engine_SDK_for_Python)，选择 Mac OS X 并开始下载。

*1.1 安装 Google App Engine SDK*
双击下载的`GoogleAppEngineLauncher-version.dmg`文件，会提取出“ **GoogleAppEngineLauncher** ”图标，拖到你希望 GAE SDK 安装的文件夹中。

*1.2 再次运行 Google App Engine Launcher*
，双击“ **GoogleAppEngineLauncher** ”图标，按照向导提示完成安装。

*图:Google appengine launcher*–这个 GAE 启动器帮助你运行、部署和管理你的应用程序。

![gae launcher example](img/f2b067e8d7880abca65c6ffe68c0c5a1.png "GoogleAppEngineLauncher-0-1")

## 2.Python Hello World

*File:hello . py*–创建一个简单的 python 文件来显示 hello world 消息。

```java
 import webapp2

class MainPage(webapp2.RequestHandler):
  def get(self):
      self.response.headers['Content-Type'] = 'text/plain'
      self.response.out.write('Hello World, GAE + Python')

app = webapp2.WSGIApplication([('/', MainPage)], debug=True) 
```

*文件:app . YAML*–创建一个简单的 GAE 配置文件。

```java
 application: helloworld
version: 1
runtime: python27
api_version: 1
threadsafe: true

handlers:
- url: /.*
  script: hello.app 
```

完成了。

## 3.导入、运行和演示

在 GAE 启动程序中，两个手指点击表格网格->选择“**添加已有…** ”，找到上面包含 Python 文件的文件夹。

![gae launcher add existing project](img/7ea5bc94688d4c2a8af13a5b69776f57.png "GoogleAppEngineLauncher-0-2")

运行它并点击**浏览**以查看部署的 web 应用程序。

![gae launcher](img/dc54bf665a4a2220aa4bb17c17f6504e.png "GoogleAppEngineLauncher-0-3")

*见演示:http://localhost:8888*

![result](img/f669cd513b1e3635a51ae5d09dd3ca39.png "demo")

## 下载源代码

Download it – [gae-python-hello-world.zip](http://web.archive.org/web/20221225035504/http://www.mkyong.com/wp-content/uploads/2012/08/gae-python-hello-world.zip) (3 kb)

## 参考

1.  [GAE 入门:Python 2.7](http://web.archive.org/web/20221225035504/https://developers.google.com/appengine/docs/python/gettingstartedpython27/)
2.  [在 Mac OS X 10.6 上使用谷歌应用引擎 SDK 和 Python 2.7](http://web.archive.org/web/20221225035504/https://stackoverflow.com/questions/8127696/using-google-app-engine-sdk-with-python-2-7-on-mac-os-x-10-6)

<input type="hidden" id="mkyong-current-postId" value="11398">