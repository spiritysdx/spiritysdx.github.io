# 免费破解图片验证码(数字或中英混合)(附代码)(2022)

# 前言

前面有破解谷歌验证码，现在来破解相对简单的图片验证码。(数字以及中英混合)

## 插件过验证码

需要加载谷歌插件[AutoVerify](https://github.com/spiritLHL/Driver_sign/blob/master/AutoVerify.crx)

这种识别是为了过简单的文字+数字的验证图片用的方法。

这个插件能实现点击输入框后自动查找图片自动填入验证码的功能，不需要浏览器级别的操作(右键菜单栏之类的浏览器级别操作)，所以是过普通验证码的不二选择。

当然这个过于依赖插件了，实际可以去使用一些公开的库进行验证码识别，这样速度更快。

比如说以下这些

[https://github.com/sml2h3/ddddocr](https://github.com/sml2h3/ddddocr)

[https://github.com/madmaze/pytesseract](https://github.com/madmaze/pytesseract)

这个识别正确率因人而异。

直接上代码，展示如何加载插件。

```python
# 插件需要放在和py文件的同一目录下
def input_dependence(): # 加载谷歌插件并初始化环境
    global driver, shadow
    # 启动浏览器内核
    opt = ChromeOptions()
    opt.headless = False
    path_e = os.getcwd() + r"\AutoVerify.crx"
    opt.add_extension(path_e)
    opt.add_argument("window-size=1920,1080")
    # opt.add_experimental_option('prefs', prefs)  # 关掉浏览器左上角的通知提示
    # opt.add_argument("disable-infobars")  # 关闭'chrome正受到自动测试软件的控制'提示
    opt.add_argument('--no-sandbox')
    # 设置开发者模式启动，该模式下webdriver属性为正常值
    opt.add_experimental_option('excludeSwitches', ['enable-automation'])
    # opt.add_argument({"extensions.ui.developer_mode": True})
    # opt.add_experimental_option('useAutomationExtension', False)
    # opt.set_preference("extensions.firebug.allPagesActivation", "on")
    opt.add_experimental_option('excludeSwitches', ['enable-logging'])
    ser = Service("chromedriver")
    driver = Chrome(service=ser, options=opt)
    driver.set_page_load_timeout(300)
```

## 免费API实现打码过验证码

列举一些常用的免费API资源

### 国外TrueCaptcha

每日100次，每月合记3000次，需要gmail邮箱注册。每月免费额度最高，适合国外环境以及每日使用。识别成功率较高。

1.truecaptha

注册地址：

[https://apitruecaptcha.org/](https://apitruecaptcha.org/)

查找对应用户的信息(userid和apikey)：

[https://apitruecaptcha.org/api](https://apitruecaptcha.org/api)

使用图像文件形式的图片(png或者jpg)

```python

import requests
import base64
import json

def solve(f): # f是png文件名字，你需要保存验证码为本地图片再进行此操作，如何保存详见下面腾讯云的main函数

    with open(f, "rb") as image_file:
        encoded_string = base64.b64encode(image_file.read())
    #print(encoded_string)
    url = 'https://api.apitruecaptcha.org/one/gettext'

    data = { 'userid':'userid的对应值填这里', 'apikey':'apikey的对应值填这里',  'data':str(encoded_string)[2:-1]}
    r = requests.post(url = url, json = data)
    j = json.loads(r.text)
    return(j)
```

或者使用链接形式的图片(base64码)(推荐上面的用selenium直观点，下面的直接通过BeautifulSoup解析了，需要对网页源码研究透彻)

```python
TRUECAPTCHA_USERID = os.environ.get("TRUECAPTCHA_USERID", "userid的对应值填这里")
TRUECAPTCHA_APIKEY = os.environ.get("TRUECAPTCHA_APIKEY", "apikey的对应值填这里")

def captcha_solver(captcha_image_url: str, session: requests.session) -> dict:
    """
    TrueCaptcha API doc: https://apitruecaptcha.org/api
    Free to use 100 requests per day.
    """
    response = session.get(captcha_image_url)
    encoded_string = base64.b64encode(response.content)
    url = "https://api.apitruecaptcha.org/one/gettext"

    data = {
        "userid": TRUECAPTCHA_USERID,
        "apikey": TRUECAPTCHA_APIKEY,
        # case sensitivity of text (upper | lower| mixed)
        "case": "lower",
        # use human or AI (human | default)
        "mode": "default",
        "data": str(encoded_string)[2:-1],
    }
    r = requests.post(url=url, json=data)
    j = json.loads(r.text)
    return j


def handle_captcha_solved_result(solved: dict) -> str:
    """Since CAPTCHA sometimes appears as a very simple binary arithmetic expression.
    But since recognition sometimes doesn't show the result of the calculation directly,
    that's what this function is for.
    """
    if "result" in solved:
        solved_text = solved["result"]
        if "RESULT  IS" in solved_text:
            log("[Captcha Solver] You are using the demo apikey.")
            print("There is no guarantee that demo apikey will work in the future!")
            # because using demo apikey
            text = re.findall(r"RESULT  IS . (.*) .", solved_text)[0]
        else:
            # using your own apikey
            log("[Captcha Solver] You are using your own apikey.")
            text = solved_text
        operators = ["X", "x", "+", "-"]
        if any(x in text for x in operators):
            for operator in operators:
                operator_pos = text.find(operator)
                if operator == "x" or operator == "X":
                    operator = "*"
                if operator_pos != -1:
                    left_part = text[:operator_pos]
                    right_part = text[operator_pos + 1 :]
                    if left_part.isdigit() and right_part.isdigit():
                        return eval(
                            "{left} {operator} {right}".format(
                                left=left_part, operator=operator, right=right_part
                            )
                        )
                    else:
                        # Because these symbols("X", "x", "+", "-") do not appear at the same time,
                        # it just contains an arithmetic symbol.
                        return text
        else:
            return text
    else:
        print(solved)
        raise KeyError("Failed to find parsed results.")


def get_captcha_solver_usage() -> dict:
    url = "https://api.apitruecaptcha.org/one/getusage"

    params = {
        "username": TRUECAPTCHA_USERID,
        "apikey": TRUECAPTCHA_APIKEY,
    }
    r = requests.get(url=url, params=params)
    j = json.loads(r.text)
    return j
```

### 国内腾讯云，账户需要有实名认证即可

免费的文字识别服务，每个接口每个月1000次免费额度，破解一般用通用印刷体或通用印刷体(高精度)的接口，合计2000次每个月免费次数。每月免费额度一般，泛用性最强，专用性也不差，识别成功率一般。

你需要准备挺多东西的，我一一说一下前期工作。

腾讯云账号一个--->[链接](https://curl.qcloud.com/afnZClZM)

右上角点击注册或登陆(不需要钱，白嫖的服务，有账号就行了)

腾讯云账号对应的访问密钥一对---->[https://console.cloud.tencent.com/cam/capi](https://console.cloud.tencent.com/cam/capi)

新建密钥后把密钥的SecretId和SecretKey记住备用。

腾讯云账号对应开启文字识别服务---->[https://console.cloud.tencent.com/ocr/](https://console.cloud.tencent.com/ocr/)

开通后记得领取免费额度。

本地python环境安装腾讯云的python的sdk：

```bash
pip install tencentcloud-sdk-python
```

好了，准备工作做完了，直接上代码！

```python
from selenium.webdriver import ChromeOptions, Chrome
from selenium.webdriver.chrome.service import Service
from selenium.common.exceptions import TimeoutException
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.common.by import By
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver import ActionChains
from selenium.webdriver.common.keys import Keys
from tencentcloud.common import credential
from tencentcloud.common.profile.client_profile import ClientProfile
from tencentcloud.common.profile.http_profile import HttpProfile
from tencentcloud.common.exception.tencent_cloud_sdk_exception import TencentCloudSDKException
import base64

def pass_ocr(src):
    from tencentcloud.ocr.v20181119 import ocr_client, models
    try:
        cred = credential.Credential(SecretId, SecretKey)
        httpProfile = HttpProfile()
        httpProfile.endpoint = "ocr.tencentcloudapi.com"

        clientProfile = ClientProfile()
        clientProfile.httpProfile = httpProfile
        client = ocr_client.OcrClient(cred, "na-toronto", clientProfile)

        req = models.GeneralAccurateOCRRequest()
        params = {
            "ImageBase64": src,
            "IsPdf": False
        }
        req.from_json_string(json.dumps(params))

        resp = client.GeneralAccurateOCR(req)
        # print(resp.to_json_string())
        # return resp.to_json_string()
        result = resp.to_json_string()
        # 处理验证结果
        temp = []
        for i in json.loads(result)["TextDetections"]:
            y = i["DetectedText"].split(" ")
            try:
                for j in y:
                    temp.append(j)
            except:
                temp.append(i["DetectedText"])
        print(temp)
        cct = "" # 验证后的字符为cct
        for i in temp:
            cct = cct + i
        return cct

    except TencentCloudSDKException as err:
        print(err)


def main():
    driver.switch_to.default_content() # 确保默认在全局中
    WebDriverWait(driver, 20, 0.5).until(
        EC.visibility_of_element_located((By.CSS_SELECTOR, 'css选择器中选中验证码图片位置')))
    element = driver.find_element(By.CSS_SELECTOR, 'css选择器中选中验证码图片位置)')
    try:
        os.remove("origin.png") # 确保删除原有图片
    except Exception as e:
        print(e)
    element.screenshot("origin.png") # 截取验证码图片元素位置并保存为origin.png
    f = open(os.getcwd() + "\\" + "origin.png", 'rb')  # 读取保存的图片
    code_data = base64.b64encode(f.read()).decode('utf-8') # 转码为API能读取的base64格式
    f.close()
    result = pass_ocr(code_data) # 验证
    time.sleep(random.uniform(1, 3))
    WebDriverWait(driver, 20, 0.5).until(
        EC.visibility_of_element_located((By.CSS_SELECTOR, 'css选择器选中的验证码填空框')))
    driver.find_element(By.CSS_SELECTOR, 'css选择器选中的验证码填空框').send_keys(result)
    print("successfully verify captha PNG to str:\n{}".format(result))
```

### 国内百度智能云，账户需要有实名认证即可

一般使用数字打码，或其他对应验证码的API，每个API每个月免费额度为1000次，专用性强，泛用性一般，免费额度一般。识别成功率一般。

开通链接：(事先记得注册和实名认证百度智能云的账号)

[https://cloud.baidu.com/product/ocr_general](https://cloud.baidu.com/product/ocr_general)

开通后记得领取免费额度。

```python
import urllib.request
import re
import base64
import requests

# 这里嫖的别人的代码，自行替换自己的访问密钥谢谢
# 原始仓库: https://github.com/zqtz/verifycode/blob/master/%E5%9B%BE%E5%83%8F%E9%AA%8C%E8%AF%81%E7%A0%81/baidu_api.py
host = "https://aip.baidubce.com/oauth/2.0/token?grant_type=client_credentials&client_id=oa6VVGS7ldI5GG1e3fHrgvB6&client_secret=xdaZFWKnqt2Hsxvnpd2GDo2QNpfGrHLQ&"
response = requests.get(host)
if response:
    access_token = re.findall(r'"access_token":"(.*?)"', response.text)[0]

'''
通用文字识别（高精度版）
'''
request_url = "https://aip.baidubce.com/rest/2.0/ocr/v1/accurate_basic"
# 二进制方式打开图片文件 fetch.jpg
f = open('fetch.jpg', 'rb') # 这里怎么截取验证码图片可以参照上面腾讯云的我咋写的
img = base64.b64encode(f.read())
params = {"image": img}
access_token = access_token
request_url = request_url + "?access_token=" + access_token
headers = {'content-type': 'application/x-www-form-urlencoded'}
response = requests.post(request_url, data=params, headers=headers)
if response:
    print(response.json()['words_result'][0]['words'])
```


### 自建打码，成功率随缘。

项目地址：(没试过，闲的没事干的可以自行测试)

[https://github.com/smxiazi/NEW_xp_CAPTCHA](https://github.com/smxiazi/NEW_xp_CAPTCHA)


# 后言

个人推荐国外环境用TrueCaptcha，国内用腾讯云或百度智能云。

我目前只使用了TrueCaptcha和腾讯云做这方面的打码服务。

付费的话有超级鹰之类的，穷人一个，没那钱买付费服务。

