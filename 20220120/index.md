# 白嫖永久的免费VPS云服务器(2022.1.16已报废)

## 2022.1.16

Euserv激活账号以及安装机子需要钱了，无法白嫖了。。。

但是有别的机子有类似机制能白嫖，详情请看

[https://github.com/spiritLHL/Hang-up-items](https://github.com/spiritLHL/Hang-up-items)

原有已经嫖到的自动续期脚本还是有效的。

## 前言

2022.1.5更新图片链接以及部分说明

2021.11.26更新教程，解决11.6增加了邮件pin验证的问题

每个号限量一台，申请审核制，没耐心的不推荐嗷！

如果想注册多账户，建议更换IP和浏览器嗷，环境得变一下，不然会被识别出已注册过，直接给你封号的！

服务器仅支持装linux系统，没有需求建议别撸，把机会留给有需要的人。

都是个人经验，如果还是不会可以--->[点击联系我](http://wpa.qq.com/msgrd?v=3&uin=2461006717&site=qq&menu=yes)，有时间或许会帮助一下

## 前期准备

**需要一个国际邮箱，什么QQ邮箱163邮箱是不能用的，推荐gmail或yandex邮箱**

相关注册地址如下：

[gmail注册地址](https://accounts.google.com/signin/v2/identifier?hl=en&continue=https%3A%2F%2Fmail.google.com%2Fmail&service=mail&ec=GAlAFw&flowName=GlifWebSignIn&flowEntry=AddSession)

[yandex注册地址](https://passport.yandex.com/registration?retpath=https%3A%2F%2Fmail.yandex.com&process_uuid=80a1d5e5-009d-4651-ac90-2c077b9d4a18)

个人推荐yandex邮箱，因为它可以国内直连，方便随时查看信息，gmail就有点麻烦，得拥有一个谷歌账户才行。

yandex注册需要手机号，手机号前加```+86```就能收到验证码了。

yandex邮箱国内直连就能上，很香。

**需要随机的没有被使用过的个人英文信息和英文地址信息**

这里比较重要，推荐使用符合自己登陆ip或离登陆ip非常接近的地区的随机生成的虚拟信息，尤其重要的是得是```全英文```的，不然必然过不了审核。

我自己使用香港IP注册的，所以使用的是随机生成的虚拟香港个人信息与地址。

随机虚拟香港个人信息：

[虚拟香港个人信息](https://www.meiguodizhi.com/hk-address)

推荐随机香港信息，注意一定要按教程写，不然你的账户会被自动注销，得重新注册！！！

**需要一个支持IPV6的环境**

自己测试一下，如果不支持IPV6的话你登不上SSH，此时建议使用手机流量开热点给电脑，手机流量一般是有IPV6环境的。

[IPV6环境测试网站](http://www.test-ipv6.com/)

测试成功后是这个样子的：

![](https://spiritlhl-tc.oss-cn-beijing.aliyuncs.com/YXUXP_4829VW$S$FXK%60IY%5BI.png)

到这里基本的东西已经完事了，下面开始正式开撸！

## 注册账户

官网地址：[http://www.euserv.de/](http://www.euserv.com/?pk=9b15448161583a1)

界面如下：

![](https://cdn.jsdelivr.net/gh/spiritLHL/tuchuang@master/20220105205959.png)

点击后注册

![](https://cdn.jsdelivr.net/gh/spiritLHL/tuchuang@master/20220105210128.png)

输入你的国际邮箱，点击发送PIN码到你的邮箱进行验证

![](https://s3.bmp.ovh/imgs/2021/08/5854d7ef9fae3166.png)

将PIN码输入后点击验证

(邮箱可能将该信息整到广告垃圾信息里了，得翻一下那块)

![](https://s3.bmp.ovh/imgs/2021/08/4b18a98e540e770c.png)

验证后需要你设置密码和用户信息啥的，自己设置吧，记得是英文，密码得包含数字，字母。

![](https://cdn.jsdelivr.net/gh/spiritLHL/tuchuang@master/20220105210226.png)

**然后下面就是重中之重了，填写账户信息！能不能成功就看这个！**

点击下面这个链接登陆客户面板。

[https://support.euserv.com/](https://support.euserv.com/)

然后它会要求你填写详情信息，重点来了嗷！

![](https://cdn.jsdelivr.net/gh/spiritLHL/tuchuang@master/20220105210321.png)

**重点一，这个街道地址和街道号，我找的实际的香港街道地址和对应的街道号，这个一定得找真实的！**

查找香港英文地址链接：

[https://www.hongkongpost.hk/correct_addressing/index.jsp?lang=zh_TW](https://www.hongkongpost.hk/correct_addressing/index.jsp?lang=zh_TW)

在里面查询关键词"新界"之类的，然后使用第二列的街道名称中的名字和街道号，对应第一个箭头的框，第一个框是街道名字，第二个框是街道号。

**重点二，城市名称和邮政编码，这个照着虚拟信息填！**

随机虚拟香港个人信息：

[https://www.meiguodizhi.com/hk-address](https://www.meiguodizhi.com/hk-address)

对应第二个箭头，第一个框是邮政号，第二个框是城市名称。

**重点三，个人手机号，这个照着上面生成的电话号填！**

生成的电话号是```XXX-XXXXXXX```的形式，对应第三个箭头的两个空格分别填前半部分和后半部分，```+86```是已经有的，不用改。

第四个箭头里的值不用改，默认就行。

注册完后就能正常使用用户面板了。

**记得全英文！不能有汉字！一定得对应做！不然后面审核过不了号就废了！**

## 购买免费的VPS云服务器

申请地址：[https://www.euserv.com/en/virtual-private-server/root-vserver/v2/vs2-free.php](https://www.euserv.com/en/virtual-private-server/root-vserver/v2/vs2-free.php)

也可以登录官网，找到“vServer VS2”会看到“VS2-free”点击申请即可。

![](https://cdn.jsdelivr.net/gh/spiritLHL/tuchuang@master/20220105210412.png)

![](https://cdn.jsdelivr.net/gh/spiritLHL/tuchuang@master/20220105210443.png)

![](https://cdn.jsdelivr.net/gh/spiritLHL/tuchuang@master/20220105210512.png)

![](https://cdn.jsdelivr.net/gh/spiritLHL/tuchuang@master/20220105210546.png)

![](https://cdn.jsdelivr.net/gh/spiritLHL/tuchuang@master/20220105210633.png)

![](https://cdn.jsdelivr.net/gh/spiritLHL/tuchuang@master/20220105210659.png)

![](https://cdn.jsdelivr.net/gh/spiritLHL/tuchuang@master/20220105210726.png)

需要等待几个小时审核，官方说24~48小时审核完成，我实际6小时不到就审核完毕了。

审核完后是这个样子

![](https://gitee.com/spiritlhl/picture/raw/master/@$P6DRAPTN$@%7D%7D~$0G%7DMD_E.png)

服务器有效期一个月，续期大概在你使用的第20天开始可以续期下一个月，下面有自动化脚本部署到腾讯云函数(免费)来实现自动续期。

## 自动续期的腾讯云函数(免费)脚本

仓库链接：

(已反代理加速，直连点击即可，无法登陆账号)

[老仓库，只有issues值得一看](https://git.spiritlhl.workers.dev/o0oo0ooo0/EUserv_extend)

2021.11.19更新日志：原仓库脚本↑已经失效，使用下方我改的脚本部署云函数，自测yandex邮箱注册的账号无问题↓（点开python栏）记得修改里面的信息

```python
#! /usr/bin/env python3

#
# SPDX-FileCopyrightText: (c) 2020-2021 CokeMine & Its repository contributors
# SPDX-FileCopyrightText: (c) 2021 A beam of light
#
# SPDX-License-Identifier: GPL-3.0-or-later
#

"""
euserv auto-renew script
       v2021.09.30
* Captcha automatic recognition using TrueCaptcha API
* Email notification
* Add login failure retry mechanism
* reformat log info
       v2021.11.06
* Receive renew PIN(6-digits) using mailparser parsed data download url
  workflow: auto-forward your EUserv PIN email to your mailparser inbox 
  -> parsing PIN via mailparser 
  -> get PIN from mailparser
* Update kc2_security_password_get_token request
"""

import os
import re
import json
import time
import base64

from email.mime.application import MIMEApplication
from email.mime.multipart import MIMEMultipart
from email.mime.text import MIMEText
from smtplib import SMTP_SSL, SMTPDataError

import requests
from bs4 import BeautifulSoup

# 多个账户请使用空格隔开
USERNAME = "" # 用户名或邮箱
PASSWORD = ""  # 密码

# default value is TrueCaptcha demo credential,
# you can use your own credential via set environment variables:
# TRUECAPTCHA_USERID and TRUECAPTCHA_APIKEY
# demo: https://apitruecaptcha.org/demo
# demo2: https://apitruecaptcha.org/demo2
# demo apikey also has a limit of 100 times per day
# {
# 'error': '101.0 above free usage limit 100 per day and no balance',
# 'requestId': '7690c065-70e0-4757-839b-5fd8381e65c7'
# }
TRUECAPTCHA_USERID = os.environ.get("TRUECAPTCHA_USERID", "arun56")
TRUECAPTCHA_APIKEY = os.environ.get("TRUECAPTCHA_APIKEY", "wMjXmBIcHcdYqO2RrsVN")

# Extract key data from your emails, automatically. https://mailparser.io 
# 30 Emails/Month, 10 inboxes and unlimited downloads for free.
# 多个mailparser下载链接id请使用空格隔开, 顺序与 EUserv 账号/邮箱一一对应
MAILPARSER_DOWNLOAD_URL_ID = "这里填写我下方加粗链接中的仓库里获取到的mailparser的json文件链接ID"
# mailparser.io parsed data download base url
MAILPARSER_DOWNLOAD_BASE_URL = "https://files.mailparser.io/d/"

# Telegram Bot Push https://core.telegram.org/bots/api#authorizing-your-bot
TG_BOT_TOKEN = "TG的机器人TOKEN"  # 通过 @BotFather 申请获得，示例：1077xxx4424:AAFjv0FcqxxxxxxgEMGfi22B4yh15R5uw
TG_USER_ID = "你的TGID"  # 用户、群组或频道 ID，示例：129xxx206
TG_API_HOST = "https://api.telegram.org"  # 自建 API 反代地址，供网络环境无法访问时使用，网络正常则保持默认


# Email notification 
RECEIVER_EMAIL = os.environ.get("RECEIVER_EMAIL", "")
YD_EMAIL = os.environ.get("YD_EMAIL", "")
YD_APP_PWD = os.environ.get("YD_APP_PWD", "")  # yandex mail 使用第三方 APP 授权码

# Magic internet access
PROXIES = {"http": "http://127.0.0.1:10808", "https": "http://127.0.0.1:10808"}

# Maximum number of login retry
LOGIN_MAX_RETRY_COUNT = 5

# Waiting time of receiving PIN, units are seconds.
WAITING_TIME_OF_PIN = 15

# options: True or False
CHECK_CAPTCHA_SOLVER_USAGE = True

user_agent = (
    "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) "
    "Chrome/97.0.4692.71 Safari/537.36"
)

desp = ""  # 空值


def log(info: str):
    print(info)
    global desp
    desp = desp + info + "\n\n"


def login_retry(*args, **kwargs):
    def wrapper(func):
        def inner(username, password):
            ret, ret_session = func(username, password)
            max_retry = kwargs.get("max_retry")
            # default retry 3 times
            if not max_retry:
                max_retry = 3
            number = 0
            if ret == "-1":
                while number < max_retry:
                    number += 1
                    if number > 1:
                        log("[EUserv] Login tried the {}th time".format(number))
                    sess_id, session = func(username, password)
                    if sess_id != "-1":
                        return sess_id, session
                    else:
                        if number == max_retry:
                            return sess_id, session
            else:
                return ret, ret_session

        return inner

    return wrapper


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


def get_pin_from_mailparser(url_id: str) -> str:
    """
    response format:
    [
      {
        "id": "83b95f50f6202fb03950afbe00975eab",
        "received_at": "2021-11-06 02:30:07",  ==> up to mailparser account timezone setting, here is UTC 0000.
        "processed_at": "2021-11-06 02:30:07",
        "pin": "123456"
      }
    ]
    """
    response = requests.get(
        f"{MAILPARSER_DOWNLOAD_BASE_URL}{url_id}",
        # Mailparser parsed data download using Basic Authentication.
        # auth=("<your mailparser username>", "<your mailparser password>")
    )
    pin = response.json()[0]["pin"]
    return pin


@login_retry(max_retry=LOGIN_MAX_RETRY_COUNT)
def login(username: str, password: str) -> (str, requests.session):
    headers = {"user-agent": user_agent, "origin": "https://www.euserv.com"}
    url = "https://support.euserv.com/index.iphp"
    captcha_image_url = "https://support.euserv.com/securimage_show.php"
    session = requests.Session()

    sess = session.get(url, headers=headers)
    sess_id = re.findall("PHPSESSID=(\\w{10,100});", str(sess.headers))[0]
    # visit png
    logo_png_url = "https://support.euserv.com/pic/logo_small.png"
    session.get(logo_png_url, headers=headers)

    login_data = {
        "email": username,
        "password": password,
        "form_selected_language": "en",
        "Submit": "Login",
        "subaction": "login",
        "sess_id": sess_id,
    }
    f = session.post(url, headers=headers, data=login_data)
    f.raise_for_status()

    if (
        f.text.find("Hello") == -1
        and f.text.find("Confirm or change your customer data here") == -1
    ):
        if (
            f.text.find(
                "To finish the login process please solve the following captcha."
            )
            == -1
        ):
            return "-1", session
        else:
            log("[Captcha Solver] 进行验证码识别...")
            solved_result = captcha_solver(captcha_image_url, session)
            captcha_code = handle_captcha_solved_result(solved_result)
            log("[Captcha Solver] 识别的验证码是: {}".format(captcha_code))

            if CHECK_CAPTCHA_SOLVER_USAGE:
                usage = get_captcha_solver_usage()
                log(
                    "[Captcha Solver] current date {0} api usage count: {1}".format(
                        usage[0]["date"], usage[0]["count"]
                    )
                )

            f2 = session.post(
                url,
                headers=headers,
                data={
                    "subaction": "login",
                    "sess_id": sess_id,
                    "captcha_code": captcha_code,
                },
            )
            if (
                f2.text.find(
                    "To finish the login process please solve the following captcha."
                )
                == -1
            ):
                log("[Captcha Solver] 验证通过")
                return sess_id, session
            else:
                log("[Captcha Solver] 验证失败")
                return "-1", session

    else:
        return sess_id, session


def get_servers(sess_id: str, session: requests.session) -> {}:
    d = {}
    url = "https://support.euserv.com/index.iphp?sess_id=" + sess_id
    headers = {"user-agent": user_agent, "origin": "https://www.euserv.com"}
    f = session.get(url=url, headers=headers)
    f.raise_for_status()
    soup = BeautifulSoup(f.text, "html.parser")
    for tr in soup.select(
        "#kc2_order_customer_orders_tab_content_1 .kc2_order_table.kc2_content_table tr"
    ):
        server_id = tr.select(".td-z1-sp1-kc")
        if not len(server_id) == 1:
            continue
        flag = (
            True
            if tr.select(".td-z1-sp2-kc .kc2_order_action_container")[0]
            .get_text()
            .find("Contract extension possible from")
            == -1
            else False
        )
        d[server_id[0].get_text()] = flag
    return d


def renew(
    sess_id: str, session: requests.session, password: str, order_id: str, mailparser_dl_url_id: str
) -> bool:
    url = "https://support.euserv.com/index.iphp"
    headers = {
        "user-agent": user_agent,
        "Host": "support.euserv.com",
        "origin": "https://support.euserv.com",
        "Referer": "https://support.euserv.com/index.iphp",
    }
    data = {
        "Submit": "Extend contract",
        "sess_id": sess_id,
        "ord_no": order_id,
        "subaction": "choose_order",
        "choose_order_subaction": "show_contract_details",
    }
    session.post(url, headers=headers, data=data)

    # pop up 'Security Check' window, it will trigger 'send PIN' automatically.
    session.post(
        url,
        headers=headers,
        data={
            "sess_id": sess_id,
            "subaction": "show_kc2_security_password_dialog",
            "prefix": "kc2_customer_contract_details_extend_contract_",
            "type": "1",
        },
    )

    # # trigger 'Send new PIN to your Email-Address' (optional),
    # new_pin = session.post(url, headers=headers, data={
    #     "sess_id": sess_id,
    #     "subaction": "kc2_security_password_send_pin",
    #     "ident": f"kc2_customer_contract_details_extend_contract_{order_id}"
    # })
    # if not json.loads(new_pin.text)["rc"] == "100":
    #     print("new PIN Not Sended")
    #     return False

    # sleep WAITING_TIME_OF_PIN seconds waiting for mailparser email parsed PIN
    time.sleep(WAITING_TIME_OF_PIN)
    pin = get_pin_from_mailparser(mailparser_dl_url_id)
    log(f"[MailParser] PIN: {pin}")

    # using PIN instead of password to get token
    data = {
        "auth": pin,
        "sess_id": sess_id,
        "subaction": "kc2_security_password_get_token",
        "prefix": "kc2_customer_contract_details_extend_contract_",
        "type": 1,
        "ident": f"kc2_customer_contract_details_extend_contract_{order_id}",
    }
    f = session.post(url, headers=headers, data=data)
    f.raise_for_status()
    if not json.loads(f.text)["rs"] == "success":
        return False
    token = json.loads(f.text)["token"]["value"]
    data = {
        "sess_id": sess_id,
        "ord_id": order_id,
        "subaction": "kc2_customer_contract_details_extend_contract_term",
        "token": token,
    }
    session.post(url, headers=headers, data=data)
    time.sleep(5)
    return True


def check(sess_id: str, session: requests.session):
    print("Checking.......")
    d = get_servers(sess_id, session)
    flag = True
    for key, val in d.items():
        if val:
            flag = False
            log("[EUserv] ServerID: %s Renew Failed!" % key)

    if flag:
        log("[EUserv] ALL Work Done! Enjoy~")


# Telegram Bot Push https://core.telegram.org/bots/api#authorizing-your-bot
def telegram():
    data = (("chat_id", TG_USER_ID), ("text", "EUserv续费日志\n\n" + desp))
    response = requests.post(
        TG_API_HOST + "/bot" + TG_BOT_TOKEN + "/sendMessage", data=data
    )
    if response.status_code != 200:
        print("Telegram Bot 推送失败")
    else:
        print("Telegram Bot 推送成功")


def send_mail_by_yandex(
    to_email, from_email, subject, text, files, sender_email, sender_password
):
    msg = MIMEMultipart()
    msg["Subject"] = subject
    msg["From"] = from_email
    msg["To"] = to_email
    msg.attach(MIMEText(text, _charset="utf-8"))
    if files is not None:
        for file in files:
            file_name, file_content = file
            # print(file_name)
            part = MIMEApplication(file_content)
            part.add_header(
                "Content-Disposition", "attachment", filename=("gb18030", "", file_name)
            )
            msg.attach(part)
    s = SMTP_SSL("smtp.yandex.ru", 465)
    s.login(sender_email, sender_password)
    try:
        s.sendmail(msg["From"], msg["To"], msg.as_string())
    except SMTPDataError as e:
        raise e
    finally:
        s.close()


def email():
    msg = "EUserv 续费日志\n\n" + desp
    try:
        send_mail_by_yandex(
            RECEIVER_EMAIL, YD_EMAIL, "EUserv 续费日志", msg, None, YD_EMAIL, YD_APP_PWD
        )
        print("eMail 推送成功")
    except requests.exceptions.RequestException as e:
        print(str(e))
        print("eMail 推送失败")
    except SMTPDataError as e1:
        print(str(e1))
        print("eMail 推送失败")


def main_handler(event, context):
    if not USERNAME or not PASSWORD:
        log("[EUserv] 你没有添加任何账户")
        exit(1)
    user_list = [USERNAME]
    passwd_list = [PASSWORD]
    mailparser_dl_url_id_list = [MAILPARSER_DOWNLOAD_URL_ID]
    if len(user_list) != len(passwd_list):
        log("[EUserv] The number of usernames and passwords do not match!")
        exit(1)
    if len(mailparser_dl_url_id_list) != len(user_list):
        log("[Mailparser] The number of mailparser_dl_url_ids and usernames do not match!")
        exit(1)
    for i in range(len(user_list)):
        print("*" * 30)
        log("[EUserv] 正在续费第 %d 个账号" % (i + 1))
        sessid, s = login(user_list[i], passwd_list[i])
        if sessid == "-1":
            log("[EUserv] 第 %d 个账号登陆失败，请检查登录信息" % (i + 1))
            continue
        SERVERS = get_servers(sessid, s)
        log("[EUserv] 检测到第 {} 个账号有 {} 台 VPS，正在尝试续期".format(i + 1, len(SERVERS)))
        for k, v in SERVERS.items():
            if v:
                if not renew(sessid, s, passwd_list[i], k, mailparser_dl_url_id_list[i]):
                    log("[EUserv] ServerID: %s Renew Error!" % k)
                else:
                    log("[EUserv] ServerID: %s has been successfully renewed!" % k)
            else:
                log("[EUserv] ServerID: %s does not need to be renewed" % k)
        time.sleep(15)
        check(sessid, s)
        time.sleep(5)

    TG_BOT_TOKEN and TG_USER_ID and TG_API_HOST and telegram()
    RECEIVER_EMAIL and YD_EMAIL and YD_APP_PWD and email()

    print("*" * 30)


if __name__ == "__main__":
    main_handler(None, None)
```

事先需要下载的包

~~[主程序废弃](https://git.spiritlhl.workers.dev/o0oo0ooo0/EUserv_extend/archive/refs/heads/main.zip)~~

[BeautifulSoup.zip](https://git.spiritlhl.workers.dev/o0oo0ooo0/EUserv_extend/releases/download/0.1/BeautifulSoup.zip)

部署：

登陆腾讯云账号，没有就自己注册一个

腾讯云注册链接:  [点我跳转，页面右上角注册](https://curl.qcloud.com/aOhlmwKr)

注册完点下面这个链接跳转云函数界面

[https://console.cloud.tencent.com/scf/list?rid=15&ns=default](https://console.cloud.tencent.com/scf/list?rid=15&ns=default)

打开上面网址，应该会跳转登陆界面

![](https://gitee.com/spiritlhl/picture/raw/master/1C18BH7P~I%603_T@F6$NXY%7DW.png)

一定要选用硅谷或多伦多地区，使得脚本运行时访问Euserv的官网流畅而且不需要挂代理。

修改我提供的脚本的配置

![](https://cdn.jsdelivr.net/gh/spiritLHL/tuchuang@master/1.png)

填写邮箱和密码，还有推送用的信息。

![](https://cdn.jsdelivr.net/gh/spiritLHL/tuchuang@master/2.png)

**2021.11.19 还得填写mailparser的解析ID**

mailparser的解析ID的获取方式：

**[https://github.com/spiritLHL/eu_ex](https://github.com/spiritLHL/eu_ex)**

上面也是2021.11.19新脚本仓库的链接，但原始脚本不支持腾讯云函数的，我改成支持了的，如果自己有能力也可以自己改到别的平台上的云函数上去，比如说华为云函数，百度云函数。国内网不好可能打开很慢，原作者使用英文书写说明，建议开启浏览器自动翻译翻译观看。

修改完后复制脚本所有内容粘贴到下图的index.py里

![](https://cdn.jsdelivr.net/gh/spiritLHL/tuchuang@master/20220105215707.png)

之前改好后的main复制粘贴到这里↓

![](https://cdn.jsdelivr.net/gh/spiritLHL/tuchuang@master/20220105215739.png)

![](https://cdn.jsdelivr.net/gh/spiritLHL/tuchuang@master/20220105215808.png)

![](https://cdn.jsdelivr.net/gh/spiritLHL/tuchuang@master/20220105215837.png)

部署成功如下：

![](https://spiritlhl-tc.oss-cn-beijing.aliyuncs.com/QQ%E5%9B%BE%E7%89%8720210801191527.png)

修改完后在层中上传需要引用的```BeautifulSoup.zip```包，注意创建层的地址必须在跟之前创建的云函数相同的地区，我的都是硅谷。

![](https://spiritlhl-tc.oss-cn-beijing.aliyuncs.com/QQ%E5%9B%BE%E7%89%8720210801191914.png)

![](https://cdn.jsdelivr.net/gh/spiritLHL/tuchuang@master/20220105215909.png)

然后回到云函数中引用

![](https://spiritlhl-tc.oss-cn-beijing.aliyuncs.com/QQ%E5%9B%BE%E7%89%8720210801191630.png)

![](https://spiritlhl-tc.oss-cn-beijing.aliyuncs.com/QQ%E5%9B%BE%E7%89%8720210801191758.png)

引用成功如下图

![](https://spiritlhl-tc.oss-cn-beijing.aliyuncs.com/QQ%E5%9B%BE%E7%89%8720210801192631.png)

现在可以测试一下是否部署成功

![](https://spiritlhl-tc.oss-cn-beijing.aliyuncs.com/QQ%E5%9B%BE%E7%89%8720210801193230.png)

![](https://spiritlhl-tc.oss-cn-beijing.aliyuncs.com/QQ%E5%9B%BE%E7%89%8720210801193300.png)

pushplus推送新脚本不支持，只支持tg推送

![](https://spiritlhl-tc.oss-cn-beijing.aliyuncs.com/QQ%E5%9B%BE%E7%89%8720210801193403.png)

## SSH连接

审核完毕后就能通过SSH连接了，如果没有通过，你的邮箱会出现这个样子的邮件

![](https://spiritlhl-tc.oss-cn-beijing.aliyuncs.com/QQ%E5%9B%BE%E7%89%8720210801193759.jpg)

这就说明号废了，你现在登号可能登不上去了了，但可以试试。

审核成功会出现这样子的邮件

![](https://spiritlhl-tc.oss-cn-beijing.aliyuncs.com/QQ%E5%9B%BE%E7%89%8720210801194139.png)

审核成功后查看IPV6地址

控制面板地址：[https://support.euserv.com/index.iphp](https://support.euserv.com/index.iphp)

点select选择系统，个人推荐centos7，其他系统比较难装东西

![](https://cdn.jsdelivr.net/gh/spiritLHL/tuchuang@master/20220105215941.png)

安装系统大概需要10~20分钟，安装成功后再点select可查看下面信息

在左侧的serverdata中找IPV6信息

![](https://spiritlhl-tc.oss-cn-beijing.aliyuncs.com/QQ%E5%9B%BE%E7%89%8720210801194941.png)

![](https://spiritlhl-tc.oss-cn-beijing.aliyuncs.com/pic%20-%201.jpg)

上图中一个箭头是ipv6转的一个免费域名

第二个箭头是你的管理员密码，如果你和我一样都是选centos7系统，那么系统用户名一样是root

第三个箭头指的是实际的IPV6地址+/128，去掉```/128```前面那串就是实际的IPV6地址。

到这里已经可以使用SSH客户端连接服务器进行操作了，但本地必须要有IPV6环境才能连，记得前期准备要做好。

SSH客户端下载：[点击下载远程SSH工具,下载解压即可使用](https://spiritlhl.lanzoui.com/i2PVDr187ng)

提取码：   ```QLHL```

打开软件，直接点击左上角会话，选择SSH类型，远程主机框填写自带转的免费域名某某莫de或者填入IPV6地址，然后点击```好的```创建会话窗口，然后等待连接成功后会出现

![](https://spiritlhl-tc.oss-cn-beijing.aliyuncs.com/QQ%E5%9B%BE%E7%89%8720210801200210.png)

填入管理员账号名字(我的是root，我是centos7系统，别的系统不一样的)

然后按回车，它会叫你输入密码

显示如下

```
root@XXXXXXXXX.DDDDDDDDD.XXXXXX.de's password:这里填密码，输入的时候看不见输入了什么的，输入完毕后按回车就登陆了
```

登陆失败按r键重试，多次还是失败估计你前面某一步错了。

登陆成功后，什么都别做，先输入下面这串代码按回车执行，不然你装什么都装不上，开启IPV4的出口，使用荷兰出口下载东西

```bash
echo -e "nameserver 2001:67c:2b0::4\nnameserver 2001:67c:2b0::6" > /etc/resolv.conf
```

然后后面就能和正常的云服务器一样装东西了。

ps:只有IPV6地址，装什么面板前缀都是IPV6地址，只能在IPV6环境下访问。

pps：没有IPV4地址，可以使用nginx反代或者cloudflare或者别的方式，使得面板在只有IPV4环境中展示并使用。

ppps：这机子可以搭那啥，你懂的嘿嘿嘿

## 后言

相关资源收集站点：[Github原始仓库链接](https://github.com/spiritLHL/Hang-up-items#%E5%89%8D%E8%A8%80)

上面仓库会及时分享资源信息，有想收取续期脚本更新提示的，建议去仓库右上角watch一下notification一下，邮件获取更新信息。

上述都是个人经验，如果还是不会可以--->[点击联系我](http://wpa.qq.com/msgrd?v=3&uin=2461006717&site=qq&menu=yes)，有时间或许会帮助一下

欢迎请站长喝一杯！

![](https://cdn.jsdelivr.net/gh/spiritLHL/tuchuang@master/20220105220033.png)



