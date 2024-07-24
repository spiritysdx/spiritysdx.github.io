# Selenium的web自动化操作03(语法补充)

## **前言**

需要下载Chrome或Firefox的driver，Chrome内核81.440与Firefox内核74.0下载链接如下：

[Firefox](https://spiritlhl.lanzous.com/icg1k3c)

[Chrome](https://spiritlhl.lanzous.com/icg1k0j)

其他版本请在搜索引擎查找，本篇使用该版本，注意，driver下载后需要配置对应内核的游览器，电脑本身需要有该内核的游览器。

---

## 正文

---

## Xpath语法简介

可以发现CSS选择元素非常灵活、强大。

还有一种灵活、强大的选择元素的方式，就是使用Xpath表达式。

XPath (XML Path Language) 是由国际标准化组织W3C指定的，用来在XML和HTML文档中选择节点的语言。

目前主流浏览器 (chrome、firefox，edge，safari) 都支持XPath语法，xpath有 1 和 2 两个版本，目前浏览器支持的是 xpath 1的语法。

既然已经有了CSS，为什么还要学习Xpath呢？因为

有些场景用css选择web元素很麻烦，而xpath却比较方便。

另外Xpath还有其他领域会使用到，比如爬虫框架Scrapy，手机App框架Appium。

测试网址：http://cdn1.python3.vip/files/selenium/test1.html

按F12打开调试窗口，点击Elements标签。

要验证Xpath语法是否能成功选择元素，也可以像验证CSS语法那样，按组合键```Ctrl + F```就会出现搜索框


xpath语法中，整个HTML文档根节点用’/‘表示，如果我们想选择的是根节点下面的html节点，则可以在搜索框输入
```
/html
```
如果输入下面的表达式
```
/html/body/div
```
这个表达式表示选择html下面的body下面的div元素。

注意```/```有点像CSS中的```>```,表示直接子节点关系。

---

### 绝对路径选择

从根节点开始的，到某个节点，每层都依次写下来，每层之间用```/```分隔的表达式，就是某元素的绝对路径

上面的xpath表达式```/html/body/div```，就是一个绝对路径的xpath表达式，等价于css表达式```html>body>div```

自动化程序要使用Xpath来选择web元素，应该调用WebDriver对象的方法```find_element_by_xpath```或者```find_elements_by_xpath```，像这样：

```python
elements = driver.find_elements_by_xpath("/html/body/div")
```

---

### 相对路径选择

有的时候，我们需要选择网页中某个元素，不管它在什么位置 。

比如，选择示例页面的所有标签名为```div```的元素，如果使用css表达式，直接写一个div就行了。

那xpath怎么实现同样的功能呢？xpath需要前面加```//```, 表示从当前节点往下寻找所有的后代元素,不管它在什么位置。

所以xpath表达式，应该这样写：```//div```


‘//’ 符号也可以继续加在后面,比如，要选择所有的```div```元素里面的所有的```p```元素 ，不管```div```在什么位置，也不管```p```元素在```div```下面的什么位置，则可以这样写```//div//p```

对应的自动化程序如下

```python
elements = driver.find_elements_by_xpath("//div//p")
```

如果使用CSS选择器，对应代码如下
```python
elements = driver.find_elements_by_css_selector("div p")
```

如果，要选择所有的```div```元素里面的直接子节点p，xpath就应该这样写了```//div/p```

如果使用CSS选择器，则为```div > p```

---

### 通配符

如果要选择所有div节点的所有直接子节点，可以使用表达式```//div/*```

```*```是一个通配符，对应任意节点名的元素，等价于CSS选择器```div > *```

代码如下：
```python
elements = driver.find_elements_by_xpath("//div/*")
for element in elements:
    print(element.get_attribute('outerHTML'))
```

---

### 根据属性选择

Xpath 可以根据属性来选择元素。

根据属性来选择元素是通过这种格式来的```[@属性名='属性值']```

---

##### 注意：

属性名注意前面有个@

属性值一定要用引号，可以是单引号，也可以是双引号

---

#### 根据id属性选择

选择id为west的元素，可以这样```//*[@id='west']```

---

#### 根据class属性选择

选择所有select元素中class为```single_choice```的元素，可以这样```//select[@class='single_choice']```

如果一个元素class有多个，比如

```
<p id="beijing" class='capital huge-city'>
    北京
</p>
```

如果要选它，对应的xpath就应该是```//p[@class="capital huge-city"]```

不能只写一个属性，像这样```//p[@class="capital"]```则不行

---

#### 根据其他属性

同样的道理，我们也可以利用其它的属性选择

比如选择具有multiple属性的所有页面元素，可以这样```//*[@multiple]```

---

#### 属性值包含字符串

要选择style属性值包含color字符串的页面元素，可以这样```//*[contains(@style,'color')]```

要选择style属性值以color字符串开头的页面元素，可以这样```//*[starts-with(@style,'color')]```

style属性值以某个字符串结尾的页面元素，大家可以推测是```//*[ends-with(@style,'color')]```，但是很遗憾，这是xpath 2.0 的语法，目前浏览器都不支持。

---

#### 按次序选择

前面学过css表达式可以根据元素在父节点中的次序选择，非常实用。

xpath也可以根据次序选择元素。语法比css更简洁，直接在方括号中使用数字表示次序

比如

某类型```第几个```子元素

比如

要选择p类型第2个的子元素，就是

```
//p[2]
```

注意，选择的是```p```类型第2个的子元素，不是第2个子元素，并且是```p```类型 。

再比如，要选取父元素为```div```中的```p```类型```第2个```子元素

```
//div/p[2]
```

---

### 第几个子元素

也可以选择第2个子元素，不管是什么类型，采用通配符

比如 选择父元素为div的第2个子元素，不管是什么类型

```
//div/*[2]
```

---

### 某类型倒数第几个子元素

当然也可以选取倒数第几个子元素

比如：

选取p类型倒数第1个子元素 ---> ```//p[last()]```
选取p类型倒数第2个子元素 ---> ```//p[last()-1]```
选择父元素为div中p类型倒数第3个子元素 --->```//div/p[last()-2]```

---

### 范围选择

xpath还可以选择子元素的次序范围。

比如，

选取option类型第1到2个子元素

```
//option[position()<=2]
```
或者
```
//option[position()<3]
```


选择class属性为multi_choice的前3个子元素
```
//*[@class='multi_choice']/*[position()<=3]
```



选择class属性为multi_choice的后3个子元素
```
//*[@class='multi_choice']/*[position()>=last()-2]
```


为什么不是 last()-3 呢？因为

```last()```本身代表最后一个元素

```last()-1```本身代表倒数第2个元素

```last()-2```本身代表倒数第3个元素

---

### 组选择、父节点、兄弟节点

---

##### 组选择

css有组选择，可以同时使用多个表达式，多个表达式选择的结果都是要选择的元素

css组选择，表达式之间用逗号隔开

xpath也有组选择，是用竖线隔开多个表达式

比如，要选所有的option元素和所有的h4元素，可以使用
```
#xpath
//option | //h4
```
等同于CSS选择器
```
#css
option , h4
```

再比如，要选所有的```class```为```single_choice```和```class```为```multi_choice```的元素，可以使用
```
#xpath
//*[@class='single_choice'] | //*[@class='multi_choice']
```
等同于CSS选择器
```
#css
.single_choice , .multi_choice
```

---

### 选择父节点

xpath可以选择父节点，这是css做不到的。

某个元素的父节点用```/..```表示

比如，要选择```id```为```china```的节点的父节点，可以这样写```//*[@id='china']/..``` 。

当某个元素没有特征可以直接选择，但是它有子节点有特征， 就可以采用这种方法，先选择子节点，再指定父节点。


还可以继续找上层父节点，比如```//*[@id='china']/../../..```

---

### 兄弟节点选择

前面学过css选择器，要选择某个节点的后续兄弟节点，用波浪线

xpath也可以选择后续兄弟节点，用这样的语法```following-sibling::```

比如，要选择```class```为```single_choice```的元素的所有后续兄弟节点```//*[@class='single_choice']/following-sibling::*```

等同于CSS选择器```.single_choice ~ *```

如果，要选择后续节点中的div节点，就应该这样写```//*[@class='single_choice']/following-sibling::div```

xpath还可以选择前面的兄弟节点，用这样的语法```preceding-sibling::```

比如，要选择```class```为```single_choice```的元素的所有前面的兄弟节点```//*[@class='single_choice']/preceding-sibling::*```

而CSS选择器目前还没有方法选择前面的兄弟节点

要了解更多Xpath选择语法，可以点击[这里](https://www.w3school.com.cn/xpath/index.asp)，打开Xpath选择器参考手册

---

### selenium 注意点

原代码：

    先选择示例网页中，id是china的元素

    然后通过这个元素的WebElement对象，使用find_elements_by_xpath，选择里面的p元素，

```python
# 先寻找id是china的元素
china = wd.find_element_by_id('china')

# 再选择该元素内部的p元素
elements = china.find_elements_by_xpath('//p')

# 打印结果
for element in elements:
    print('----------------')
    print(element.get_attribute('outerHTML'))
```

运行发现，打印的不仅仅是china内部的p元素，而是所有的p元素。

要在**某个元素内部**使用xpath选择元素，**需要在xpath表达式最前面加个点**。

像这样
```python
elements = china.find_elements_by_xpath('.//p')
```
