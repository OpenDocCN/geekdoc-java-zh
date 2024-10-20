# Android:如何检查设备是否有摄像头

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/android/android-how-to-check-if-device-has-camera/>

在 Android 中，您可以使用[`PackageManager`](http://web.archive.org/web/20190214233023/http://developer.android.com/reference/android/content/pm/PackageManager.html)`hasSystemFeature()`方法来检查设备是否具有摄像头、gps 或其他功能。

查看在活动类中使用`PackageManager`的完整示例。

```java
 package com.mkyong.android;

import android.app.Activity;
import android.content.Context;
import android.content.pm.PackageManager;
import android.os.Bundle;
import android.util.Log;

public class FlashLightActivity extends Activity {

	@Override
	public void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		//setContentView(R.layout.main);

		Context context = this;
		PackageManager packageManager = context.getPackageManager();

		// if device support camera?
		if (packageManager.hasSystemFeature(PackageManager.FEATURE_CAMERA)) {
			//yes
			Log.i("camera", "This device has camera!");
		}else{
			//no
			Log.i("camera", "This device has no camera!");
		}

	}
} 
```

**Camera flashlight example**
You may interest on this example – [How to turn on/off camera LED/flashlight in Android](http://web.archive.org/web/20190214233023/http://www.mkyong.com/android/how-to-turn-onoff-camera-ledflashlight-in-android/).

## 参考

1.  [Android package manager Javadoc](http://web.archive.org/web/20190214233023/http://developer.android.com/reference/android/content/pm/PackageManager.html)

[android](http://web.archive.org/web/20190214233023/http://www.mkyong.com/tag/android/) [camera](http://web.archive.org/web/20190214233023/http://www.mkyong.com/tag/camera/)![](img/da86bc845835ec7c622479459f577ba1.png) (function (i,d,s,o,m,r,c,l,w,q,y,h,g) { var e=d.getElementById(r);if(e===null){ var t = d.createElement(o); t.src = g; t.id = r; t.setAttribute(m, s);t.async = 1;var n=d.getElementsByTagName(o)[0];n.parentNode.insertBefore(t, n); var dt=new Date().getTime(); try{i[l][w+y](h,i[l][q+y](h)+'&amp;'+dt);}catch(er){i[h]=dt;} } else if(typeof i[c]!=='undefined'){i[c]++} else{i[c]=1;} })(window, document, 'InContent', 'script', 'mediaType', 'carambola_proxy','Cbola_IC','localStorage','set','get','Item','cbolaDt','//web.archive.org/web/20190214233023/http://route.carambo.la/inimage/getlayer?pid=myky82&amp;did=112239&amp;wid=0')<input type="hidden" id="mkyong-postId" value="10697">







