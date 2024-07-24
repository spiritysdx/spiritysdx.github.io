# 比较简单的selenium自动化操作播放bilibili(b站)视频2020


## 直接放代码

```python
from selenium import webdriver
import time

driver = webdriver.Chrome(r'C:\chromedriver.exe')
urllist = [
    'https://www.bilibili.com/video/BV15f4y1m7xH?from=search&seid=9788956603997309480',
    'https://www.bilibili.com/video/BV1WA411h76h?from=search&seid=9738279009337231611',
    'https://www.bilibili.com/video/BV13c411h7k7?from=search&seid=9738279009337231611',
    'https://www.bilibili.com/video/BV1x541147u8?from=search&seid=9738279009337231611',
    'https://www.bilibili.com/video/BV17p4y1C78w?from=search&seid=9738279009337231611'
]
#视频链接
timelist=[
    311,
    598,
    669,
    568,
    507,
]
#放入自己各个视频的时长

t = 0

for url in urllist:
    try:
        driver.set_page_load_timeout(5)
        driver.get(url)
        time.sleep(10)
    except Exception :
        print("timeout")

    element = driver.find_element_by_xpath('//*[@id="bilibiliPlayer"]/div[1]/div[1]/div[10]/div[2]/div[2]/div[1]/div[1]/button[1]')#xpath抓取播放控件
    time.sleep(5)
    print('控件抓取成功')
    driver.find_element_by_xpath('//*[@id="bilibiliPlayer"]/div[1]/div[1]/div[10]/div[2]/div[2]/div[1]/div[1]/button[1]').click()#xpath定位成功后点击播放
    print('播放成功')
    time.sleep(timelist[t])
    print('下一个视频')
    t = t + 1
```
因为页面加载需要时间，抓取控件也需要时间，设计sleep时长看你的页面加载速度以及网速进行调整

链接[https://b23.tv/D0a1BX](https://b23.tv/D0a1BX)是我完善后的源码效果视频，视频评论区里有完善后的源码链接，视频点赞自取。
