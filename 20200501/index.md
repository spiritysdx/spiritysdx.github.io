# 爬虫流程及方法12(高性能异步爬虫)


### 前言

目的:在爬虫中使用异步实现高性能的数据爬取操作。   
异步爬虫的方式: 
--多线程，多进程(不建议):
     好处:可以为相关阻塞的操作单独开启线程或者进程，阻塞操作就可以异步执行。   
    弊端:无法无限制的开启多线程或者多进程。   

ps:get方法与post方法是阻塞的方法

--线程池、进程池(适当的使用)：
    好处:我们可以降低系统对进程或者线程创建和销毁的一个频率，从而很好的降低系统的开销。
    弊端:池中线程或进程的数量是有上限。|

### 模拟多线程操作

单线程模拟：

```python
import time
#使用单线程串行方式执行


def get_page(str):
    print("正在下载: ", str)
    time.sleep(2)
    print('下载成功: ', str)
name_list = ['xiaozi', 'aa','bb','cc']
start_time = time.time()
for i in range(len(name_list)):
    get_page(name_list[i])
end_time = time.time()
print('%d second' % (end_time-start_time))
```

结果:8s

线程池模拟：  

```python
import time
#导入线程池模块对应的类
from multiprocessing.dummy import Pool
#使用线程池方式进行
#导入线程池所对应的pool


start_time = time.time()#程序开始时计时

def get_page(str):
    print("正在下载: ", str)
    time.sleep(2)
    print('下载成功: ', str)

name_list = ['xiaozi', 'aa','bb','cc']#可迭代对象

#实例化一个线程对象
pool = Pool(4)#线程池开启4个线程

#将列表中每一个列表元素传递给get_page进行处理
pool.map(get_page, name_list)#若有返回值返回的是列表，因为多次传入到map
end_time = time.time()#程序结束时结束计时
print(end_time-start_time)
```

结果:2s

### 实际案例

提取js动态加载内容，使用re正则匹配

*js源码*

```
var contId="1671755",liveStatusUrl="liveStatus.jsp",liveSta="",playSta="1",autoPlay=!1,isLiving=!1,isVrVideo=!1,hdflvUrl="",sdflvUrl="",hdUrl="",sdUrl="",ldUrl="",srcUrl="https://video.pearvideo.com/mp4/third/20200429/cont-1671755-11742488-084919-hd.mp4",vdoUrl=srcUrl,skinRes="//www.pearvideo.com/domain/skin",videoCDN="//video.pearvideo.com"
```

*正则匹配*

ex =  'srcUrL="( .*? )”,vdoUrl'  

分组操作提取链接

```python
import requests
from bs4 import BeautifulSoup
import re
from multiprocessing import Pool



def get_video_data(dic):
    headers = {
        'User-Agent': "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.122 Safari/537.36"
    }
    #使用线程池对视频数据进行请求（较为耗时的堵塞操作）
    url = dic['url']
    print(dic['name'], '正在下载')
    data = requests.get(url=url, headers=headers, timeout=0.5).content
    #持久化存储操作
    with open(dic['name'], 'wb') as fp:
        fp.write(data)
        print(dic['name'], '下载成功')

if __name__ == '__main__':
    headers = {
        'User-Agent':"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.122 Safari/537.36"
    }
    url = 'https://www.pearvideo.com/category_5'
    page_text = requests.get(url=url, headers=headers).text
    soup = BeautifulSoup(page_text, 'lxml')
    li_urls = soup.select('.vervideo-bd')
    urls = []#存储所有视频的链接和名字
    i = 1
    for li in li_urls:
        try:
            i = i + 1
            detail_url = 'https://www.pearvideo.com/' + li.a['href']
            name = soup.select('.vervideo-title')[i].text+'.mp4'
            detail_page_text = requests.get(url=detail_url, headers=headers, timeout=0.5).text
            ex = 'srcUrl="(.*?)",vdoUrl'
            video_url = re.findall(ex, detail_page_text)[0]
            dic = {
             'name': name,
             'url': video_url
             }
            urls.append(dic)
        except:
            continue

    pool = Pool(4)
    pool.map(get_video_data, urls)
    pool.close()
    pool.join()

```
总结：

1.windows环境下需要将主函数放在以下代码下方

```python
if __name__ == '__main__':
```

mac环境下不需要此操作

2.下载时下载二进制数据，使用'wb'而不是'w'。

3.如果下载视频过多(爬取大量数据)，网站要求验证证书，大量爬取需要使用其他方法应对ssl反爬策略。

ps：感谢csdn学院提供的案例支持
