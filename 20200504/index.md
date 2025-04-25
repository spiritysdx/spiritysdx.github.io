# 爬虫流程及方法13(xpath解析页面)

### 前言

xpath解析原理:   
1.实例化一个etree的对象，且需要将被解析的页面源码数据加载到该对象中。  
2.调用et ree对象中的xpath方法结合着xpath表达式实现标签的定位和内容的捕获。  

环境的安装:

```
pip install Lxml
```

如何实例化一个etree对象:

```python
from Lxml import etree
```

1.将本地的html文档中的源码数据加载到etree对象中:
```python
etree.parse(fiLePath)
```

2.可以将从互联网上获取的源码数据加裁到该对象中
```python
etree.HTML( 'page_ text' )
```
```python
xpath('xpath表达式')
```
```python
#3.7版本后引入etree模块如下，3.5版本以下可以直接从lxml中引入
from lxml import html
etree = html.etree

parser = etree.HTMLParser(encoding="utf-8")
#实例化好了一个etree对象，且将被解析的源码加载到了该对象中
tree = etree.parse('bs4练习.html', parser=parser)
# r = tree.xpath('/html/body/div')
# r = tree.xpath('/html//div')
# r = tree.xpath('//div')
```

/表示的是从根节点开始定位，一个/x/表示一个层级，//表示跨越多个层级,可以表示从任意位置开始定位

```python
r = tree.xpath('//div[@class="song"]')
```

属性定位：


```
//div[@class="song"] tag[@attrName="attrValue"]
```

```pythpn
r = tree.xpath('//div[@class="song"]/p[3]')
```

索引定位：

```html
'//div[@class="song"]/p[3]'  #这里索引以1开始
```

```
r = tree.xpath('//div[@class="tang"]//li[5]/a/text()')[0]

取文本：

```
/text() #获取的是标签中直系的文本内容
```

```
//text() #获取标签中非直系文本内容（所有文本内容）
```

```python
r = tree.xpath('//div[@class="tang"]/text()')
```

取属性：

/@attrName  ==>img/src
```python
r = tree.xpath('//div[@class="song"]/img/@src')
```

实际案例：
```python
#3.7版本后引入etree模块如下，3.5版本以下可以直接从lxml中引入
from lxml import html
import requests
etree = html.etree


a = True
while a:
    #爬取页面源码数据
    url = 'https://mm.58.com/ershoufang/'
    headers = {
        'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.122 Safari/537.36'
    }
    page_text = requests.get(url=url, headers=headers).text
    #数据解析
    tree = etree.HTML(page_text)
    li_list = tree.xpath('//ul[@class="house-list-wrap"]/li')
    fp = open('58.txt', 'w', encoding='utf-8')
    for li in li_list:
        #页面数据局部解析
        title = li.xpath('./div[2]/h2/a/text()')[0]#./表示从前面的li开始（局部开始）
        print(title)
        fp.write(title+'\n')
a = False
```




