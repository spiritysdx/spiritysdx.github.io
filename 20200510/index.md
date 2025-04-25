# Selenium的web自动化操作01(环境布置与标准流程)

### 前言

需要下载Chrome或Firefox的driver，Chrome内核81.440与Firefox内核74.0下载链接如下：

[Firefox](https://spiritlhl.lanzous.com/icg1k3c)

[Chrome](https://spiritlhl.lanzous.com/icg1k0j)

其他版本请在搜索引擎查找，本篇使用该版本，注意，driver下载后需要配置对应内核的游览器，电脑本身需要有该内核的游览器。

---

### web自动化操作流程

```python
from selenium import webdriver
import time

wd = webdriver.Chrome(r'C:\chromedriver.exe')
'''
wd.get('https://www.baidu.com')
#webdriver类选择的是整个页面
element = wd.find_element_by_id("kw")
#class，name，id(id比较精确)
#通过webelement对象进行操作
element.send_keys('csdn')#输入  如果输入csdn\n可以模拟回车键
element = wd.find_element_by_id('su')#点击搜索
element.click()#点击
'''
wd.get('https://blog.csdn.net/spiritLHL/article/details/105252792?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522158864611119724848300006%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.57677%2522%257D&request_id=158864611119724848300006&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_v2~rank_v25-1')
#elements = wd.find_element_by_class_name("blog-content-box")
#time.sleep(2)#模拟点击等待的时间(找不找得到都会等待)
wd.implicitly_wait(5)#更方便的等待时间(找不到元素才会等待)
elements = wd.find_elements_by_tag_name("p")
for e in elements:
    print(e.text)
#print(elements.get_attribute('href'))
wd.close()#关闭游览器
```

---

### find_element和find_elements的区别

使用find_elements选择的是符合条件的所有元素，如果没有符合条件的元素，回空列表   
使用find_element选择的是符合条件的第一个元素，如果没有符合条件的元素，抛出异常   

获取属性值--->   ```element.get_attribute('href')```

获取整个元素对应html--->   ```elements.get_attribute('outerHTML')```

获取某元素内所有html文本内容--->  ```elements.get_attribute('innerHTML')```

获取输入框内的文本内容--->   ```elements.get_attribute('value')```

---

#### 获取元素文本内容

通过WebElement对象的text属性，可以获取元素展示在界面上的文本内容。  

但是，有时候，元素的文本内容没有展示在界面上，或者没有完全完全展示在界面上。这时用WebElement的text属性,获取文本内容，就会有问题。   

出现这种情况，可以尝试使用  
```python
element.get_attribute('innerText')
```
或者
```python
element.get_attribute('textContent')
```

---

## css选择器

```python
wd.find_element_by_css_selector()
```

CSS Selector同样可以根据tag名、id 属性和class属性来选择元素`。  

根据tag名选择元素的CSS Selector语法非常简单，直接写上tag名即可。

比如要选择所有的tag名为div的元素,就可以是这样
```python
elements = wd.find_elements_by_css_selector('div')
```
等价于
```python
elements = wd.find_elements_by_tag_name('div')
```

---

### 根据属性值定位

```python
element = wd.find_element_by_css_selector('[href="http://ww.miitbeian.gov.cn"]')
```

---

### 验证所找元素是否正确书写格式

在开发者工具中按ctrl+f，输入所写格式，若存在着会高亮显示    
css选择某个class的所有，写 .class0 (表示选中名为class0的所有标签)     
css选择两个不同名class(都选中)(中间写英文逗号）写  .class1 , .class2     
逗号左右的属性类型可以不同     

---

### 父元素 

---

##### 父元素的第几个某类型的子节点  
我们可以指定选择的元素是```父元素的第几个某类型的子节点```(要选择元素的上一层级为父节点)
使用 ```nth-of-type```  
比如，我们要选择class1和class2的第几个元素，   
可以像上面那样思考：选择的是第2个子元素，并且是span类型
所以这样可以这样写 ```span:nth-child(2)```    
还可以这样思考，选择的是第1个span类型的子元素
所以也可以这样写 ```span:nth-of-type(1)```   

---

##### 父元素的倒数第几个某类型的子节点
当然也可以反过来，选择父元素的倒数第几个某类型的子节点
使用 ```nth-last-of-type```  
像这样
```p:nth-last-of-type(2)```   

---

#### 相邻兄弟节点选择
上面的例子里面，我们要选择我们要选择class1和class2的第几个元素，
还有一种思考方法，就是选择h3后面紧跟着的兄弟节点span。
这就是一种相邻兄弟关系，可以这样写```h3 + span```
表示元素紧跟关系的是加号

---

#### 后续所有兄弟节点选择
如果要选择是选择 h3 后面所有的兄弟节点span可以这样写 ```h3 ~ span```

---

### 内嵌

内嵌html页面的，使用wd.switch_to.frame(frame_reference)切换到内嵌
页面进行操作。   
其中，```frame_reference```可以是```frame```元素的属性```name```或者```ID```。       
比如这里，就可以填写```iframe```元素的id```'frame1'``` 或者```name```属性值```'innerFrame'```。   
像这样   
```python
wd.switch_to.frame('frame1')
```
或者
```python
wd.switch_to.frame('innerFrame')
```
也可以填写```frame```所对应的```WebElement```对象。   
我们可以根据```frame```的元素位置或者属性特性，使用```find```系列的方法，选择到该元素，得到对应的```WebElement```对象    
比如，这里就可以写  
```python
wd.switch_to.frame(wd.find_element_by_tag_name("iframe"))
```
然后，就可以进行后续操作```frame```里面的元素了。     
上面的例子的正确代码如下   
```python
from selenium import webdriver


wd = webdriver.Chrome(r'd:\webdrivers\chromedriver.exe')
wd.get('http://cdn1.python3.vip/files/selenium/sample2.html')

# 先根据name属性值 'innerFrame'，切换到iframe中
wd.switch_to.frame('innerFrame')

# 根据 class name 选择元素，返回的是 一个列表
elements = wd.find_elements_by_class_name('plant')

for element in elements:
    print(element.text)

#如果我们已经切换到某个iframe里面进行操作了，那么后续选择和操作界面元素 就都是在这个frame里面进行的。

#这时候，如果我们又需要操作主html（我们把最外部的html称之为主html）里面的元素了呢？

#怎么切换回原来的主html呢？

#很简单，写如下代码即可

wd.switch_to.default_content()
#例如，在上面 代码 操作完 frame里面的元素后， 需要 点击 主html 里面的按钮，就可以这样写

from selenium import webdriver

wd = webdriver.Chrome(r'd:\webdrivers\chromedriver.exe')

wd.get('http://cdn1.python3.vip/files/selenium/sample2.html')

# 先根据name属性值 'innerFrame'，切换到iframe中
wd.switch_to.frame('innerFrame')

# 根据 class name 选择元素，返回的是 一个列表
elements = wd.find_elements_by_class_name('plant')

for element in elements:
    print(element.text)

# 切换回 最外部的 HTML 中
wd.switch_to.default_content()

# 然后再 选择操作 外部的 HTML 中 的元素
wd.find_element_by_id('outerbutton').click()

wd.quit()
```

如果我们要到新的窗口里面操作，该怎么做呢？    
可以使用```Webdriver```对象的```switch_to```属性的```window```方法，如下所示：    
```python
wd.switch_to.window(handle)
```
其中，参数```handle```需要传入什么呢？    
WebDriver对象有```window_handles```属性，这是一个列表对象，里面包括了当前浏览器里面所有的窗口句柄。   
所谓句柄，大家可以想象成对应网页窗口的一个ID，     
那么我们就可以通过类似下面的代码，   
```python
for handle in wd.window_handles:
    # 先切换到该窗口
    wd.switch_to.window(handle)
    # 得到该窗口的标题栏字符串，判断是不是我们要操作的那个窗口
    if 'Bing' in wd.title:
        # 如果是，那么这时候WebDriver对象就是对应的该该窗口，正好，跳出循环，
        break
```
上面代码的用意就是：     
我们依次获取```wd.window_handles```里面的所有句柄对象，并且调用```wd.switch_to.window(handle)```方法，切入到每个窗口，然后检查里面该窗口对象的属性(可以是标题栏，地址栏)判断是不是我们要操作的那个窗口，如果是，就跳出循环。    
同样的，如果我们在新窗口操作结束后， 还要回到原来的窗口，该怎么办？   
我们可以仍然使用上面的方法，依次切入窗口，然后根据标题栏之类的属性值判断。    
还有更省事的方法:      
因为我们一开始就在原来的窗口里面，我们知道进入新窗口操作完后，还要回来，可以事先保存该老窗口的句柄，使用如下方法   
(mainWindow变量保存当前窗口的句柄)
```python
mainWindow = wd.current_window_handle
```

切换到新窗口操作完后，就可以直接像下面这样，将driver对应的对象返回到原来的窗口
(通过前面保存的老窗口的句柄，自己切换到老窗口)
```python
wd.switch_to.window(mainWindow)
```

---
