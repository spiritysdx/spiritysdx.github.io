# 爬虫流程及方法02(Beautiful Soup解析页面)


```python
#!/usr/bin/env python3

#对某论坛的爬取
import requests
from bs4 import BeautifulSoup
import time
#需求：爬取网站标题及详情页的文本

a = True
while a:#可转变成实时循环#对首页的页面数据进行爬取
    url = "https://www.lolichan.vip/"
    headers = {
        'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.122 Safari/537.36'
    }
    page_text = requests.get(url=url, headers=headers).text#获取响应数据得加text，不然获取的是响应对象
    #在首页中解析出章节标题和详情页URL
    #1.实例化BeautifulSoup对象，需要将页面源码加载到该对象中
    soup = BeautifulSoup(page_text, 'lxml')
    #解析章节标题与详情页url
    div_list = soup.select('.node-body > div > h3 > a')#使用select层级选择器
    fp = open('./xiangqing.text','w',encoding='utf-8')#创建文档及设定只写w和编码utf-8
    for div in div_list:
        time.sleep(填入休息时间)#防止频繁的请求链接失去响应
        title = div.string#获得该标签下的所有文本
        detail_url = 'https://www.lolichan.vip/' + div['href']#获得详情页的url
        #对详情页发起请求，解析出章节内容
        detail_page_text = requests.get(url=detail_url, headers=headers).text
        #解析出详情页中相关的章节内容
        detail_soup = BeautifulSoup(detail_page_text,'lxml')
        div_tag  = detail_soup.select('.structItem-title > a')
        #原来的class属性得用class_表示，不然会报错（class是保留字）
        content = []#设定空列表
        for a in div_tag:
            content.append(a.text)#往空列表内装填
        fp.write(title+':'+str(content)+'\n')#str()使content对象变为字符串形式
        print(title, '爬取成功')#响应成功
    a = False
```

#号符号是python注释的前置符号

pycharm的热键：
            ctrl+z 键回撤你的对代码的改动  ctrl+/键注释与代码的转换 
            ctrl+c复制 ctrl+v粘贴 


```python
#!/usr/bin/env python3
soup.tagName
    print(soup.meta)#第一组a标签 
```
soup.tagName返回的是html中第一次出现的tagName标签


```python
#!/usr/bin/env python3
soup.find
    print(soup.find('meta'))=print(soup.meta)
```
    属性定位,定位一定class加空格再=  #属性可以是class 或id 或attr


```python
#!/usr/bin/env python3
print(soup.find('div', class = 'song'))
```

```python
#!/usr/bin/env python3
soup.find_all('tagName')
```
找到符合标准的所有标签,返回一个列表

```python
#!/usr/bin/env python3
print(soup.find_all('meta'))
```
```python
#!/usr/bin/env python3
select('某种选择器').
```  
表示class类选择器  id选择器  标签选择器  
返回的是一个列表
```python
#!/usr/bin/env python3
soup.select('xx >  xx  xx')
```
或
```python
#!/usr/bin/env python3
soup.select('xx >  xx > xx')
```

```python
#!/usr/bin/env python3
print(soup.select('.tang'))#<div class='tang'>
```
部分html代码
```
 <div class='tang'>
        <ul>
            <li><a href="http://...." title="qingming">mutongyao</a></li>
            <li><a href="http://...." title="chunjie">...</a></li>
        <ul>
```
```python
print(soup.select('.tang > ul > li > a'))
```
结果：
```
<a href="http://...." title="qingming">...</a>，
<a href="http://...." title="chunjie">...</a>
```

层级选择器：此时返回一个列表，此时'.class的内容 > ul > li > a'中大于号>表示一个层级

```python
#!/usr/bin/env python3
    print(soup.select('.tang > ul > li > a')[0])
    #结果：<a href="http://...." title="qingming">mutongyao</a>
    #多个层级'.tang > ul a' 这里空格表示多个层级，大于号>表示一个层级
```

```python
#!/usr/bin/env python3
    print(soup.select('.tang > ul > li > a')[0])=print(soup.select('.tang > ul a')[0])
```

获取标签之间的文本数据

```python
#!/usr/bin/env python3
    soup.a.text/string/get_text()
    print(soup.select('. class的内容> ul a')[0].text)
```

结果：
```
mutongyao
```

```python
#!/usr/bin/env python3
    text/get_text()
```

get_text()可以获取一个标签中‘所有的’文本内容（即使不属于该标签直系文本内容）

string:只可以获得该标签下直系文本内容

获取标签中的属性值

```python
#!/usr/bin/env python3
print(soup.select('.class的内容 > ul a')[0]['href'])
```

### <font color=cyan>下行遍历（这里两个方法必须是循环中使用）</font>

```python
#!/usr/bin/env python3
for child in soup.body.children:
   print(child)
```

遍历儿子节点

```python
#!/usr/bin/env python3
for child in soup.body.descendants:
   print(child)
```

遍历子孙节点

### <font color=cyan>上行遍历</font>

```python
#!/usr/bin/env python3
for sibling in soup.a.next_siblings:
    print(sibling)
```

遍历后续节点

```python
#!/usr/bin/env python3
for sibling in soup.a.previous_siblings:
    print(sibling)
```

遍历前续节点

```
soup.prettify()
```

每个节点一个换行符

```
<> .find_all(name, attrs, recursive, string, **kwargs）
```
返回一个列表类型，存储查找的结果。  

name:对标签名称的检索字符串。  
attrs:对标签属性值的检索字符串，可标注属性检索。  
recursive:是否对子孙全部检索，默认True。  
string: <>...</>中字符串区域的检索字符串。
