# 如何在 Android 的网络浏览器中打开一个网址

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/android/how-to-open-an-url-in-androids-web-browser/>

这里有一个代码片段，展示了如何使用“`android.content.Intent`”在 Android 的网络浏览器中打开一个指定的 URL。

```java
 button.setOnClickListener(new OnClickListener() {

		@Override
		public void onClick(View arg0) {

			Intent intent = new Intent(Intent.ACTION_VIEW, 
			     Uri.parse("http://www.mkyong.com"));
			startActivity(intent);

		}

	}); 
```

**Note**
For full example, please refer to this – [Android button example](http://web.archive.org/web/20190215001302/http://www.mkyong.com/android/android-button-example/).

## 参考

1.  [安卓按钮 JavaDoc](http://web.archive.org/web/20190215001302/http://developer.android.com/reference/android/widget/Button.html)
2.  [安卓意向。ACTION_VIEW JavaDoc](http://web.archive.org/web/20190215001302/http://developer.android.com/reference/android/content/Intent.html#ACTION_VIEW)

[android](http://web.archive.org/web/20190215001302/http://www.mkyong.com/tag/android/) [browser](http://web.archive.org/web/20190215001302/http://www.mkyong.com/tag/browser/) [url](http://web.archive.org/web/20190215001302/http://www.mkyong.com/tag/url/)![](img/f64156194a826e6ebf878db60a2e972f.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190215001302/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="10345">







