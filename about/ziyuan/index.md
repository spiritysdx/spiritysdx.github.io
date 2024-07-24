# 博客相关资源


### 尹成爬虫课件

[蓝奏云链接](https://spiritlhl.lanzous.com/ic2ut2f)


### 电脑抓包工具fiddler

[fiddler百度云盘链接](https://pan.baidu.com/s/1Q1paSfVbpXpjmITOOIW1rw)  
提取码：qk5c    

[fiddler蓝奏云盘链接](https://spiritlhl.lanzous.com/ic2un3a)  


### 爬虫headers伪装UserAgent集合

#### 电脑端
```python
pcUserAgent = {
"safari 5.1 – MAC":"User-Agent:Mozilla/5.0 (Macintosh; U; Intel Mac OS X 10_6_8; en-us) AppleWebKit/534.50 (KHTML, like Gecko) Version/5.1 Safari/534.50",
"safari 5.1 – Windows":"User-Agent:Mozilla/5.0 (Windows; U; Windows NT 6.1; en-us) AppleWebKit/534.50 (KHTML, like Gecko) Version/5.1 Safari/534.50",
"IE 9.0":"User-Agent:Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Trident/5.0);",
"IE 8.0":"User-Agent:Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 6.0; Trident/4.0)",
"IE 7.0":"User-Agent:Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 6.0)",
"IE 6.0":"User-Agent: Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1)",
"Firefox 4.0.1 – MAC":"User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.6; rv:2.0.1) Gecko/20100101 Firefox/4.0.1",
"Firefox 4.0.1 – Windows":"User-Agent:Mozilla/5.0 (Windows NT 6.1; rv:2.0.1) Gecko/20100101 Firefox/4.0.1",
"Opera 11.11 – MAC":"User-Agent:Opera/9.80 (Macintosh; Intel Mac OS X 10.6.8; U; en) Presto/2.8.131 Version/11.11",
"Opera 11.11 – Windows":"User-Agent:Opera/9.80 (Windows NT 6.1; U; en) Presto/2.8.131 Version/11.11",
"Chrome 17.0 – MAC":"User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_7_0) AppleWebKit/535.11 (KHTML, like Gecko) Chrome/17.0.963.56 Safari/535.11",
"Maxthon":"User-Agent: Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; Maxthon 2.0)",
"Tencent TT":"User-Agent: Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; TencentTraveler 4.0)",
"The World 2.x":"User-Agent: Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1)",
"The World 3.x":"User-Agent: Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; The World)",
"sogou 1.x":"User-Agent: Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; Trident/4.0; SE 2.X MetaSr 1.0; SE 2.X MetaSr 1.0; .NET CLR 2.0.50727; SE 2.X MetaSr 1.0)",
"360":"User-Agent: Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; 360SE)",
"Avant":"User-Agent: Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; Avant Browser)",
"Green Browser":"User-Agent: Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1)"
}
```

#### 移动手机端

```python
mobileUserAgent = {
"iOS 4.33 – iPhone":"User-Agent:Mozilla/5.0 (iPhone; U; CPU iPhone OS 4_3_3 like Mac OS X; en-us) AppleWebKit/533.17.9 (KHTML, like Gecko) Version/5.0.2 Mobile/8J2 Safari/6533.18.5",
"iOS 4.33 – iPod Touch":"User-Agent:Mozilla/5.0 (iPod; U; CPU iPhone OS 4_3_3 like Mac OS X; en-us) AppleWebKit/533.17.9 (KHTML, like Gecko) Version/5.0.2 Mobile/8J2 Safari/6533.18.5",
"iOS 4.33 – iPad":"User-Agent:Mozilla/5.0 (iPad; U; CPU OS 4_3_3 like Mac OS X; en-us) AppleWebKit/533.17.9 (KHTML, like Gecko) Version/5.0.2 Mobile/8J2 Safari/6533.18.5",
"Android N1":"User-Agent: Mozilla/5.0 (Linux; U; Android 2.3.7; en-us; Nexus One Build/FRF91) AppleWebKit/533.1 (KHTML, like Gecko) Version/4.0 Mobile Safari/533.1",
"Android QQ":"User-Agent: MQQBrowser/26 Mozilla/5.0 (Linux; U; Android 2.3.7; zh-cn; MB200 Build/GRJ22; CyanogenMod-7) AppleWebKit/533.1 (KHTML, like Gecko) Version/4.0 Mobile Safari/533.1",
"Android Opera ":"User-Agent: Opera/9.80 (Android 2.3.4; Linux; Opera Mobi/build-1107180945; U; en-GB) Presto/2.8.149 Version/11.10",
"Android Pad Moto Xoom":"User-Agent: Mozilla/5.0 (Linux; U; Android 3.0; en-us; Xoom Build/HRI39) AppleWebKit/534.13 (KHTML, like Gecko) Version/4.0 Safari/534.13",
"BlackBerry":"User-Agent: Mozilla/5.0 (BlackBerry; U; BlackBerry 9800; en) AppleWebKit/534.1+ (KHTML, like Gecko) Version/6.0.0.337 Mobile Safari/534.1+",
"WebOS HP Touchpad":"User-Agent: Mozilla/5.0 (hp-tablet; Linux; hpwOS/3.0.0; U; en-US) AppleWebKit/534.6 (KHTML, like Gecko) wOSBrowser/233.70 Safari/534.6 TouchPad/1.0",
"Nokia N97":"User-Agent: Mozilla/5.0 (SymbianOS/9.4; Series60/5.0 NokiaN97-1/20.0.019; Profile/MIDP-2.1 Configuration/CLDC-1.1) AppleWebKit/525 (KHTML, like Gecko) BrowserNG/7.1.18124",
"Windows Phone Mango":"User-Agent: Mozilla/5.0 (compatible; MSIE 9.0; Windows Phone OS 7.5; Trident/5.0; IEMobile/9.0; HTC; Titan)",
"UC":"User-Agent: UCWEB7.0.2.37/28/999",
"UC standard":"User-Agent: NOKIA5700/ UCWEB7.0.2.37/28/999",
"UCOpenwave":"User-Agent: Openwave/ UCWEB7.0.2.37/28/999",
"UC Opera":"User-Agent: Mozilla/4.0 (compatible; MSIE 6.0; ) Opera/UCWEB7.0.2.37/28/999"
}
```

### Appium布置软件集合

链接: https://pan.baidu.com/s/126x-AgLKvM7qSJqdOzAAHA 

提取码: h9b2 

### go-cqhttp

下载链接：[点我](https://spiritlhl.lanzoui.com/iv4evr02zed)

### Numpy课件

[**源文档(上)**](https://spiritlhl.lanzous.com/icjjc5c)

[**源文档(下)**](https://spiritlhl.lanzous.com/icwc7ib)

### Pandas课件

[**源文档及相关文件(上)**](https://spiritlhl.lanzous.com/id0v4tc)

[**源文档及相关文件(下)**](https://spiritlhl.lanzous.com/iHs20dxx0ih)

### MySQL数据库下载链接(5.5版本)

[https://spiritlhl.lanzous.com/id5vt2f](https://spiritlhl.lanzous.com/id5vt2f)

### MySQL图形界面客户端下载链接

[https://spiritlhl.lanzous.com/id5vuji](https://spiritlhl.lanzous.com/id5vuji)

### Matplotlib课件

[**源文档及相关文件压缩包**](https://spiritlhl.lanzous.com/iHs20dxx0ih)

### 1Stopt安装包

[**点击跳转下载解压即可使用，记得看说明先**](https://spiritlhl.lanzoui.com/ihGCMrdiutc)

### 代理加速网站

[**主站若下载慢请去镜像站**](https://ghproxy.com/)

### 反代理加速访问Github

将下面链接替换官方链接```https://github.com```即可国内直连加速访问，仅供访问查看。

[**https://git.spiritlhl.workers.dev**](https://git.spiritlhl.workers.dev)

### 新冠疫情数据分析文件

[https://github.com/spiritLHL/Cov2019Analysis](https://github.com/spiritLHL/Cov2019Analysis)

[备用仓库](https://gitee.com/spiritlhl/Cov2019Analysis)

### 常微分偏微分建模练习

[**python数据处理的所有文件**](https://spiritlhl.lanzoui.com/inrOFr79utc)

[**matlab建模的所有文件**](https://spiritlhl.lanzoui.com/izqwWr7a04d)

### 长三角数学建模B题

个人自己写的代码文件包，很乱没整理，1Stopt安装包在上面

[**matlab建模的所有文件**](https://spiritlhl.lanzoui.com/iYK0srdj1fa)
