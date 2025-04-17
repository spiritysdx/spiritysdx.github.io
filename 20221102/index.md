# Python破解Google验证码ReCaptchav3的成功案例(附代码)(免费)(2022)

# 前言

破解谷歌验证码，其实并不需要用高深的法子，只需要借助一些免费资源，免费插件，就能达到很好的效果。

网上现在流传的几乎都说Google验证码ReCaptchav3得用深度学习，或者第三方收费打码网站，又或者抓住某些验证漏洞才能过，实际上，并不需要。这里我先从普通的验证码开始，介绍我破解Google验证码的思路以及个人成功案例。(全网独家，目前我各大搜索引擎找遍了都没有的，如需转发请注明本文来源)

### 验证码识别(数字字符混合图片)

1.需要加载谷歌插件[AutoVerify](https://github.com/spiritLHL/Driver_sign/blob/master/AutoVerify.crx)

这种识别是为了过简单的文字+数字的验证图片用的方法。

这个插件能实现点击输入框后自动查找图片自动填入验证码的功能，不需要浏览器级别的操作(右键菜单栏之类的浏览器级别操作)，所以是过普通验证码的不二选择。

当然这个过于依赖插件了，实际可以去使用一些公开的库进行验证码识别，这样速度更快。

比如说以下这些

[https://github.com/sml2h3/ddddocr](https://github.com/sml2h3/ddddocr)

[https://github.com/madmaze/pytesseract](https://github.com/madmaze/pytesseract)

这个识别正确率因人而异。

2.使用公开的API进行验证码识别，需要各种平台账户，这部分将在我的另一篇博客中总结。

(具体使用方法跟我过谷歌验证的reCAPTCHA的第二路线有异曲同工之妙)

### 过谷歌验证(V3)reCAPTCHA(V3)的路线

两个路线，跟上面的想法也是类似的。

1.直接通过插件过验证，但需要你点击某个特定按钮显示特定内容才能过。优点是只需要点点点，配置简单，速度极快，缺点是通过率80%左右(国内环境)，点的过程中需要点击shadow-root(closed)里的按钮，而且可能触发谷歌的人机识别，导致IP短暂进入黑名单不给后续验证。

2.通过第三方接口。优点是不需要插件，通过率几乎100%，不易触发谷歌的人机识别，缺点是配置比较麻烦，而且你的第三方接口账号有一定的风险(几乎忽略不计，只要你不泄露密钥)。

### 代码

#### 1.先说插件版本的路线，直接上代码，具体内容看注释

插件地址[buster.crx](https://github.com/spiritLHL/extend_mode/blob/main/buster.crx)

```python
# by spiritlhl
import random
import time
import os
from selenium.webdriver import ChromeOptions, Chrome
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.common.by import By
from selenium.common.exceptions import TimeoutException
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver import ActionChains
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.support.ui import WebDriverWait as wait
from selenium.webdriver import DesiredCapabilities
from pyshadow.main import Shadow


# 加载依赖
def input_dependence():
    global driver, shadow
    # 启动浏览器内核
    opt = ChromeOptions()
    opt.headless = False # 有头模式才能加载插件，无头模式无法加载插件
    path_e = os.getcwd() + r"\buster.crx" # 插件放同一文件夹下，加载
    opt.add_extension(path_e) # 通过配置参数加载
    # 为了提高成功率，需要随机浏览器指纹，当然也可以不随机，此时建议同IP或地域运行，避免不同IP或不同地域同浏览器指纹的奇怪情况
#     path_e = os.getcwd() + r"\random-user-agent.crx"
#     opt.add_extension(path_e)
#     path_e = os.getcwd() + r"\canvas-fingerprint-blocker.crx"
#     opt.add_extension(path_e) 
    opt.add_argument("--start-maximized") # 最大化浏览器窗口，避免谷歌验证检测出默认大小判定机器
    # 设置开发者模式启动，该模式下webdriver属性为正常值
    opt.add_argument('--no-sandbox') # 这一串在linux下有用，win下没啥大用
    # 下面几个参数开启开启插件的功能避免导入后无法显示使用
    opt.add_experimental_option('excludeSwitches', ['enable-automation'])
    opt.add_experimental_option('excludeSwitches', ['enable-logging'])
    opt.add_argument('--disable-blink-features=AutomationControlled')
    # 配置谷歌浏览器的chromedriver，我的在同一目录下，直接写名字即可
    ser = Service("chromedriver")
    driver = Chrome(service=ser, options=opt)
    # 加载阴影模块，有需要GitHub搜一下pyshadow库，在shadow-root为open的状态下好用
    shadow = Shadow(driver)
    # 设置加载页面超时时长
    driver.set_page_load_timeout(300)
    return driver

# 在谷歌浏览器配置界面修改插件默认配置(很重要，不然你插件加载了但不在所有网站启用，照样白加载了)
def change_seeting():
    global driver, shadow
    element = shadow.find_element("#devMode") # pyshadow库真是个好东西，点谷歌配置shadow-root(open)按钮的不二之选
    element.click()
    time.sleep(3)
    element = shadow.find_element('#detailsButton')
    time.sleep(3)
    element.click()
    time.sleep(10)
    element = shadow.find_element('#host-access')
    element.click()
    all_options = element.find_elements(By.TAG_NAME, "option")
    for option in all_options: # 第三个选项适配所有网站
        try:
            print(translator.trans("选项显示的文本 "), translator.trans(option.text))
            print(translator.trans("选项值为 "), translator.trans(option.get_attribute("value")))
        except:
            print(translator.trans("选项显示的文本 "), option.text)
            print(translator.trans("选项值为 "), option.get_attribute("value"))
    time.sleep(2)
    option.click()
    element = shadow.find_element('#centeredContent')
    time.sleep(2)
    element.click()
    time.sleep(2)
    element = shadow.find_element('#closeButton')# 一定要点返回，不然白改选项了，不返回不保存，你直接跳转别的页面也没保存的！！！
    time.sleep(2)
    element.click()
    time.sleep(2)

def pass_recaptha():
    global driver
    # 点击验证
    # 这一句是跳转验证框内，不然在iframe外无法对iframe内元素进行操作
    driver.switch_to.frame(driver.find_element(By.XPATH,
                                               '/html/body/main/div/div/div[2]/div/div/div/div/div/form/div[3]/div/div/div/iframe'))
    # 等待那个勾选框加载，当可点击时再点击
    WebDriverWait(driver, 10).until(EC.element_to_be_clickable((By.CSS_SELECTOR, "span#recaptcha-anchor"))).click()
    # 返回默认的界面，跳出iframe，此时需要加载进入另一个iframe
    driver.switch_to.default_content()
    time.sleep(1)
    # 加载进入另一个iframe
    element = driver.switch_to.frame(driver.find_element(By.XPATH, '/html/body/div[10]/div[4]/iframe'))
    time.sleep(random.uniform(3, 5))
    # 选中buster插件需要使用的第三个黄色无障碍按钮
    driver.find_element(By.CSS_SELECTOR,
                        '#rc-imageselect > div.rc-footer > div.rc-controls > div.primary-controls > div.rc-buttons > div.button-holder.help-button-holder')
    # 下面的操作是使用tab键盘事件选中按钮，enter键盘事件点击，因为该按钮不在open的shadow-root中，在shadow-root(closed)中，必须使用这种操作点击，否则其他任何方法都无法奏效，pyshadow也不能
    Ac = ActionChains(driver)
    Ac.send_keys(Keys.TAB).perform()
    Ac.send_keys(Keys.ENTER).perform()
    print(translator.trans("点击了"))
    # 点击完毕后，页面跳转到无障碍页面，此时有可能需要再次点击无障碍按钮使得插件运作(我尝试多次，有可能需要有可能不需要，自己判别)
    time.sleep(3)
    driver.find_element(By.CSS_SELECTOR,
                        'body > div > div > div.rc-footer > div.rc-controls > div.primary-controls > div.rc-buttons > div.button-holder.help-button-holder')
    Bc = ActionChains(driver)
    Bc.send_keys(Keys.TAB).perform()
    Bc.send_keys(Keys.ENTER).perform()
    time.sleep(5)
    print("再次点击无障碍按钮")

# 此时如果插件正常运作，应该已经自动过验证了，但我自己测试的过程中过的几率低，有可能是因为我在大陆内的原因，无法连接顺畅(Github的Actions中倒是成功率极高，几乎没有不过的，大概率肯定网络原因了)
# 正常运作后需要driver.switch_to.default_content()回到原始界面进行别的操作，否则你的无法操作iframe外的元素
```

附上那个无障碍按钮的源码图，在shadow-root(closed)里的按钮，真的只能通过键盘事件进行操作了。

![](https://camo.githubusercontent.com/a24bbd24ba25ed89d2a6d2e15dae55dbe0ec619f864f8f0258265f5422bc0310/68747470733a2f2f63646e2e6a7364656c6976722e6e65742f67682f7370697269744c484c2f7475636875616e67406d61737465722f31353933484e585246445630584744372535444d4f253544427e312e706e67)

附上pyshadow的Github仓库，操作open状态下的shadowroot的不二选择：

[https://github.com/sukgu/pyshadow](https://github.com/sukgu/pyshadow)

#### 2.第三方接口路线(个人尤其推荐)

你需要准备挺多东西的，我一一说一下前期工作。

腾讯云账号一个--->[链接](https://curl.qcloud.com/afnZClZM)

右上角点击注册或登陆(不需要钱，白嫖的服务，有账号就行了)

腾讯云账号对应的访问密钥一对---->[https://console.cloud.tencent.com/cam/capi](https://console.cloud.tencent.com/cam/capi)

新建密钥后把密钥的SecretId和SecretKey记住备用。

腾讯云账号对应开启语音识别服务---->[https://console.cloud.tencent.com/asr](https://console.cloud.tencent.com/asr)

免费的语音识别服务，每个月5小时免费时长，识别一年都用不了5小时吧。。。

本地python环境安装腾讯云的python的sdk：

```bash
pip install tencentcloud-sdk-python
```

好了，准备工作做完了，直接上代码！

```python
# by spiritlhl
import random
import time
import json
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
from tencentcloud.asr.v20190614 import asr_client, models


SecretId = "腾讯云账号的SecretId"
SecretKey = "腾讯云账号的SecretKey"

def get_result(id_d):
    try:
        cred = credential.Credential(SecretId, SecretKey)
        httpProfile = HttpProfile()
        httpProfile.endpoint = "asr.tencentcloudapi.com"

        clientProfile = ClientProfile()
        clientProfile.httpProfile = httpProfile
        client = asr_client.AsrClient(cred, "", clientProfile)

        req = models.DescribeTaskStatusRequest()
        params = {
            "TaskId": id_d
        }
        req.from_json_string(json.dumps(params))

        resp = client.DescribeTaskStatus(req)
        # print(resp.to_json_string())
        if json.loads(resp.to_json_string())["Data"]["StatusStr"] == "waiting" or json.loads(resp.to_json_string())["Data"]["StatusStr"] == "doing":
            return False
        try:
            json.loads(resp.to_json_string())["Data"]["Result"].split("]")[-1][2:]
        except:
            # print(json.loads(resp.to_json_string()))
            return False
        return json.loads(resp.to_json_string())["Data"]["Result"].split("]")[-1][2:-1]

    except TencentCloudSDKException as err:
        print(err)
        return False

# 上传音频链接msg_url
def upload(msg_url):
    try:
        cred = credential.Credential(SecretId, SecretKey)
        httpProfile = HttpProfile()
        httpProfile.endpoint = "asr.tencentcloudapi.com"

        clientProfile = ClientProfile()
        clientProfile.httpProfile = httpProfile
        client = asr_client.AsrClient(cred, "", clientProfile)

        req = models.CreateRecTaskRequest()
        params = {
            "EngineModelType": "16k_en",
            "ChannelNum": 1,
            "ResTextFormat": 0,
            "SourceType": 0,
            "Url": msg_url
        }
        req.from_json_string(json.dumps(params))

        resp = client.CreateRecTask(req)
        # print(resp)
        ID = json.loads(resp.to_json_string())["Data"]["TaskId"]
        # print(ID)
        count = 0
        while True:
            st = get_result(int(ID))
            if st != False:
                break
            time.sleep(0.7)
            count += 1
            if count > 120:
                return
        print(st)
        return st
    except TencentCloudSDKException as err:
        print(err)

# 关键点，上面两函数直接套着用就行了，不用管，这里是重点
def pass_recaptha():
    global driver
    # 点击验证
    # 等待加载上验证框，验证框iframe被套在一个form中
    WebDriverWait(driver, 15, 0.5).until(
        EC.visibility_of_element_located((By.XPATH,
                                               '//*[@id="form-submit"]/div[2]/div/div/div/iframe')))
    # 进入验证框所在iframe
    driver.switch_to.frame(driver.find_element(By.XPATH,
                                               '//*[@id="form-submit"]/div[2]/div/div/div/iframe'))
    print(translator.trans("加载验证中"))
    # 等待勾选框可点击再点击
    WebDriverWait(driver, 15, 0.5).until(EC.element_to_be_clickable((By.CSS_SELECTOR, "#recaptcha-anchor"))).click()
    # 随机2~3秒避免加载不出来
    time.sleep(random.uniform(2, 3))
    print(1)
    try:
        # 回到默认页面
        driver.switch_to.default_content()
        # 等待点击勾选框后的弹窗界面iframe有没有加载出来
        WebDriverWait(driver, 15, 0.5).until(
            EC.visibility_of_element_located((By.XPATH, '/html/body/div[10]/div[4]/iframe')))
        # 进入弹窗界面的iframe
        driver.switch_to.frame(driver.find_element(By.XPATH, '/html/body/div[10]/div[4]/iframe'))
        print(2)
        # 等待语音按钮是否加载出来，注意，这里在shadow-root里面，不可以直接用css选择器或xpath路径点击
        WebDriverWait(driver, 30, 0.5).until(
            EC.visibility_of_element_located((By.XPATH, '//*[@id="recaptcha-audio-button"]')))
        print(3)
        # 选中语音按钮
        driver.find_element(By.XPATH, '//*[@id="recaptcha-audio-button"]')
        # 初始化键盘事件
        Ac = ActionChains(driver)
        # tab按键选中
        Ac.send_keys(Keys.TAB).perform()
        # enter按键点击
        Ac.send_keys(Keys.ENTER).perform()
        print(translator.trans("点击了语音按钮"))
        # 等待页面跳转出现下载按钮，跳转后会出现语音下载按钮，需要捕获它的href值，它就是音频链接msg_url
        WebDriverWait(driver, 30, 0.5).until(
            EC.visibility_of_element_located((By.XPATH, '/html/body/div/div/div[7]/a')))
        msg_url = driver.find_element(By.XPATH, '/html/body/div/div/div[7]/a').get_attribute("href") # 获取链接
        print(4)
        result1 = upload(msg_url) # 上传链接返回结果
        time.sleep(1)
        print(5)
        print(translator.trans("识别结果为："))
        print(result1)
        # 等待加载填写框
        WebDriverWait(driver, 15, 0.5).until(
            EC.visibility_of_element_located((By.XPATH, '//*[@id="audio-response"]')))
        # 选中填写框
        driver.find_element(By.XPATH, '//*[@id="audio-response"]').send_keys(result1)
        print(6)
        # 随机时长，避免判断为机器人
        time.sleep(random.uniform(1.5, 3))
        # 等待加载verify验证按钮
        WebDriverWait(driver, 15, 0.5).until(
            EC.element_to_be_clickable((By.XPATH, '//*[@id="recaptcha-verify-button"]')))
        # 选中点击verify按钮
        driver.find_element(By.XPATH, '//*[@id="recaptcha-verify-button"]').click()
        print(translator.trans("语音验证通过"))
    except Exception as e:
        print(e)
    print(7)
    # 回到初始页面，进行下一步操作
    driver.switch_to.default_content()
    print(8)
```

这样子就通过腾讯云提供的免费语音识别过了谷歌验证，每次识别只需要识别不到30秒的音频，然后结果在10秒内出来，正确率几乎百分百，我暂时没碰到过没验证出来的。(每次几乎都在6~8秒内过，从上传到出结果)

这种法子风险在于如果你部署的环境不是私密环境，你的腾讯云账号访问密钥(等同于你的账号密码)就容易泄露，所以建议确认环境的安全性后再使用本方法。

当然如果使用Github的Actions，又是私库，那就没有这种顾虑了。

# 后言

当然，还有第三种法子，这时候就需要你用更高级的深度学习以及cv图像识别进行了。

个人使用免费API过的多一点，插件用不习惯，判断是否成功过了验证很麻烦。

付费的话有很多国外网站做，谷歌一搜一大把。我穷人一个，没那钱买付费服务。
