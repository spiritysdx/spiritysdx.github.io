# Selenium的web自动化操作02(基本语法)

## **前言**

需要下载Chrome或Firefox的driver，Chrome内核81.440与Firefox内核74.0下载链接如下：

[Firefox](https://spiritlhl.lanzous.com/icg1k3c)

[Chrome](https://spiritlhl.lanzous.com/icg1k0j)

其他版本请在搜索引擎查找，本篇使用该版本，注意，driver下载后需要配置对应内核的游览器，电脑本身需要有该内核的游览器。

---

## 正文

---

### radio框

radio框选择选项，直接用WebElement的click方法，模拟用户点击就可以了。

比如, 我们要在下面的html中： 

先打印当前选中的  
```python
# 获取当前选中的元素
element = wd.find_element_by_css_selector(
  '#s_radio input[checked=checked]')
print('当前选中的是: ' + element.get_attribute('value'))

# 点选
wd.find_element_by_css_selector(
  '#s_radio input[value="小雷老师"]').click()
```

---

### checkbox框

对checkbox进行选择，也是直接用```WebElement```的```click```方法，模拟用户点击选择。

需要注意的是，要选中checkbox的一个选项，必须先获取当前该复选框的状态。   
如果该选项已经勾选了，就不能再点击。   
否则反而会取消选择。   

先把已经选中的选项全部点击一下，确保都是未选状态再点击要选的   

```python
# 先把 已经选中的选项全部点击一下
elements = wd.find_elements_by_css_selector(
  '#s_checkbox input[checked="checked"]')

for element in elements:
    element.click()

wd.find_element_by_css_selector(
  "#s_checkbox input[value='要选']").click()
```

---  

### select框

radio框及checkbox框都是input元素，只是里面的type不同而已。

select框 则是一个新的select标签，大家可以对照浏览器网页内容查看一下

对于Select 选择框， Selenium 专门提供了一个 Select类 进行操作。

Select类 提供了如下的方法

```
select_by_value
```
根据选项的 value属性值 ，选择元素。

比如，下面的HTML

```
<option value="xxl">cool</option>
```

就可以根据 xxl 这个值选择该选项

```python
s.select_by_value('xxl')
```


```
select_by_index
```
根据选项的 次序 （从0开始），选择元素

```
select_by_visible_text
```
根据选项的 可见文本 ，选择元素。

比如，下面的HTML：
```
<option value="xxl">cool</option>
```

就可以根据 cool 这个内容，选择该选项

```python
s.select_by_visible_text('cool')
```

```
deselect_by_value
```
根据选项的value属性值， 去除选中元素

```
deselect_by_index
```
根据选项的次序，去除选中元素

```
deselect_by_visible_text
```
根据选项的可见文本，去除选中元素

```
deselect_all
```
去除选中所有元素

---

###  Select单选框
 
对于 select单选框，操作比较简单：  
不管原来选的是什么，直接用Select方法选择即可。  
例如，选择要选的，示例代码如下  
```python
# 导入Select类
from selenium.webdriver.support.ui import Select


# 创建Select对象（实例化）
select = Select(wd.find_element_by_id("要选id"))

# 通过 Select 对象选中 要选选项
select.select_by_visible_text("要选选项")
```

---

### Select多选框

对于select多选框，要选中某几个选项，要注意去掉原来已经选中的选项。

例如，我们选择示例多选框中的 x1 和 x2

可以用select类的```deselect_all```方法，清除所有 已经选中 的选项。

然后再通过```select_by_visible_text```方法选择 x1 和 x2。

示例代码如下：

```python
# 导入Select类
from selenium.webdriver.support.ui import Select

# 创建Select对象
select = Select(wd.find_element_by_id("x"))

# 清除所有 已经选中 的选项
select.deselect_all()

# 选择 x1 和 x2
select.select_by_visible_text("x1")
select.select_by_visible_text("x2")
```

---

### ActionChains类

鼠标右键点击、双击、移动鼠标到某个元素、鼠标拖拽等。

这些操作，可以通过Selenium提供的```ActionChains```类来实现。

```ActionChains```类里面提供了 一些特殊的动作的模拟，我们可以通过```ActionChains```类的代码查看到，如下所示

我们以移动鼠标到某个元素为例。

百度首页的右上角，有个选项

网址：https://www.baidu.com/

如果我们把鼠标放在上边，就会弹出下面的各种图标。

使用```ActionChains```来模拟鼠标移动 操作的代码如下：

(百度网站首页可能有变化，选择的类可能需要更改)

```python
from selenium import webdriver


driver = webdriver.Chrome(r'f:\chromedriver.exe')
driver.implicitly_wait(5)

driver.get('https://www.baidu.com/')

from selenium.webdriver.common.action_chains import ActionChains

ac = ActionChains(driver)

# 鼠标移动到 元素上
ac.move_to_element(
    driver.find_element_by_css_selector('[name="tj_briicon"]')
).perform()
```

直接执行javascript

我们可以直接让浏览器运行一段javascript代码，并且得到返回值，如下

```python
# 直接执行 javascript，里面可以直接用return返回我们需要的数据
nextPageButtonDisabled = driver.execute_script(
    '''
    ele = document.querySelector('.soupager > button:last-of-type');
    return ele.getAttribute('disabled')
    ''')

# 返回的数据转化为Python中的数据对象进行后续处理
if nextPageButtonDisabled == 'disabled': # 是最后一页
    return True
else: # 不是最后一页
    return False
```

---

### 冻结界面

有些网站上面的元素，我们鼠标放在上面，会动态弹出一些内容。

比如，百度首页的右上角，有个选项：[https://www.baidu.com/](https://www.baidu.com/)

如果我们把鼠标放在上边，就会弹出很多图标。

如果我们要用```selenium```自动化```点击```其中图标，就需要```F12```查看这个元素的特征。

但是当我们的鼠标从图标移开，这个栏目就整个消失了，就没法查看其对应的HTML。

怎么办？

在开发者工具栏console里面执行如下js代码

```js
setTimeout(function(){debugger}, 5000)
```

这句JavaScript代码什么意思呢？

表示在 5000毫秒后，执行```debugger```命令

执行该命令会 浏览器会进入debug状态。debug状态有个特性，界面被冻住，不管我们怎么点击界面都不会触发事件。

所以，我们可以在输入上面代码并回车 执行后，立即鼠标放在界面右上角图标处。

这时候，就会弹出下面的各种图标。

然后，我们仔细等待5秒到了以后，界面就会因为执行了```debugger```命令而被冻住。

然后，我们就可以点击开发者工具栏的查看箭头，再去点击其中图标，查看其属性了。

---

### 弹出对话框

有的时候，我们经常会在操作界面的时候，出现一些弹出的对话框。

弹出的对话框有三种类型，分别是 Alert（警告信息）、confirm（确认信息）和prompt（提示输入）

---

### Alert

Alert 弹出框，目的就是显示通知信息，只需用户看完信息后，点击 OK（确定） 就可以了。

那么，自动化的时候，代码怎么模拟用户点击 OK 按钮呢？

selenium提供如下方法进行操作

```python
driver.switch_to.alert.accept()
```

注意：如果我们不去点击它，页面的其它元素是不能操作的。

如果程序要获取弹出对话框中的信息内容，可以通过如下代码

```python
driver.switch_to.alert.text
```

示例代码如下

```python
from selenium import webdriver


driver = webdriver.Chrome()
driver.implicitly_wait(5)
driver.get('http://cdn1.python3.vip/files/selenium/test4.html')

# --- alert ---
driver.find_element_by_id('b1').click()

# 打印 弹出框 提示信息
print(driver.switch_to.alert.text)

# 点击 OK 按钮
driver.switch_to.alert.accept()
```

---

## Confirm

Confirm弹出框，主要是让用户确认是否要进行某个操作。

比如：当管理员在网站上选择删除某个账号时，就可能会弹出Confirm弹出框，要求确认是否确定要删除。

Confirm弹出框 有两个选择供用户选择，分别是OK和Cancel，分别代表确定和取消操作。

那么，自动化的时候，代码怎么模拟用户点击OK或者Cancel按钮呢？

selenium提供如下方法进行操作

如果我们想点击OK按钮，还是用刚才的accept方法，如下

```python
driver.switch_to.alert.accept()
```

如果我们想点击 Cancel 按钮， 可以用 dismiss方法，如下

```python
driver.switch_to.alert.dismiss()
```

示例代码如下
```python
from selenium import webdriver


driver = webdriver.Chrome()
driver.implicitly_wait(5)
driver.get('http://cdn1.python3.vip/files/selenium/test4.html')

# --- confirm ---
driver.find_element_by_id('b2').click()

# 打印 弹出框 提示信息
print(driver.switch_to.alert.text)

# 点击 OK 按钮
driver.switch_to.alert.accept()

driver.find_element_by_id('b2').click()

# 点击 取消 按钮
driver.switch_to.alert.dismiss()
```

---

### Prompt

出现 Prompt 弹出框 是需要用户输入一些信息，提交上去。

比如：当管理员在网站上选择给某个账号延期时，就可能会弹出 Prompt 弹出框， 要求输入延期多长时间。

可以调用如下方法

```python
driver.switch_to.alert.send_keys()
```

示例代码如下
```python
from selenium import webdriver


driver = webdriver.Chrome()
driver.implicitly_wait(5)
driver.get('http://cdn1.python3.vip/files/selenium/test4.html')


# --- prompt ---
driver.find_element_by_id('b3').click()

# 获取 alert 对象
alert = driver.switch_to.alert

# 打印 弹出框 提示信息
print(alert.text)

# 输入信息，并且点击 OK 按钮 提交
alert.send_keys('web自动化 - selenium')
alert.accept()

# 点击 Cancel 按钮 取消
driver.find_element_by_id('b3').click()
alert = driver.switch_to.alert
alert.dismiss()
```
注意：
有些弹窗并非浏览器的alert 窗口，而是html元素，这种对话框，只需要通过之前介绍的选择器选中并进行相应的操作就可以了。

---

## 其他技巧

下面是一些其他的 Selenium 自动化技巧

---

### 窗口大小

有时间我们需要获取窗口的属性和相应的信息，并对窗口进行控制

获取窗口大小
```python
driver.get_window_size()
```
改变窗口大小
```python
driver.set_window_size(x, y)
```

---

### 获取当前窗口标题

浏览网页的时候，我们的窗口标题是不断变化的，可以使用WebDriver的title属性来获取当前窗口的标题栏字符串。

```python
driver.title
```
获取当前窗口URL地址
```python
driver.current_url
```
例如，访问网易，并获取当前窗口的标题和URL

```python
from selenium import  webdriver


driver = webdriver.Chrome()
driver.implicitly_wait(5)

# 打开网站
driver.get('https://www.163.com')

# 获取网站标题栏文本
print(driver.title)

# 获取网站地址栏文本
print(driver.current_url)

```

---

### 截屏
有的时候，我们需要把浏览器屏幕内容保存为图片文件。

比如，做自动化测试时，一个测试用例检查点发现错误，我们可以截屏为文件，以便测试结束时进行人工核查。

可以使用```WebDriver```的```get_screenshot_as_file```方法来截屏并保存为图片。

```python
from selenium import  webdriver


driver = webdriver.Chrome()
driver.implicitly_wait(5)

# 打开网站
driver.get('https://www.baidu.com/')

# 截屏保存为图片文件
driver.get_screenshot_as_file('1.png')
```

---

### 手机模式

我们可以通过 desired_capabilities 参数，指定以手机模式打开chrome浏览器

参考代码，如下
```python
from selenium import webdriver

mobile_emulation = { "deviceName": "Nexus 5" }

chrome_options = webdriver.ChromeOptions()

chrome_options.add_experimental_option("mobileEmulation", mobile_emulation)

driver = webdriver.Chrome( desired_capabilities = chrome_options.to_capabilities())

driver.get('http://www.baidu.com')

input()
driver.quit()
```

---

### 上传文件

有时候，网站操作需要上传文件。

比如，著名的在线图片压缩网站： [https://tinypng.com/](https://tinypng.com/)

以及由网友Cristy分享的压缩网站：[https://www.websiteplanet.com/webtools/imagecompressor/](https://www.websiteplanet.com/webtools/imagecompressor/)

后者运行同时压缩jpeg和png文件，并且每幅图片的大小可以达到50MB

通常，网站页面上传文件的功能，是通过 type 属性 为 file 的 HTML input 元素实现的。

如下所示：

```
<input type="file" multiple="multiple">
```
使用selenium自动化上传文件，我们只需要定位到该input元素，然后通过 send_keys 方法传入要上传的文件路径即可。

如下所示：
```python
# 先定位到上传文件的 input 元素
ele = wd.find_element_by_css_selector('input[type=file]')

# 再调用 WebElement 对象的 send_keys 方法
ele.send_keys(r'h:\g02.png')
```

如果需要上传多个文件，可以多次调用send_keys，如下

```python
ele = wd.find_element_by_css_selector('input[type=file]')
ele.send_keys(r'h:\g01.png')
ele.send_keys(r'h:\g02.png')
```

---

### 自动化Edge浏览器

自动化基于Chromium内核的微软最新Edge浏览器，首先需要查看Edge的版本。

点击菜单 帮助和反馈 > 关于Microsoft Edge ，在弹出界面中，查看到版本，比如

Microsoft Edge 是最新版本。
版本 81.0.416.72 (官方内部版本) (64 位)

然后[点击这里](https://developer.microsoft.com/en-us/microsoft-edge/tools/webdriver/#downloads)，打开Edge浏览器驱动下载网页 ，并选择下载对应版本的驱动。

在自动化代码中，指定使用```Edge Webdriver```类，并且指定```Edge```驱动路径，如下所示

```python
from selenium import webdriver


driver = webdriver.Edge(r'd:\tools\webdrivers\msedgedriver.exe')
driver.get('http://www.51job.com')
```

---

### 自动化Electron程序

Electron程序都是基于基于Chromium技术开发的，所以基本也可以用Chromedriver驱动自动化。

要自动化，首先需要得到内置Chromium的版本号。

向开发人员查询打开 Dev Tools 窗口的快捷键（通常是ctrl + Shift + I），打开Dev Tools 窗口后， 在 Console tab中输入 如下语句，查看版本

> navigator.appVersion.match(/.*Chrome\/([0-9\.]+)/)[1]
  "79.0.3945.130"

然后去 chromedriver下载网址 ，下载对应版本的驱动。

在自动化程序中需要指定打开的可执行程序为Electron程序，而不是 Chrome浏览器。

如下所示

```python
from selenium import webdriver
from selenium.webdriver.chrome.options import Options


ops = Options()

# 指定Electron程序路径
ops.binary_location = r"C:\electronAPP.exe"

driver = webdriver.Chrome(r"e:\chromedriver.exe",
                            options = ops)
```

---

## 黑科技

---

### 使用代理

selenium 自动化谷歌浏览器可以这样使用代理
```python
from selenium import webdriver
from selenium.webdriver.common.proxy import Proxy, ProxyType

prox = Proxy()
prox.proxy_type = ProxyType.MANUAL
prox.http_proxy = "127.0.0.1:10800"
prox.ssl_proxy = "127.0.0.1:10800"
# prox.socks_proxy = "127.0.0.1:10800"

capabilities = webdriver.DesiredCapabilities.CHROME
prox.add_to_capabilities(capabilities)

driver = webdriver.Chrome(desired_capabilities=capabilities)

driver.get('https://youtube.com')

input()
```

---

### 使用缺省用户的profile运行浏览器

前面我们selenium打开浏览器，都是创建一个临时的新的用户，在新的用户环境中运行自动化。

如果我们想使用现有缺省用户的 profile运行浏览器自动化，可以这样

```python
from selenium import webdriver
from selenium.webdriver.chrome.options import Options


options = Options()
# 缺省使用的是该目录下面的 Default目录里面保存的用户profile
options.add_argument(r'user-data-dir=c:\Users\baiyh\AppData\Local\Google\Chrome\User Data')
driver = webdriver.Chrome(options=options)
```

---

### 自动化手工打开的Chrome浏览器

Selenium自动化打开的浏览器，每次都是使用全新的profile，有的网站用Selenium自动化，会有奇怪的问题（可能是profile的原因），比如不能登录，打开首页是空白等等。

这时，我们可以

1.关闭所有的Chrome浏览器

2.找到chrome的安装目录，打开命令行窗口，cd进入该目录，

3.输入如下命令，手动启动Chrome浏览器，指定debug端口。

```
chrome.exe --remote-debugging-port=9222
```

因为没有 –user-data-dir 参数，使用的是缺省用户profile。就是我们手动直接启动Chrome使用的profile。


这一步，也可以不这么麻烦，参考[这篇文章](https://stackoverflow.com/questions/51563287/how-to-make-chrome-always-launch-with-remote-debugging-port-flag/56457835#56457835)，修改chrome桌面快捷图标启动参数，修改后双击打开Chrome

然后 手动操作浏览器网页，比如登录，进入到可以自动化的状态，

然后，自动化程序中这样写

```python
from selenium import webdriver
from selenium.webdriver.chrome.options import Options


options = Options()
# 指定Chrome的debug地址 和前面命令行中启动参数一致
# 这样，就会直接自动化刚才启动的浏览器
options.add_experimental_option("debuggerAddress", "127.0.0.1:9222")

wd = webdriver.Chrome(options=options)
wd.implicitly_wait(10)

# 下面接着写自动化的代码
```

---

### 无头模式

```python
from selenium import webdriver
from selenium.webdriver.chrome.options import Options


options = Options()
# 设置为无头模式参数
options.add_argument("--headless")
driver = webdriver.Chrome(options=options)

driver.implicitly_wait(10)

driver.get('http://www.ahtba.org.cn/Category/Detail?id=568')

eles = driver.find_elements_by_css_selector('#night_7460 li a')
for ele in eles:
    print(ele.text)
```


