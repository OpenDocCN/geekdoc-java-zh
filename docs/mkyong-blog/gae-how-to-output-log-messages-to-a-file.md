# GAE:如何将日志信息输出到文件中

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/google-app-engine/gae-how-to-output-log-messages-to-a-file/>

默认情况下，所有日志记录消息都将输出到日志控制台。要更改日志设置，找到这个文件—*{ Google App Engine SDK directory } \ Google \ App Engine \ tools \`dev_appserver_main.py`*。

*File:dev _ appserver _ main . py*–查找以下模式

```java
 #...
import getopt
import logging
import os
import signal
import sys
import tempfile
import traceback

logging.basicConfig(
    level=logging.INFO,
    format='%(levelname)-8s %(asctime)s %(filename)s:%(lineno)s] %(message)s')
#... 
```

## 输出到文件

为了将日志信息输出到一个文件中，我们可以在`dev_appserver_main.py`中更改日志记录的配置，如下所示:

```java
 #...
import getopt
import logging
import os
import signal
import sys
import tempfile
import traceback

# default , comment out
#logging.basicConfig(
#    level=logging.INFO,
#    format='%(levelname)-8s %(asctime)s %(filename)s:%(lineno)s] %(message)s')

# new log settings , output to a file
logging.basicConfig(
    filename='/Users/lokjack/gae.log',
    filemode='a', 
    level=logging.DEBUG,
    format='%(levelname)-8s %(asctime)s %(filename)s:%(lineno)s] %(message)s')
#... 
```

更改`dev_appserver_main.py`后重启`dev_appserver.py`。

现在，日志控制台不会显示任何日志消息，而是将所有日志消息输出到一个文件中(在本例中，所有日志消息都将输出到" */Users/lokjack/gae.log* ")。

**Note**
This hacks only works on local GAE development environment.

## 下载源代码

Download it – [gae-logging-to-file.zip](http://web.archive.org/web/20190226091841/http://www.mkyong.com/wp-content/uploads/2012/08/gae-logging-to-file.zip) (11 kb)

## 参考

1.  [在 Python 中记录到文件](http://web.archive.org/web/20190226091841/http://docs.python.org/howto/logging.html#logging-to-a-file)
2.  Google App Engine 允许在服务器上创建文件和文件夹吗？

[gae](http://web.archive.org/web/20190226091841/http://www.mkyong.com/tag/gae/) [logs](http://web.archive.org/web/20190226091841/http://www.mkyong.com/tag/logs/)![](img/f46b568135bcdf0820b0c2da828870b7.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190226091841/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="11411">





##### 杰克洛克

Co-founder of [penefit.com](http://web.archive.org/web/20190226091841/http://www.penefit.com/).