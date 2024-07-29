# 爬虫流程及方法08(BeautifulSoup实例)(非ajax请求)



```python
import requests
from bs4 import BeautifulSoup


a = True
while a:
    headers = {
        'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.122 Safari/537.36'
    }
    params = {
        '_v': '5.12.0'
    }
    fp = open('./萌新论坛爬虫.text', 'w', encoding='utf-8')
    urls = 'http://www.lolichan.vip/'
    response = requests.get(url=urls, params=params, headers=headers).text
    soup = BeautifulSoup(response, 'lxml')
    class_list= soup.select('.node-title')
    for li in class_list:
        try:
            detail_url = 'http://www.lolichan.vip/' + li.a['href']
            detail_page_text = requests.get(url=detail_url, params=params, headers=headers).text
            detail_soup = BeautifulSoup(detail_page_text, 'lxml')
            page_list = detail_soup.select('.structItem-title')
            print('抓取页面成功')
        except:
            page_list = '-----'
        for i in page_list:
            try:
                page_title = i.a.string
                page_url = 'http://www.lolichan.vip/' + i.a['href']
                page_text = requests.get(url=page_url, headers=headers).text
                detail_soup = BeautifulSoup(page_text, 'lxml')
                div_tag = detail_soup.find('div', class_='bbWrapper')
                content = div_tag.text
                fp.write(page_title + ':' + content + '\n')
                print('爬取页面成功')
            except:
                continue
    a = False
```
ajax请求请参考[爬虫流程及方法09(动态加载页面)(ajax请求)(Json实例)](https://spiritlhl.github.io/blog/%E7%88%AC%E8%99%AB09/)
