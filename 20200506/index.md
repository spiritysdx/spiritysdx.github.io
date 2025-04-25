# 爬虫流程及方法14(正则表达式篇)

### 前言

re库的实用实例如下

```python
import requests
import re
import os


a = True
while a:
    #创建一个文件夹，保存所有图片
    if not os.path.exists('./tupianLibs'):
        os.mkdir('./tupianLibs')
    headers = {
        'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.122 Safari/537.36'
    }
    url = "https://www.pexels.com/"
    #使用通用爬虫对整张页面进行爬取
    page_text = requests.get(url=url, headers=headers).text
    #使用聚焦爬虫将页面中所有的图片进行解析/提取
    #正则.*?表示一切内容
    #re.S单行匹配
    ex = '<a class="js-photo-link photo-item__link" style.*? >.*?<img srcset="(.*?)" class.*?></div>'
    image_src_list = re.findall(ex, page_text, re.S )
    for src in image_src_list:
        src = 'https:'+ src
        #拼接出一个完整的图片url
        image_data = requests.get(url=src, headers=headers).content
        #请求到了图片的二进制数据
        image_name = src.split('/')[-1]
        #生成图片名称
        imgPath = './tupianlbs/' + image_name
        #图片最终存储的路径
        with open(imgPath, 'W') as fp:
            fp.write(image_data)
            print('下载成功')
a = False
```

### 正则表达式详情

![1](https://pic./2020/05/03/0baa55fd80836.png)

![2](https://pic./2020/05/03/9b38f52cd276f.png)

raw string类型区别于原生字符串类型（不包含转义字符）

```python
r'[1-9]\d{5}'
```

![3](https://pic./2020/05/03/2b21d0b2e8d13.png)

```python 
re.search(pattern, string, flags=0)
```
在一个字符串中搜索匹配正则表达式的第一个位置，返回match对象。
```
    pattern:正则表达式的字符串或原生字符串表示
    string:待匹配字符串
    flags:正则表达式使用时的控制标记
```

![5](https://pic./2020/05/03/f1ab052345ee9.png)


```python
re.split(pattern, string, maxsplit=0, flags=0)   
```
将一个字符串按照正则表达式匹配结果进行分割,返回列表类型。   
```
    pattern:正则表达式的字符串或原生字符串表示   
    string:待匹配字符串   
    maxsplit:最大分割数，剩余部分作为最后一个元素输出  
    flags:正则表达式使用时的控制标记  
```

```python
re.finditer(pattern, string, flags=0)   
```

搜索字符串，返回一个匹配结果的迭代类型，每个迭代元素是match对象。  
```
    pattern:正则表达式的字符串或原生字符串表示  
    string:待匹配字符串  
    flags:正则表达式使用时的控制标记  
```

```python
re.sub(pattern, repl, string, count=0, flags=0)   
```
在一个字符串中替换所有匹配正则表达式的子串，返回替换后的字符串。  
```
    pattern:正则表达式的字符串或原生字符串表示  
    repl:替换匹配字符串的字符串  
    string:待匹配字符串  
    flags:正则表达式使用时的控制标记  
```

```python
regex = re.compile(pattern, flags=0)  
```
将正则表达式的字符串形式编译成正则表达式对象  
```
    pattern:正则表达式的字符串或原生字符串表示  
    flags:正则表达式使用时的控制标记  
    Match:对象的属性  
```

![6](https://pic./2020/05/03/d4ce22e3595ae.png)

```python
 match = re.search(r'PY.*N',' PYANBNCNDN' )  
 match.group(0)  
```

Re库默认采用贪婪匹配，即输出匹配最长的子串。   

输出'PYANBNCNDN'   

```python
match = re.search(r'PY. *?N', ' PYANBNCNDN')  
match group(0)  
```

输出'PYAN '  

![7](https://pic./2020/05/03/c49378c91f34c.png)

```python
import re
restr="<td data-v-80203e10="">（\\d+）</td>"#括号表示只取数据（数字）
regex=re.compile(restr,re.IGNORECASE)
mylist=regex.findall(province)
```

