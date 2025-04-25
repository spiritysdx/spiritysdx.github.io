# 爬虫流程及方法03(搜索引擎爬取)

```python
#!/usr/bin/env python3

#本篇介绍抓取含搜索引擎的爬虫
#UA检测：门户网站检测对应请求的身份标识
#UA：useragent（请求载体的身份标识）
#UA伪装：伪装游览器
import requests
a = True
while a:
    # UA伪装：伪装游览器,将对应user-agent封装到字典headers中
    headers = {
        'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.122 Safari/537.36'
    }#这里可以使用各种headers，包括ios或者安卓，模拟手机或平板登录
    url = "https://www.sogou.com/web?"
    kw = input('key words:')#输入搜索所需要的关键词
    # step1:处理url携带的参数：封装到字典中
    param = {
        'query': kw
    }
    res = requests.get(url=url, params=param, headers=headers)#几乎所有大型搜索引擎都是get请求，若属于某些私密的网址搜索可能需要post请求等加密传输方式
    # 对指定url发起的请求对应的url是带参数的，请求过程中处理了参数
    page_text = res.text #text处理数据
    filename = kw + '.html'#命名文件
    with open(filename,'w',encoding='utf-8') as fp:
    #该语法自动创建文件并自动打开关闭文件，需要注意的是该文件在本爬虫文件所在文档内，需要更精确的存储位置推荐使用os库
        fp.write(page_text)#输入数据
    print(filename, '保存成功')
    a = False
```
