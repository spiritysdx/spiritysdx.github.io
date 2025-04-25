# Appium自动化操作02(元素定位及查看工具)

# 前言

最好有```Selenium```的Web自动化的实际经验

运行基础：client库(0.52版本)，Appium Server，安卓SDK(含JDK环境)，USB调试模式下的手机(开发者模式)

# 定位元素

从示例代码就可以发现，和```Selenium Web```自动化一样，要操作界面元素，必须先定位(选择)元素。

Appium是基于Selenium的，所以和```Selenium```代码定位元素的基本规则相同：

```find_element_by_XXX```方法，返回符合条件的第一个元素，找不到抛出异常

```find_elements_by_XXX```方法，返回符合条件的所有元素的列表，找不到返回空列表

通过```WebDriver```对象调用这样的方法，查找范围是整个界面

通过```WebElement```对象调用这样的方法，查找范围是该节点的子节点

---

# 界面元素查看工具

做```Selenium Web```自动化的时候，要找到元素，我们是通过浏览器的开发者工具栏来查看元素的特性，根据这些特性（属性和位置），来定位元素

```Appium```要自动化手机应用，同样需要工具查看界面元素的特征。

常用的查看工具： 

```Android Sdk```中的```uiautomateviewer```

```Appium Desktop```中的```Appium Inspector```

---

## uiautomateviewer

安卓查看APP界面元素，最常用的就是```Android SDK```中的工具```uiautomateviewer```，它在SDK目录目录的```tools\bin```目录中

和```Selenium```一样，我们要定位选择元素，也是根据元素的特征，包括:

    元素的属性
    元素的相对位置（相对父元素、兄弟元素等）
    具体细节，参考视频里面的讲解。

---

## Appium Inspector

```Appium Desktop```中的```Appium Inspector```也可以查看元素。

它的一个优点是可以直接验证**选择表达式是否能定位到元素**

---

## 定位元素的方法

1.根据ID

在Selenium Web自动化里，如果能根据ID选择定位元素，最好根据ID，因为通常来说ID是唯一的，所以根据ID选择 效率高。

在安卓应用自动化的时候，同样可以根据ID查找。

但是这个ID，是安卓应用元素的```resource-id```属性

具体代码：

```python
driver.find_element_by_id('')
```

2.根据CLASS NAME

安卓界面元素的```class```属性其实就是根据元素的类型，类似web里面的```tagname```， 所以通常不是唯一的。

通常，我们根据```class```属性来选择元素，是要选择多个而不是一个。

当然如果你确定要查找的界面元素的类型 在当前界面中只有一个，就可以根据```class```来唯一选择。

具体代码：

```python
driver.find_elements_by_class_name('')
```

3.根据ACCESSIBILITY ID

元素的```content-desc```属性是用来描述该元素的作用的。

如果要查询的界面元素有```content-desc```属性，我们可以通过它来定位选择元素。

具体代码：

```python
driver.find_element_by_accessibility_id('')
```

4.Xpath

Appium也支持通过```Xpath```选择元素。

但是其可靠性和性能不如Selenium Web自动化。因为Web自动化对Xpath的支持是由浏览器实现的，而```Appium Xpath```的支持是Appium Server实现的。

毕竟，浏览器产品的成熟度比Appium要高很多。

当然，```Xpath```是标准语法，所以这里表达式的语法规则和Selenium里面```Xpath```的语法是一样的，比如

```python
driver.find_element_by_xpath('//ele0/ele1[@attr="value"]')
```

注意：

selenium自动化中，```xpath```表达式中每个节点名是html的```tagname```。

但是在appium中，```xpath```表达式中每个节点名是元素的```class```属性值。

比如：要选择所有的文本节点，就用如下代码:

```python
driver.find_element_by_xpath('//android.widget.TextView')
```

---

## 参考文档

根据```id```，```classname```， ```accessibilityid```，```xpath```这些方法选择元素，其实底层都是利用了安卓```uiautomator```框架的API功能实现的。

这里是谷歌安卓官方文档介绍： [https://developer.android.google.cn/training/testing/ui-automator](https://developer.android.google.cn/training/testing/ui-automator)

也就是说，程序的这些定位请求，被```Appium server```转发给手机自动化代理程序，就转化为为```uiautomator```里面相应的定位函数调用。

其实，自动化程序，可以直接告诉手机上的自动化代理程序，让它调用```UI Automator API```的java代码，实现最为直接的自动化控制。

主要是通过```UiSelector```这个类里面的方法实现元素定位的，比如

```python
code = 'new UiSelector().text("热门").className("android.widget.TextView")'
ele = driver.find_element_by_android_uiautomator(code)
ele.click()
```

就是通过```text```属性和```className```的属性两个条件来定位元素。


```UiSelector```里面有些元素选择的方法可以解决前面解决不了的问题。

比如

```text```方法

    可以根据元素的文本属性查找元素

```textContains```

    根据文本包含什么字符串

```textStartsWith```

    根据文本以什么字符串开头

```textmartch```方法

    可以使用正则表达式选择一些元素，如下

    ```python
    code = 'new UiSelector().textMatches("^我的.*")'
    ```

```UiSelector```的```instance```和```index```也可以用来定位元素，都是从0开始计数，他们的区别：

    instance是匹配的结果所有元素里面 的第几个元素

    index则是其父元素的几个节点，类似xpath里面的*[n]


```UiSelector```的```childSelector```可以选择后代元素，比如

```python
code = 'new UiSelector().resourceId("tv.danmaku.bili:id/recycler_view").childSelector(new UiSelector().className("android.widget.TextView"))'

ele = driver.find_element_by_android_uiautomator(code)
```

注意：```childSelector```后面的引号要框住整个```uiSelector```的表达式

