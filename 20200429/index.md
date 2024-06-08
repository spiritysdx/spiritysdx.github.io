# 爬虫流程及方法10(爬虫伪装专题篇)


原网页链接[萌新论坛](https://www.lolichan.vip/)

### requests 伪装 headers 发送请求

headers中空着的可能有也可能无，user-agent基本得有

在chrome中找到网页的请求头，图片如下

![chrome](https://pic./2020/04/29/2b9000003ca32.png)

```python
headers = {
        "Accept": "  ",
        "Accept-Encoding": "  ",
        "Accept-Language": "   ",
        "Host": "   ",
        'user-agent':'粘贴1处'
    }    
    #在页面中点击右键选择检查，调出网页自带的抓包工具，打开Network后刷新当前页面抓包找到user-agent的项复制粘贴1
```

cookie的三种传参方法

```python
headers = {"User_Agent":"Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/64.0.3282.186 Safari/537.36",
    "Cookie" : "   ",
    "Refer" : " 从哪个网页来的(url)"
}

#三种Cookie请求方式
'''第一种：cookie放在headers中'''
headers = {"User_Agent":"Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/64.0.3282.186 Safari/537.36",
    "Cookie" : "   "
}
'''第二种：cookie字典传给cookies参数'''
'''第三种 先发送post请求，获取cookie，带上cookie请求登陆之后的页面'''
# 如果没有的就要抓包了Network -> preserve log -> login包 ->requesy
seesion = requests.seesion()
# 用户名作为键， 真正的密码作为值  模拟登陆
post_data = {"email":"xxxx", "password":"xxxx"}
seesion.post(url=url, data=post_data, headers=headers) # 服务器设置在本地的cookie会保存在本地
seesion.get(url) # 会带上之前保存在seesion中的cookie，能够请求成功
```

### requests 伪装 params 传输数据

```python
params = {
        '某名字':'某值',
        '粘贴2': '黏贴2'
    }
#在页面中点击右键选择检查，调出网页自带的抓包工具，打开Network后刷新当前页面抓包找到Query String Parameters的项复制粘贴2（记得加符号'粘贴2'）
```

User-Agent：这里面存放浏览器的信息。可以看到上面的参数值，它表示我是通过Windows的Chrome浏览器，访问的这个服务器。如果我们不设置这个参数，用Python程序直接发送GET请求，服务器接受到的User-Agent信息就会是一个包含python字样的User-Agent。如果后台设计者验证这个User-Agent参数是否合法，不让带Python字样的User-Agent访问，这样就起到了反爬虫的作用。这是一个最简单的，最常用的反爬虫手段。

Referer：这个参数也可以用于反爬虫，它表示这个请求是从哪发出的。可以看到我们通过浏览器访问网站，这个请求是从哪个页面来的(那个页面包含该链接)，这个地址发出的。如果后台设计者，验证这个参数，对于不是从这个地址跳转过来的请求一律禁止访问，这样就也起到了反爬虫的作用。

ps: authorization：这个参数是基于AAA模型中的身份验证信息允许访问一种资源的行为。在我们用浏览器访问的时候，服务器会为访问者分配这个用户ID。如果后台设计者，验证这个参数，对于没有用户ID的请求一律禁止访问，这样就又起到了反爬虫的作用。

UserAgent伪装集合详见[资源页面](https://spiritlhl.github.io/resource/)

### selenium 模拟使用浏览器伪装 headers

使用自动化测试工具 selenium 可以模拟使用浏览器访问网站。使用的selenium版本大都支持 Chrome 和 Firefox 浏览器。要使用该库浏览器需要下载对应版本到电脑上。

使用 webdriver 访问本身自带浏览器的 headers。

```python
import selenium
import selenium.webdriver
import ssl

def get_url_text(url):
    driver = selenium.webdriver.Chrome()#模拟调用谷歌游览器（模拟你电脑有的游览器操作）
    driver.get(url)#访问链接
    pagesource=driver.page_source#抓取网页源代码
    #你要执行的预处理写这里
    driver.close()
    return #返回值
```

### ssl处理(仅针对使用urllib与urllib3)

*urllib库爬虫*  

```python
import ssl


context = ssl._create_unverified_context()
#忽略安全
```

*requests库爬虫*

忽略ssl验证使得网页访问得以顺利通过

verify=False 代表不做证书验证

```python
import requests
from requests.packages import urllib3


urllib3.disable_warnings() #关闭警告
respone=requests.get('https://www.12306.cn',verify=False)
print(respone.status_code)

```

不做证书验证的情况，在某些情况下是行不通的的，这需要其他处理方式

### requests 使用 ip 代理发送请求

查询自己的ip，网址[https://httpbin.org/ip](https://httpbin.org/ip)

```python
import requests


a = True
while a:
    try:
        url = 'https://httpbin.org/ip'
        response = requests.get(url)
        print(response.text)
    except:
        print("爬取失败")
a = False
```


使用代理ip与伪装headers的方式相似,只需要传入proxies参数。  
比如现在我有一个代理ip:111.164.20.86:8111，使用如下：

```python
import requests


a = True
while a:
    url = 'https://httpbin.org/ip'
    proxy = {
        'http': '111.164.20.86:8111',
    }
    #或者填入https请求的'https': '   '
    response = requests.get(url=url, proxies=proxy)
    print(response.text)
    #返回值是代理ip地址，更换url即可使用代理ip爬虫
a = False
```
支持socks代理,安装输入：
```
pip install requests[socks]
```
实例：
```python
import requests
proxies = {
    'http': 'socks5://user:pass@host:port',
    'https': 'socks5://user:pass@host:port'
}
respone=requests.get('https://www.12306.cn', proxies=proxies)

print(respone.status_code)
```

如果没考虑好用http/https，建议两个都写入字典proxy里面，就不会报错

代理免费ip可能会时不时中断，建议淘宝购买(支持验证那种)，当然如果购买阿里云端口也是可以的

### 重复执行报错处理

```python
import requests
from retrying import retry


#@retry(stop_max_attempt_number = 10)
'''让被装饰的函数反复执行10次，10次全部报错才会报错， 中间有一次正常就继续往下走'''
url = "http://www.baidu.com"
response = requests.get(url,timeout=0.01)#timeout=0.01 代表请求+接收服务端数据的总时间
#如果想明确控制链接与等待接收服务端数据的时间则写  timeout=(1,2)
#timeout=(1,2)--->1代表链接超时时间  2代表接收数据的超时时间
print(response.content.decode())

headers = {"User_Agent":"Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/64.0.3282.186 Safari/537.36",
}
```





