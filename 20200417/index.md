# 爬虫流程及方法04(Scrapy框架)

### <font color=cyan>安装scrapy</font>

pycharm安装步骤:  
1.打开左上角file  
2.打开Other Setting下的Setting for New Project  
3.在Project Interpreter选择Project Interpreter里你使用的编译器后，点击加号(+)添加包  

![图片1](https://pic./2020/04/17/50187a9d453a1.png)  

4.修改Manage Repositories(参考第三方下载包修改篇)  
5.在搜索框里搜索以下包名xxx(注意字母大小写不同)
```
Scrapy
Twisted
pywin32
wheel
```
![2](https://pic./2020/04/17/5a07d6374190d.png)

6.在terminal窗口输入scrapy确认出现版本信息及命令提示  

![3](https://pic./2020/04/17/47c822b71eabc.png)
### <font color=cyan>step1:</font>
建立工程文档  
终端terminal输入：  

```
scrapy startproject 工程文档名
```


创建得到的文档结构：

    工程文档名/  ----->外层目录

        scrapy.cfg ----->部署scrapy爬虫的配置文件

        工程文档名/ ---->scrapy框架的用户自定义的python代码

            _init_.py  ----->初始化脚本

            items.py  ----->items代码模块（继承类）

            middlewares.py  ----->middlewares代码模块（继承类）

            pipelines.py ------>pipelines代码模板（继承类）

            setting.py ------>scrapy爬虫的配置文件

            spiders/ ------>代码模板目录(继承类)

### <font color=cyan>step2:</font>

产生爬虫  
终端terminal输入：(cmd内或pycharm里面的terminal)  

```
cd 工程文档名
scrapy genspider demo 爬取页面的url
```

##### <font color=cyan>或者：</font>

直接在含spider的目录下新建demo.py文件  
写入以下代码
```python
import scrapy


class DemoSpider(scrapy.Spider):
    name = "demo"
    allowed_domain = ["python123.io"]#爬取该域名下的链接
    start_urls = ['https://python123.io/']#爬取页面的初始页面

    def parse(self, response):#解析页面方法类，形成字典类型或发现新链接
        pass
```

### <font color=cyan>step3:</font>
配置产生的spider爬虫(具体修改demo文件)  
eg：
```python    
def parse(self, response):
    fname = response.url.split('/')[-1]
    with open(fname, 'wb') as f:
        f.write(response.body)
    self.log('Saved file %s.' % fname)
```
### <font color=cyan>step4:</font>

终端terminal运行：  
输入以下代码  

```
scrapy crawl 文件名
eg：scrapy crawl demo
```

#### <font color=cyan>ps:(爬虫的另一种框架)</font>
```python
import scrapy:


class DemoSpider(scrapy.Spider):
    name = "demo"

    def start requests(se1f):
        urls = [
                  'http://python123.io/ws/demo.html '
               ]
        for url in urls:
            yield scrapy.Request(ur1=ur1,callback=self.parse)

    def parse(self,response):
        fname = response.url.split('/')[-1]
        with open (fname,'wb') as f:
            f. write (response.body)
        self.log('Saved file %s.' % fname)
```
#### <font color=cyan>yield关键字</font>

yield ---->生成器  
优势：占用存储少，响应速度快  
生成器是一个不断产生值的函数，包含yield语句的函数是一个生成器  
生成器每次产生一个值(yield语句)，函数被冻结，被唤醒后再产生一个值。  
生成器写法  
```python
def gen(n):
    for i in range(n):
        yield i**2

for 1 in gen(5):
    print(1,"  ",end="")
```
结果
```
>>>0  1    4   9   16
```
一般写法  
```python
def square(n) :
    ls =[i**2 for i in range (n) ]
    return ls
for i in square (5) :
    print(i,"   ",end="")
```
结果  
```
>>>0   1   4   9   16
```




