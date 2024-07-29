# Appium自动化操作01(环境安装与初始结构)

# 前言

最好有```Selenium```的Web自动化的实际经验

本篇用到的相关软件链接：

链接: https://pan.baidu.com/s/126x-AgLKvM7qSJqdOzAAHA 

提取码: h9b2 

---
# Appium 基础知识

**Appium 用途和特点**

Appium 是一个移动 App （手机应用）自动化工具。

手机APP自动化有什么用？

    自动化完成一些重复性的任务

比如

    微信客服机器人

    爬虫

    自动化测试

爬虫就是通过手机自动化爬取信息。

为什么不通过网页、HTTP 爬取呢？

    有的系统没有网页，也不方便通过HTTP爬取

自动化测试

    很多软件开发里面有这样的需求

Appium 自动化方案的特点：

    开源免费

    支持多个平台

    iOS （苹果）、安卓 App 的自动化都支持。

    支持多种类型的自动化

    支持 苹果、安卓 应用 原生界面 的自动化

    支持 应用 内嵌 WebView 的自动化

    支持 手机浏览器 中的 web网站自动化

    支持 flutter 应用的自动化

    支持多种编程语言

像 Selenium 一样，Appium可以用多种编程语言调用它开发自动化程序。

---

# 自动化原理

Appium自动化的原理图:

