# 爬虫流程及方法01(入门准备及Request库使用)

### <font color=cyan>爬虫究竟是合法还是违法的?</font>
    在法律中是不被禁止 
    具有违法风险 

# 请善意爬虫
# 切勿恶意爬虫

### <font color=cyan>爬虫带来的风险可以体现在如下2方面:</font>

爬虫干扰了被访问网站的正常运营  
爬虫抓取了受到法律保护的特定类型的数据或信息

### <font color=cyan>如何在使用编写爬虫的过程中避免进入局子的厄运呢?</font>

时常的优化自己的程序，避免干扰被访问网站的正常运行  
在使用传播爬取到的数据时，审查抓取到的内容，如果发现了涉及到用户的商业机密等敏感内容需要及时停止爬取或传播  

### <font color=cyan>爬虫的矛与盾</font>

反爬机制:门户网站，可以通过制定相应的策略或者技术手段，防止爬虫程序进行网站数据的爬取。  
反反爬策略:爬虫程序可以通过制定相关的策略或者技术手段，破解]户网站中具备的反爬机制，从而可以获取门户网

### <font color=cyan>robots. txt协议:?</font>

君子协议:规定了网站中哪些数据可以被爬虫爬取哪些数据不可以被爬取。

例如这个：[https://www.bilibili.com/robots.txt](https://www.bilibili.com/robots.txt)

# <font color=cyan>正文：</font>

### 

软件：pycharm/thonny/自带IDLE,明白python的基本语法，缩进/字典/元组/列表/循环结构/函数运用/文件存储（绝对路径/相对路径）

#### <font color=cyan>step1.安装第三方模块：调出cmd窗口输入以下字符</font>

（每一行是一个包，等待下载完后再进行下一个包的下载）

```
           python -m pip install --upgrade pip

           pip install requests

           pip install bs4

           pip install lxml

           pip install urllib
```

#### <font color=cyan>step2.确认爬虫流程：</font>
          1.指定url

          2.请求前进行UA伪装（模拟游览器发出请求）

          3.选择post还是get请求

          4.请求发送

          5.获取响应数据

          6.进行存储


#### <font color=cyan>step3.实际代码的编写：</font>
```python
import requests  #每使用一个包的方法就得导入一个包
#（引入包后空两行，语法的正确书写习惯）
a = True
while a:  #这里可以改成循环结构对网页进行实时爬取，每次爬取覆盖上次的成果
    url = "网址"  #指定所爬取页面的网址
    headers = {
        'user-agent':'粘贴1处'
    }    #在页面中点击右键选择检查，调出网页自带的抓包工具，在network中刷新当前页面抓包找到user-agent的项复制粘贴1，找到Query String Parameters的项复制粘贴2（记得加符号’粘贴2‘）
    params = {
        '某名字':'某值',
        '粘贴2':'黏贴2'
    }
    res = requests.get(url=url,params=params,headers=headers)  #这里是对网页发起请求并内置参数
    dict_object = res.text #对返回内容进行text处理并赋值给一个字典
    with open("./lolichan.html","w",encoding='utf-8') as fp:
           #打开或生成一个文档并选定为写w的状态，转换字符的编码为utf-8
        fp.write(dict_object)#往文档内存储爬取的网页源码
    print('over') #存储成功提示
    a = False
```

#### <font color=cyan>step4.实际运用：</font>

安装而未用到的包下次再讲，剩下的包用于数据的解析定位  
如果想要看效果视频，参照B站视频[av92683334](https://www.bilibili.com/video/BV1QE411J7m7/)
更多内容:
【官方文档】[opencv-python中文文档](https://www.kancloud.cn/aollo/aolloopencv/269602)

