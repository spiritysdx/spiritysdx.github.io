# 爬虫流程及方法05(Scrapy入门级爬虫)

### <font color=cyan>Request类</font>
```python
class scrapy.http.Request()
```
Request对象表示一个HTTP请求。  
由Spider生成，由Downloader执行。  
常用属性：
![1](https://pic./2020/04/19/46edb08266e4f.png)

### <font color=cyan>Response类</font>
```python
class scrapy.http.Response()
```
Response对象表示一个HTTP响应。  
由Downloader生成，由Spider处理。  
常用属性与方法：
![2](https://pic./2020/04/19/584ddc21cbb3c.png)

### <font color=cyan>Item类</font>
```python
class scrapy.item.Item()
```
Item对象表示一个从HTML页面中提取的信息内容。  
由Spider生成，由Item Pipeline处理。  
Item类似字典类型，可以按照字典类型操作。

### <font color=cyan>Scrapy爬虫支持多种HTML信息提取方法</font>
```
Beautiful Soup
lxml
re
XPath Selector
CSS Selector
```
CSS Selector的基本形式:
![3](https://pic./2020/04/19/f685927e4f207.png)

### <font color=cyan>步骤</font>
步骤1:建立工程和Spider模板  
步骤2:编写Spider(实际爬虫)  
步骤3:编写ITEM Pipelines(爬虫数据处理)

#### <font color=cyan>步骤3:编写Pipelines:</font>
配置pipelines.py文件  
定义对爬取项(Scraped Item)的处理类  
配置ITEM PIPELINES选项(配置setting.py文件)  

## PS：以下代码仅供参考，不具备运行基础
```python
#工程文档为以下内容
import scrapy
import re


class StocksSpider (scrapy . Spider) :
    name = "stock s"
    start_urls = ['http://quote.eastmoney.com/stocklist.html']

    def parse(self, response):
        for href in response.css ('a: :attr (href) ').extract():
            try:
                stock = re.findall(r"[s][hz]\d{6}", href)[0]
                url = 'https://gupiao.baidu.com/stock/' + stock + '.html'
                yield scrapy.Request(url, callback=self.parse_stock)
            except:
                continue

    def parse_stock(self, response):
        infoDict = { }
        stockInfo = response.css('.stock-bets')
        name = stockInfo.css('.bets-name').extract()[0]
        keyList = stockInfo.css('dt').extract()
        valueList = stockInfo.css('dd').extract()
        for i in range(len(keyList)):
            key = re.findall(r'>.*</dt>', keyList[i])[0][1:-5]
        try:
            val = re.findall(r'\d+\.?.*</dd>', valueList[1])[0][0:-5]
        except:
            val = '----'
        infoDict[key] = val

        infoDict.update({'股票名称': re.findall('\s.*\(', name)[0].split()[0] + re.findall('\>.*\<', name)[0][1:-1]})
        yield infoDict
```
```python
#pipeline文档下的内容
class BaidustocksPipeline(object):
    def process_item(self, item, spider):
        return item
class BaidustocksInfoPipeline(object):
    def open_spider(self, spider):
        self.f = open('BaidustockInfo.txt', 'W')

    def close_spider(self, spider):
        self.f.close()

    def process_item(self, item, spider):
        try:
            line = str(dict(item)) + '\n'
            self.f.write(line)
        except:
            pass
        return item
```

### <font color=cyan>总结：</font>
技术路线：  
requests-bs4-re      
scrapy(5+2结构)   
scrapy + requests-bs4-re + PhantomJS   --->表单提交、爬取周期、入库存储(js处理)


