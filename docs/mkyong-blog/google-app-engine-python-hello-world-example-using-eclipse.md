# 使用 Eclipse 的 Google app engine Python hello world 示例

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/google-app-engine/google-app-engine-python-hello-world-example-using-eclipse/>

在本教程中，我们将向您展示如何使用 **Eclipse** 创建一个**Google App Engine**(GAE)**Python**web 项目(hello world 示例)，在本地运行它，并将其部署到 Google App Engine 帐户。

使用的工具:

1.  Python 2.7
2.  Eclipse 3.7 + PyDev 插件
3.  用于 Python 1.6.4 的谷歌应用引擎 SDK

*P.S 假设安装了 Python 2.7 和 Eclipse 3.7。*

## 1.为 Eclipse 安装 PyDev 插件

使用下面的 URL 安装 [PyDev 作为 Eclipse 插件](http://web.archive.org/web/20221225035511/http://pydev.org/)。

```java
 http://pydev.org/updates 
```

*图 1*——在 Eclipse 菜单中，“帮助—>安装新软件。”并放在上面的网址。选择“ **PyDev for Eclipse** ”选项，按照步骤操作，并在完成后重启 Eclipse。

![pydev eclipse](img/da80d8c0491ec107f95fcf7179975e61.png "python-gae-pydev-eclipse")

## 2.验证 PyDev

Eclipse 重启后，确保 **PyDev 的解释器**指向你的`python.exe`。

*图 2*-Eclipse->-Windows->首选项，确保“**解释器-Python**配置正确。

![pydev eclipse config](img/32210c80a6c62a40131a4022dbcfe0ca.png "python-gae-pydev-eclipse-config")

## 3.谷歌应用引擎 SDK Python

下载并安装[Google App Engine SDK for Python](http://web.archive.org/web/20221225035511/https://developers.google.com/appengine/downloads#Google_App_Engine_SDK_for_Python)。

## 4.Eclipse 中的 Python Hello World

下面的步骤向你展示了如何通过 Pydev 插件创建一个 GAE 项目。

*图 4.1*–Eclipse 菜单，文件- >新建- >其他…，PyDev 文件夹，选择“ **PyDev Google App Engine 项目**”。

![gae python hello world example](img/90fc5ef49dea02ccf42d4c4dcd92a783.png "gae-eclipse-python-hello-world-1")

*图 4.2*–键入项目名称，如果解释器尚未配置(在步骤 2 中)，您现在可以这样做。并选择此选项—**“创建‘src’文件夹并将其添加到 PYTHONPATH** ”。

![gae python hello world example](img/36e8a1d7b885125b7edff28e000fd6e4.png "gae-eclipse-python-hello-world-2")

*图 4.3*–点击“浏览”按钮，指向 Google App Engine 安装目录(在步骤 3 中)。

![gae python hello world example](img/a177a3b93638b819b2ff09d73a9402e3.png "gae-eclipse-python-hello-world-3")

*图 4.4*–在 GAE 命名您的应用程序 id，键入任何内容，您可以稍后更改。并选择“ **Hello Webapp World** 模板生成示例文件。

![gae python hello world example](img/b2be4da35329debfd635c01747b08c6d.png "gae-eclipse-python-hello-world-4")

*图 4.5*–完成，生成 4 个文件，`.pydevproject`、`.project`都是 Eclipse 项目文件，忽略。

![gae python hello world example](img/35068573a11bc2572856351f11adffe5.png "gae-eclipse-python-hello-world-5")

查看生成的 Python 文件:

*File:helloworld . py*——只输出一个 hello world。

```java
 from google.appengine.ext import webapp
from google.appengine.ext.webapp.util import run_wsgi_app

class MainPage(webapp.RequestHandler):

    def get(self):
        self.response.headers['Content-Type'] = 'text/plain'
        self.response.out.write('Hello, webapp World!')

application = webapp.WSGIApplication([('/', MainPage)], debug=True)

def main():
    run_wsgi_app(application)

if __name__ == "__main__":
    main() 
```

*文件:app . YAML*–GAE 需要这个文件来运行和部署你的 Python 项目，它非常简单明了，详细的语法和配置，请访问 [yaml](http://web.archive.org/web/20221225035511/http://www.yaml.org/) 和 [app.yaml 参考](http://web.archive.org/web/20221225035511/https://developers.google.com/appengine/docs/python/config/appconfig)。

```java
 application: mkyong-python
version: 1
runtime: python
api_version: 1

handlers:
- url: /.*
  script: helloworld.py 
```

## 5.在本地运行

要在本地运行它，右击`helloworld.py`，选择“运行方式”—>“运行配置”，创建一个新的“ **PyDev Google App 运行**”。

*图 5.1*-在主选项卡- >主模块中，手动键入“ **dev_appserver.py** 的目录路径。“浏览”按钮不能够帮助你，手动输入。

![gea python run locally](img/270194289f835a3806724084d169bab7.png "gae-eclipse-python-hello-world-run-1")

*图 5.2*–在“参数”选项卡- >程序参数中，填入“ **${project_loc}/src** ”。

![gea python run locally](img/6911256865e505bd594e5ce04f523b4a.png "gae-eclipse-python-hello-world-run-2")

*图 5.3*–运行它。默认情况下，它会部署到 *http://localhost:8080* 。

![gea python run locally](img/50eeb66772a69ad4947f3aa6a08c9600.png "gae-eclipse-python-hello-world-run-3")

*图 5.4*–完成。

![gea python run locally](img/23445045d952816a4e5daec0f2dcde78.png "gae-eclipse-python-hello-world-run-4")

## 5.部署到 Google 应用引擎

在[https://appengine.google.com/](http://web.archive.org/web/20221225035511/https://appengine.google.com/)上注册一个账户，并为你的网络应用创建一个应用 ID。再次回顾“`app.yaml`”，这个 web 应用程序将被部署到 GAE，应用程序 ID 为“ **mkyong-python** ”。

*文件:app.yaml*

```java
 application: mkyong-python
version: 1
runtime: python
api_version: 1

handlers:
- url: /.*
  script: helloworld.py 
```

要部署到 GAE，请参见以下步骤:

*图 5.1*-新建一个“PyDev Google App Run”，在主选项卡- >主模块中，手动键入“ **appcfg.py** 的目录路径。

![deploy python to GAE](img/a6b1ba4b69db4425f2d9d88a5b95e6ce.png "gae-eclipse-python-hello-world-deploy-1")

*图 5.2*–在参数选项卡- >程序参数中，放入“**更新${project_loc}/src** ”。

![deploy python to GAE](img/fd0b1524d2269be4e9aaaaf7dcf415d3.png "gae-eclipse-python-hello-world-deploy-2")

*图 5.3*–在部署过程中，您需要输入您的 GAE 电子邮件和密码进行验证。

![deploy python to GAE](img/e8e92a60985005ec0898889e57325669.png "gae-eclipse-python-hello-world-deploy-3")

*图 5.4*-如果成功，web app 将部署到 http://mkyong-python.appspot.com/*。*

*![deploy python to GAE](img/a90e9d473cbf3dc3fa551b43f79057b4.png "gae-eclipse-python-hello-world-deploy-4")

完成了。

## 参考

1.  用于 Eclipse 的 PyDev 插件
2.  [Yaml 官网](http://web.archive.org/web/20221225035511/http://www.yaml.org/)
3.  [GAE 开始使用 Python](http://web.archive.org/web/20221225035511/https://developers.google.com/appengine/docs/python/gettingstarted/)
4.  [为 Eclipse 安装 PyDev】](http://web.archive.org/web/20221225035511/https://developers.google.com/appengine/articles/eclipse)
5.  [GAE Java hello world 使用 Eclipse 的例子](http://web.archive.org/web/20221225035511/http://www.mkyong.com/google-app-engine/google-app-engine-hello-world-example-using-eclipse/)

<input type="hidden" id="mkyong-current-postId" value="10787">*