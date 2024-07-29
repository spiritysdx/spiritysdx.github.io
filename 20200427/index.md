# 爬虫流程及方法09(动态加载页面)(ajax请求)(Json实例)

动态加载数据ajax   
### 首页中对应企业数据通过ajax请求得到   
详情页url只有id不同其余相同   

id从json中获取，域名与id拼接新url   

详情页的数据也是动态加载出来的   

详情页的url也是相同的只有id不同   

爬取的原网站[药妆局](http://125.35.6.84:81/xk/)  

推荐使用chrome网页抓包工具查看请求类型  
![response](https://pic./2020/04/29/27a9fc59752b1.png)
首页与详情页都使用post请求传输json数据包，需要json解析 
```python
import requests
import json

a = True
while a:
    headers = {
        "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.122 Safari/537.36"
    }
    #批量获取企业详情页对应id
    url = "http://125.35.6.84:81/xk/itownet/portalAction.do?method=getXkzsList"
    id_list = []  # 存储企业id
    all_data_list = []
    #参数封装
    for page in range(1,6):
        data = {
            'on': 'true',
            'page': page,
            'pageSize': ' 15',
            'productName': '',
            'conditionType': ' 1',
            'applyname': '',
            'applysn': ''
        }
        json_ids = requests.post(url=url, headers=headers,data=data).json()
        for i in json_ids['list']:
            id_list.append(i['ID'])
    # 存储所有企业详情数据
    post_url = "http://125.35.6.84:81/xk/itownet/portalAction.do?method=getXkzsList"
    for id in id_list:
        data = {
            'id': id
        }
        detail_json = requests.post(url=post_url, headers=headers, data=data).json()
        all_data_list.append(detail_json)
    #持久化存储
    fp = open('./alldata.json','w',encoding='utf-8')
    json.dump(all_data_list, fp=fp, ensure_ascii=False)
    print('over')


    a = False
```
总结：爬虫检查网页的传输方式，选择get请求还是post请求，注意是否为ajax请求，注意解析数据包的格式