![](https://pic./2020/05/23/c90f8b49b6e01.jpg)

它和Selenium原理图很像。因为Appium自动化架构借鉴了Selenium。

它包含了3个主体部分：自动化程序、Appium Server、移动设备

---

**自动化程序**

自动化程序是由我们来开发的，实现具体的手机自动化功能。

要发出具体的指令控制手机，也需要使用客户端库。

和Selenium一样，Appium 组织也提供了多种编程语言的客户端库，包括 java，python，js， ruby等，方便不同编程语言的开发者使用。

首先需要安装好客户端库，调用这些库，就可以发出自动化指令给手机。

---

**Appium Server**

Appium Server 是 Appium 组织开发的程序，它负责管理手机自动化环境，并且转发自动化程序的控制指令给手机，并且转发手机给自动化程序的响应消息。

---

**手机设备**

这里说的手机设备，其实不仅仅是手机，包括所有苹果、安卓的移动设备，比如：手机、平板、智能手表等。

为了直观方便的讲解，这里我们简称：手机

当然手机上也包含了我们要自动化控制的手机应用APP。

手机设备为什么能接收并且处理自动化指令呢？

因为，Appium Server 会在手机上安装一个自动化代理程序，代理程序会等待自动化指令，并且执行自动化指令

PS:这里使用手机端的自动化代理后你的键盘会无法弹出，只能接受电脑控制输入，手机键盘输入与电脑输入不能并存

比如：要模拟用户点击界面按钮，Appium自动化系统的流程是这样的：

```自动化程序```调用客户端库相应的函数，发送```点击元素```的指令（封装在HTTP消息里）给```Appium Server```

```Appium Server```再转发这个指令给```手机上的自动化代理```

```手机上的自动化代理```接收到```指令```后，调用手机平台的自动化库，执行点击操作，返回点击成功的结果给 ```Appium Server```

```Appium Server```转发给```自动化程序```

```自动化程序```了解到本操作成功后，继续后面的自动化流程

其中，自动化代理控制，使用的什么库来实现自动化的呢？

如果测试的是苹果手机， 用的是苹果的 XCUITest 框架 （IOS9.3版本以后）

如果测试的是安卓手机，用的是安卓的 UIAutomator 框架 (Android4.2以后)

这些自动化框架提供了在手机设备上运行的库，可以让程序调用这些库，像人一样自动化操控设备和APP，比如：点击、滑动，模拟各种按键消息等。

---

# 自动化环境搭建

这里以安卓APP的自动化为例。

环境搭建需要下载安装不少软件，而且还有不少是国外网站下载的。

这些软件```安装包```都放在```前言```的百度网盘链接中了，请自行下载。

---

1.安装client第三方库

首先需要在开发环境下下载appium-python-client库，也可以用pip安装，如下(pip默认下载最新库，要指定特殊版本，要在后面加上==特殊版本名)

PS:这里的appium-python-client库有特殊版本要求，需要安装的是```0.52```版本，不是```1.0```版本以上，否则会报错，建议使用Pycharm环境

```
pip install appium-python-client==0.52
```

---

2.安装Appium Server

Appium Server 是用 nodejs 运行的，基于js开发出来的。

Appium组织为了方便软件安装使用，制作了一个可执行程序 Appium Desktop，把nodejs 运行环境、Appium Server 和一些工具打包在里面了，只需要简单的下载安装就可以了。

可以从```前言```给出的百度网盘里下载安装： 

Appium-windows-1.15.1.exe

Appium Desktop官方下载链接：

[https://github.com/appium/appium-desktop/releases/tag/v1.15.1](https://github.com/appium/appium-desktop/releases/tag/v1.15.1)

---

3.安装JDK

安卓APP的自动化，必须要安装安卓SDK(后面会提到)，而安卓SDK需要JDK环境。

可以从```前言```给出的百度网盘里下载安装： 

jdk-8u211-windows-x64.exe

安装好之后，还需要添加一个环境变量```JAVA_HOME```，指定值为```jdk安装目录```，比如

JAVA_HOME   D:\Javajdk

实际情况如下：

![](https://pic./2020/05/23/b00d24eaac905.png)

---

4.安装Android SDK

对于安卓APP的自动化，Appium Server是需要Android SDK的。

因为要用到里面的一些工具，比如要执行命令设置手机、传送文件、安装应用、查看手机界面等。

可以从```前言```给出的百度网盘里下载Android SDK文件包： ```androidsdk.zip```，并且解压，即可。

解压完成后，需要配置一下添加一个环境变量 ```ANDROID_HOME```，设置值为sdk包解压目录，比如 ```D:\androidsdk```添加步骤参照第三步的图


另外，还推荐大家配置环境变量```PATH```，加入```adb```所在目录， ```D:\安卓dk\androidsdk\androidsdk\platform-tools\```

实际情况如下：

![](https://pic./2020/05/23/e0f7ebf71e4d7.png)

注意：是```添加```该目录到环境变量PATH中，不是替换!否则会导致系统命令都找不到的严重后果，```双击2处使用新建添加！```。

---

5.连接手机

上述的软件环境都准备好以后，要自动化手机APP，需要：

在你运行程序的电脑上用```USB线```连接上你的安卓手机

进入手机设置 -> 关于手机，不断点击```版本号```菜单（大概7次以上）进入开发者模式

退出到上级菜单，在设置首页里点系统与更新，在系统与更新的开发者模式中，启动USB调试

如果手机连接USB线后，手机界面弹出是否选择允许USB调试，请选择是。

注意：

有的手机系统，可能需要一些额外的选项需要设置好。

比如，有的手机，开发者选项里需要打开允许通过USB安装应用等。

总之，给USB开发调试尽可能方便的控制手机。

连接好以后，打开命令行窗口，执行```adb devices -l```命令来列出连接在电脑上的安卓设备。

如果输出类似如下的内容：

```
List of devices attached
MGFNW19731015276       device product:HLK-AL00 model:HLK_AL00 device:HWHLK-H transport_id:1
```

表示电脑上可以查看到连接的设备，就可以运行自动化程序了。

---

# 第一个例子

运行代码前，首先得运行```Appium Desktop```

```python
from appium import webdriver
from appium.webdriver.extensions.android.nativekey import AndroidKey

desired_caps = {
  'platformName': 'Android', # 被测手机是安卓
  'platformVersion': '10', # 手机安卓版本，这个需要在设置里的关于手机里查找，注意是Android版本，不是MIUI等系统版本
  'deviceName': 'HLK-AL00', # 设备名，安卓手机可以随意填写
  'appPackage': 'tv.danmaku.bili', # 启动APP Package名称
  'appActivity': '.ui.splash.SplashActivity', # 启动Activity名称
  'unicodeKeyboard': True, # 使用自带输入法，输入中文时填True
  'resetKeyboard': True, # 执行完程序恢复原来输入法
  'noReset': True,       # 不要重置App
  'newCommandTimeout': 6000, #服务自动断开时间
  'automationName' : 'UiAutomator2'#服务协议
}

# 连接Appium Server，初始化自动化环境
driver = webdriver.Remote('http://localhost:4723/wd/hub', desired_caps)

# 设置等待时间
driver.implicitly_wait(5)

# 如果有`青少年保护`界面，点击`我知道了`
iknow = driver.find_elements_by_id("text3")
if iknow:
    iknow.click()

# 根据id定位搜索位置框，点击
driver.find_element_by_id("expand_search").click()

# 根据id定位搜索输入框，点击
sbox = driver.find_element_by_id('search_src_text')
sbox.send_keys('祈LHL')

# 输入回车键，确定搜索
driver.press_keycode(AndroidKey.ENTER)

# 选择（定位）所有视频标题
eles = driver.find_elements_by_id("title")

for ele in eles:
    # 打印标题
    print(ele.text)

input('**** Press to quit..')
driver.quit()
```

---

# 查找应用```Package```和```Activity```

1.应用已安装

如果应用已经安装在手机上了，可以直接打开手机上该应用，进入到你要操作的界面

然后在cmd执行

```
adb shell dumpsys activity recents | find "intent={"
```
会显示如下，最近的几个```activity```信息，

```
    intent={act=android.intent.action.MAIN cat=[android.intent.category.LAUNCHER] flg=0x10200000 cmp=tv.danmaku.bili/.ui.splash.SplashActivity}
    intent={act=android.intent.action.MAIN cat=[android.intent.category.HOME] flg=0x10010000 pkg=com.huawei.android.launcher cmp=com.huawei.android.launcher/.unihome.UniHomeLauncher}
    intent={act=android.intent.action.MAIN cat=[android.intent.category.LAUNCHER] flg=0x10200000 cmp=com.android.settings/.HWSettings}
    intent={act=android.intent.action.MAIN flg=0x10840000 hwFlg=0x4100 cmp=com.android.incallui/.InCallActivity}

```

其中第一行就是当前的应用，我们特别关注最后

cmp=tv.danmaku.bili/.ui.splash.SplashActivity
应用的package名称就是 tv.danmaku.bili

应用的启动Activity就是 .ui.splash.SplashActivity

2.有apk但是没有安装到手机端

如果你已经获取到了```apk```，在cmd命令行窗口执行

```
D:\安卓dk\androidsdk\build-tools\29.0.3\aapt.exe dump badging D:\apk\bili.apk | find "package: name="
```
输出信息中，就有应用的package名称 

PS:第一串链接里的```D:\安卓dk```是我解压```androidsdk.zip```压缩包的地址；第二串链接里的```D:\apk\bili.apk```是指的是我D盘的apk文件夹下有一个```bili.apk```的apk文件

输出结果：

```
package: name='tv.danmaku.bili' versionCode='5531000' versionName='5.53.1' platformBuildVersionName='5.53.1' compileSdkVersion='28' compileSdkVersionCodename='9'
```

在命令行窗口执行

```
D:\安卓dk\androidsdk\build-tools\29.0.3\aapt.exe dump badging d:\tools\apk\bili.apk | find "launchable-activity"
```

输出信息中，就有应用的启动Activity

输出结果：
```
launchable-activity: name='tv.danmaku.bili.ui.splash.SplashActivity'  label='' icon=''
```





































