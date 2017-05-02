
--
layout: blog
title: 'Python网页转PDF文件'
date: 2017-05-02 12:11:34
categories: blog
tags: code
image: ''
lead-text: 'Python将HTML文件转换为PDF文件'
---

## 使用 pdfkit 
[pdfkit的安装和基本使用](http://blog.csdn.net/shenwanjiang111/article/details/67634794)

上面博文已经将pdfkit的基本使用讲述清楚了，这里说的是转换参数中options中的尺寸说明：<br>

```javascript
options = {  
        'page-size': 'Letter',  
        'margin-top': '0.75in',  
        'margin-right': '0.75in',  
        'margin-bottom': '0.75in',  
        'margin-left': '0.75in',  
        'encoding': "UTF-8",  
        'custom-header': [  
            ('Accept-Encoding', 'gzip')  
        ],  
        'cookie': [  
            ('cookie-name1', 'cookie-value1'),  
            ('cookie-name2', 'cookie-value2'),  
        ],  
        'outline-depth': 10,  
    }  
```
[pdfkit的参数说明](https://wkhtmltopdf.org/usage/wkhtmltopdf.txt)这个文档关于转换的size只有
```page-size <Size> Set paper size to: A4, Letter, etc.(default A4)```默认A4大小，或者可以直接设置为B4，等直接按照纸张大小来设置pdf大小，但是为了匹配kindle的大小（6寸）需要单独设置width和height
```page-width <unitreal>```，根据测试，kindle的大小在width:1.2 height:1.4还是比较合适，这个大小看个人对屏幕字体尺寸的设置<br>
还可一设置字体的大小 **minimum-font-size** 来设置字体的大小  

下面是一个参考转pdf时的参数设置
```javascript
options = {  
	'minimum-font-size':'20'
	'page-height': 1.4in'
	'page-width': '1.2in'
        'margin-top': '0.01in',  
        'margin-right': '0.01in',  
        'margin-bottom': '0.01in',  
        'margin-left': '0.01in',  
        'encoding': "UTF-8",  
        'custom-header': [  
            ('Accept-Encoding', 'gzip')  
        ],  
        'cookie': [  
            ('cookie-name1', 'cookie-value1'),  
            ('cookie-name2', 'cookie-value2'),  
        ],  
        'outline-depth': 10,  
    }  
```
