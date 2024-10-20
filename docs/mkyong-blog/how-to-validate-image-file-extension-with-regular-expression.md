# 如何用正则表达式验证图像文件扩展名

> 原文：<http://web.archive.org/web/20230101150211/http://www.mkyong.com/regular-expressions/how-to-validate-image-file-extension-with-regular-expression/>

图像文件扩展名正则表达式模式

```java
 ([^\s]+(\.(?i)(jpg|png|gif|bmp))$) 
```

描述

```java
 (			#Start of the group #1
 [^\s]+			#  must contains one or more anything (except white space)
       (		#    start of the group #2
         \.		#	follow by a dot "."
         (?i)		#	ignore the case sensive checking for the following characters
             (		#	  start of the group #3
              jpg	#	    contains characters "jpg"
              |		#	    ..or
              png	#	    contains characters "png"
              |		#	    ..or
              gif	#	    contains characters "gif"
              |		#	    ..or
              bmp	#	    contains characters "bmp"
             )		#	  end of the group #3
       )		#     end of the group #2	
  $			#  end of the string
)			#end of the group #1 
```

整个组合是指，必须有一个或多个字符串(但不是空白)，后跟点“.”并且字符串以“jpg”或“png”或“gif”或“bmp”结尾，文件扩展名不区分大小写。

这种正则表达式模式广泛用于不同文件广泛检查。您只需改变结尾组合 **(jpg|png|gif|bmp)** 就可以得到适合您需要的不同文件扩展名检查。

## Java 正则表达式示例

```java
 package com.mkyong.regex;

import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class ImageValidator{

   private Pattern pattern;
   private Matcher matcher;

   private static final String IMAGE_PATTERN = 
                "([^\\s]+(\\.(?i)(jpg|png|gif|bmp))$)";

   public ImageValidator(){
	  pattern = Pattern.compile(IMAGE_PATTERN);
   }

   /**
   * Validate image with regular expression
   * @param image image for validation
   * @return true valid image, false invalid image
   */
   public boolean validate(final String image){

	  matcher = pattern.matcher(image);
	  return matcher.matches();

   }
} 
```

## 匹配的图像文件:

1." a.jpg "，" a.gif "，" a.png "，" a.bmp "，
2。"..jpg "，"..gif "，"..png "，"..bmp”，
3。“a.JPG”，“a.GIF”，“a.PNG”，“a.BMP”，
4。" a.JpG "，" a.GiF "，" a.PnG "，" a.BmP "，
5。" jpg.jpg "，" gif.gif "，" png.png "，" bmp.bmp "

## 不匹配的图像:

1.".jpg“，”。gif“，”。png“，”。BMP”-需要图像文件名
2。" .jpg“，”。gif“，”。png“，”。BMP”-第一个字符
3 中不允许有空格。" a.txt "，" a.exe "，" a . "，" a . MP3 "-只允许图像文件扩展名为
3。"jpg "，" gif "，" png "，" BMP "-需要图像文件扩展名

## 单元测试–image validator

```java
 package com.mkyong.regex;

import org.testng.Assert;
import org.testng.annotations.*;

/**
 * Image validator Testing
 * @author mkyong
 *
 */
public class ImageValidatorTest {

	private ImageValidator imageValidator;

	@BeforeClass
        public void initData(){
		imageValidator = new ImageValidator();
        }

	@DataProvider
	public Object[][] ValidImageProvider() {
	   return new Object[][]{
    	     {new String[] {
		   "a.jpg", "a.gif","a.png", "a.bmp",
		   "..jpg", "..gif","..png", "..bmp",
		   "a.JPG", "a.GIF","a.PNG", "a.BMP",
		   "a.JpG", "a.GiF","a.PnG", "a.BmP",
		   "jpg.jpg", "gif.gif","png.png", "bmp.bmp"
  	       }
              }
	   };
	}

	@DataProvider
	public Object[][] InvalidImageProvider() {
	  return new Object[][]{
	    {new String[] {
		   ".jpg", ".gif",".png",".bmp",
		   " .jpg", " .gif"," .png"," .bmp",
                   "a.txt", "a.exe","a.","a.mp3",
		   "jpg", "gif","png","bmp"
	       }
             }
	   };
	}

	@Test(dataProvider = "ValidImageProvider")
	 public void ValidImageTest(String[] Image) {

	   for(String temp : Image){
		   boolean valid = imageValidator.validate(temp);
		   System.out.println("Image is valid : " + temp + " , " + valid);
		   Assert.assertEquals(true, valid);
	   }

	}

	@Test(dataProvider = "InvalidImageProvider", 
                 dependsOnMethods="ValidImageTest")
	public void InValidImageTest(String[] Image) {

	   for(String temp : Image){
		   boolean valid = imageValidator.validate(temp);
		   System.out.println("Image is valid : " + temp + " , " + valid);
		   Assert.assertEquals(false, valid);
	   }
	}	
} 
```

## 单元测试–结果

```java
 Image is valid : a.jpg , true
Image is valid : a.gif , true
Image is valid : a.png , true
Image is valid : a.bmp , true
Image is valid : ..jpg , true
Image is valid : ..gif , true
Image is valid : ..png , true
Image is valid : ..bmp , true
Image is valid : a.JPG , true
Image is valid : a.GIF , true
Image is valid : a.PNG , true
Image is valid : a.BMP , true
Image is valid : a.JpG , true
Image is valid : a.GiF , true
Image is valid : a.PnG , true
Image is valid : a.BmP , true
Image is valid : jpg.jpg , true
Image is valid : gif.gif , true
Image is valid : png.png , true
Image is valid : bmp.bmp , true
Image is valid : .jpg , false
Image is valid : .gif , false
Image is valid : .png , false
Image is valid : .bmp , false
Image is valid :  .jpg , false
Image is valid :  .gif , false
Image is valid :  .png , false
Image is valid :  .bmp , false
Image is valid : a.txt , false
Image is valid : a.exe , false
Image is valid : a. , false
Image is valid : a.mp3 , false
Image is valid : jpg , false
Image is valid : gif , false
Image is valid : png , false
Image is valid : bmp , false
PASSED: ValidImageTest([Ljava.lang.String;@1d4c61c)
PASSED: InValidImageTest([Ljava.lang.String;@116471f)

===============================================
    com.mkyong.regex.ImageValidatorTest
    Tests run: 2, Failures: 0, Skips: 0
===============================================

===============================================
mkyong
Total tests run: 2, Failures: 0, Skips: 0
=============================================== 
```

想了解更多关于正则表达式的知识吗？强烈推荐这本最好最经典的书——《掌握正则表达式》

<center>

[http://web.archive.org/web/20221227014350if_/https://rcm.amazon.com/e/cm?t=progrlife-20&o=1&p=8&l=as1&asins=0596528124&fc1=000000&IS2=1&lt1=_blank&m=amazon&lc1=0000FF&bc1=000000&bg1=FFFFFF&f=ifr](http://web.archive.org/web/20221227014350if_/https://rcm.amazon.com/e/cm?t=progrlife-20&o=1&p=8&l=as1&asins=0596528124&fc1=000000&IS2=1&lt1=_blank&m=amazon&lc1=0000FF&bc1=000000&bg1=FFFFFF&f=ifr)

</center>

<input type="hidden" id="mkyong-current-postId" value="1925">