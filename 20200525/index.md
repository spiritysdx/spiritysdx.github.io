# Appium自动化操作03(界面操作和adb命令)

# 前言

最好有```Selenium```的Web自动化的实际经验

运行基础：client库(0.52版本)，Appium Server，安卓SDK(含JDK环境)，USB调试模式下的手机(开发者模式)

所用apk包(wv.apk)链接：

[https://spiritlhl.lanzous.com/icxhm7g](https://spiritlhl.lanzous.com/icxhm7g)

---

# 界面操作和adb命令

### click点击

最常见的操作之一，使用```WebElement```对象的```click```方法。

---
### tap点按

```WebElement```对象的```tap```方法和```click```类似，都是点击界面。

但是最大的区别是，```tap```是针对```坐标```而不是针对找到的```元素```。

为了保证自动化代码在所有分辨率的手机上都能正常执行，我们通常应该使用click方法。

但有的时候，我们难以用通常的方法定位元素，可以用这个```tap```方法，根据坐标来点击

用```inspect```查看该元素的属性中，有一个```bounds```属性吗？

它就是表示元素的```左上角```，```右下角```坐标的坐标。

我们还可以使用```UIAutomatorviewer```直接十字光标移动，看右边的属性提示。

```tap```方法可以像这样进行调用

```python
driver.tap([(1100,1080),],700)
```

它有两个参数：

第一个参数是个```列表```，表示点击的坐标。

注意最多可以有```5个元素```，代表5根手指点击5个坐标。所以是```list```类型。

如果我们只要模拟一根手指点击屏幕，list中只要一个元素就可以了

第二个参数表示```tap```点按屏幕后停留的时间（毫秒）。

如果点按时间过长，就变成了长按操作了。

---

### 输入

最常见的操作之一，使用```WebElement```对象的```send_keys```方法。

---

### 获取界面文本信息

可以通过```WebElement```对象的```.text```属性获取该对象的文本信息。

---

### 滑动

做移动app测试的时候，经常需要滑动界面。

这需要使用```WebDriver```对象的```swipe```方法。

```python
driver.swipe(start_x=x, start_y=y1, end_x=x, end_y=y2, duration=800)
```

前面4个参数是```滑动起点```和```终点```的x、y坐标。

第5个参数```duration```是滑动从起点到终点坐标所耗费的时间（毫秒）。

注意这个时间非常重要，在屏幕上滑动同样的距离，如果时间设置的很短，就是快速的滑动。

例如：一个翻动新闻的界面，快速的滑动，就会是扫动的动作，会导致内容随惯性滚动很多。

---

### 按键

调用```press_keycode```方法，就能模拟```按键动作```，包括安卓手机的```实体```按键和```键盘```按钮。

具体代码如下：

```python
from appium.webdriver.extensions.android.nativekey import AndroidKey


# 输入回车键，确定搜索
driver.press_keycode(AndroidKey.ENTER)
```

按键的定义，可以参考这篇文档[https://github.com/appium/python-client/blob/master/appium/webdriver/extensions/android/nativekey.py](https://github.com/appium/python-client/blob/master/appium/webdriver/extensions/android/nativekey.py)

---

### 长按、双击、移动

Appium的```TouchAction```类提供了更多的手机操作方法，如：长按、双击、移动

参考源代码中的注释[https://github.com/appium/python-client/blob/master/appium/webdriver/common/touch_action.py](https://github.com/appium/python-client/blob/master/appium/webdriver/common/touch_action.py)

下面有一个长按的例子

```python
from appium.webdriver.common.touch_action import TouchAction

actions = TouchAction(driver)
actions.long_press(element)
actions.perform()
```

---

### 打开通知栏

安卓手机，查看通知栏的动作可以是从屏幕顶端下滑来查看通知。

也可以使用如下代码，直接打开通知栏

```python
driver.open_notifications()
```

通知栏里面的元素，自动化的方法和前面介绍的App界面元素自动化是一样的。

---

### 收起通知栏

收起通知栏，可以使用前面介绍的模拟按键，发出```返回按键```的方法。

---

### adb 命令

```android sdk```里面有一个命令行工具```adb```。

```adb```全称```Android Debug Bridge```，这个```adb```使用非常广泛。

它可以与```Android```手机设备进行通信，它可进行各种设备操作。

如：安装应用和调试应用，传输文件，甚至登录到手机设备上shell的进行访问，就像远程登录一样

这个```adb```在```sdk```的```platform-tools```目录下面，请大家确保路径在```Path环境变量```中。

Appium对anroid的自动化非常依赖这个adb工具。执行自动化过程中，有很多内部操作，比如获取设备信息，传送文件到手机，安装apk，启动某些程序等，都是通常这个```adb```实现的。

```adb```命令既然是个命令，就可以使用```Python```的```os.system()```或者```subprocess```来自动化调用它，完成我们的各种自动化需求。

而自动化过程中，可能需要截屏手机，并且下载到指定目录中，可以在Python程序中这样写

```python
import os


os.system('adb shell screencap /sdcard/screen3.png && adb pull /sdcard/screen3.png')
```

特别的，还可以通过```adb```使用```am(activity manager)```和```pm(package manager)```两个工具，可以启动```Activity```、强行停止进程、广播```intent```、修改设备屏幕属性、列出应用、卸载应用等。

adb命令的官方文档：[https://developer.android.google.cn/studio/command-line/adb.html#devicestatus](https://developer.android.google.cn/studio/command-line/adb.html#devicestatus)

一些常见的adb命令：

查看连接的设备

    adb devices -l

查看文件目录
    
    adb shell ls /sdcard

上传文件

    adb push wv.apk /sdcard/wv.apk

下载文件

    adb pull /sdcard/new.txt

截屏

    adb shell screencap /sdcard/screen.png

截屏后的文件存在手机上，可以使用```adb pull```下载下来

---

### shell

登录到手机设备上shell的进行访问，就像远程登录一样，可用来在连接的设备上运行各种命令。

输入```adb shell```然后执行各种安卓支持的```Linux命令```，比如```ps```、```netstat```、```netstat -an|grep 4724```、```pwd```、```ls```、```cd```、```rm```等。

执行```quit```退出```shell```
