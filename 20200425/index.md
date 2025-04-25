# 撰写hugo博客的markdown格式

## 1. 前言
本markdown文档初版下载链接：  
(建议看初版文档理解书写格式，书写标准以本网站本页面为主)
[https://pan.baidu.com/s/1z_2IsuaRh8cYmtssepvIXQ 
](https://pan.baidu.com/s/1z_2IsuaRh8cYmtssepvIXQ)  
提取码：0a83 

本篇鸣谢胡国磊学长整理，由我进行加工发布

## 2. 示例文章

- <span style="color:orangered;font-weight:bold;">橙心：</span>[终于等到你，公众号排版神器](https://mp.weixin.qq.com/s/raFgkqlV5hZmrXiEWVAyfQ)
- <span style="color:#773098;font-weight:bold;">姹紫：</span>[JavaScript 数据结构与算法之美](https://mp.weixin.qq.com/s/KmoRDGdJLZ7reMfTDDaFGg)
- <span style="color:#35b378;font-weight:bold;">绿意：</span>[前端硬核面试专题之 CSS 55 问](https://mp.weixin.qq.com/s/SVKMsQtOLNqYXeT_f95FUw)
- <span style="color:rgb(248,57,41);font-weight:bold;">红绯：</span>[日常 | 我用什么工具写作？](https://mp.weixin.qq.com/s/DrvJBEWqH14atF_4O1IXFw)
- <span style="color:orangered;font-weight:bold;">Wechat-Format</span>：[Markdown Nice 新特性：阿里云图床](https://mp.weixin.qq.com/s/QPsOUkLCsvhqSicTOGaHJg)
- <span style="color:#0e88eb;font-weight:bold;">科技蓝</span>：[2019 前端秋季社招面试经历总结（二年多经验）](https://mp.weixin.qq.com/s/eDIDOESem_s93liccYK-qw)

效果及代码图如下:

![1](https://pic./2020/04/27/2fb934480db8e.png)

## 3 通用语法

### 3.0 分割线

在markdown语法中,一行连用三个或者三个以上的星号,减号,或者下划线,就可以表示分割线。 

例如:*** --- __     

为方便记忆,只需记住三个减号"---"可以表示分割线即可。     

另外,分割线可以起到把分割线上下的内容分割成两个段落的作用。

### 3.1 标题

在文字写书写不同数量的`#`可以完成不同的标题，一行开头打上不同数目的#最后在#末尾加上空格再书写标题内容，效果如下：

# 一级标题

## 二级标题

### 三级标题

### 3.2 无序列表

无序列表的使用，在符号`-`后加空格使用。如下：

- 无序列表 1
- 无序列表 2
- 无序列表 3

如果要控制列表的层级，则需要在符号`-`前使用空格。如下：

- 无序列表 1
- 无序列表 2
  - 无序列表 2.1
  - 无序列表 2.2


### 3.3 有序列表

有序列表的使用，在数字及符号`.`后加空格后再输入内容，如下：

1. 有序列表 1
2. 有序列表 2
3. 有序列表 3

### 3.4 引用

引用的格式是在符号`>`后面书写文字。效果如下：

> 读一本好书，就是在和高尚的人谈话。 ——歌德

> 雇用制度对工人不利，但工人根本无力摆脱这个制度。 ——阮一峰

### 3.5 粗体和斜体

粗体的使用是在需要加粗的文字前后各加两个`*`。

而斜体的使用则是在需要斜体的文字前后各加一个`*`。

如果要使用粗体和斜体，那么就是在需要操作的文字前后加三个`*`。效果如下：

**这个是粗体**

*这个是斜体*

***这个是粗体加斜体***

注：由于 commonmark 标准，可能会导致加粗与想象不一致，如下

**今天天气好晴朗，**处处好风光。

这个是正常现象，请参考[加粗 Issue](https://github.com/markdown-it/markdown-it/issues/410 "加粗 Issue")。

### 3.6 链接

代码图：

![2](https://pic./2020/04/27/7b82a928f7c77.png)

效果如下：


文章链接[你是《未来世界的幸存者》么？](https://zhuanlan.zhihu.com/p/39717516)


### 3.7 分割线

代码图：

![4](https://pic./2020/04/27/e7378f5bdfd3a.png)

可以在一行中用三个以上的减号来建立一个分隔线，同时需要在分隔线的上面空一行。效果如下：

---

### 3.8 删除线

删除线的使用，在需要删除的文字前后各使用两个`~`，效果如下：

~~这是要被删除的内容。~~

### 3.9 表格

可以使用冒号来定义表格的对齐方式，格式如下：

![图片](https://pic./2020/04/27/9e142d6c1a0a5.png)

效果如下：

| 姓名   | 年龄 |     工作 |
| :----- | :--: | -------: |
| 小可爱 |  18  | 吃可爱多 |
| 小小勇敢 |  20  | 爬棵勇敢树 |
| 小小小机智 |  22  | 看一本机智书 |

### 3.10 图片

插入图片，如果是行内图片则无图例，否则有图例，格式图如下：

![5](https://pic./2020/04/27/0e56081913d91.png)

效果如下：

![这里写图片描述](https://imgkr.cn-bj.ufileos.com/22cf98bd-3f85-45fc-9df7-e6b2808329d0.png)

支持 jpg、png、gif 等图片格式，

- 支持图片**拖拽和截图粘贴**到编辑器中上传，上传时使用当前选择的图床。
- 可使用**格式->图片**上传本地图片，网站仅支持「图壳」图床，失败率低可长久保存！

### 4.1 代码块

如果在一个行内需要引用代码，只要用反引号引起来就好，如下：

Use the `printf()` function.

在需要高亮的代码块的前一行及后一行使用三个反引号，同时**第一行反引号后面表示代码块所使用的语言**

格式图如下：
![6](https://pic./2020/04/27/14bcd1f7064f3.png)

效果如下：

```java
// FileName: HelloWorld.java
public class HelloWorld {
  // Java 入口程序，程序从此入口
  public static void main(String[] args) {
    System.out.println("Hello,World!"); // 向控制台打印一条语句
  }
}
```

支持以下语言种类：

```
bash
clojure，cpp，cs，css
dart，dockerfile, diff
erlang
go，gradle，groovy
haskell
java，javascript，json，julia
kotlin
lisp，lua
makefile，markdown，matlab
objectivec
perl，php，python
r，ruby，rust
scala，shell，sql，swift
tex，typescript
verilog，vhdl
xml
yaml
```

如果想要更换代码主题，可在上方挑选，不支持代码主题自定义。

## 5 其他语法

### 5.1 HTML

支持原生 HTML 语法，请写内联样式，格式图如下：

![7](https://pic./2020/04/27/58b5045c3724a.png)

效果如下：

<span style="display:block;text-align:right;color:orangered;">橙色居右</span>
<span style="display:block;text-align:center;color:orangered;">橙色居中</span>

### 5.2 UML

不支持，推荐使用开源工具`https://draw.io/`制作后再导入图片

### 5.3 组件图床

组件目前共支持 3 种图床和 1 种自定义图床，主要特点如下：

| 图床   | 费用     | 有效期 | 失败率 |
| ------ | -------- | ------ | ------ |
| SM.MS  | 免费     | 长期   | 高     |
| 阿里云 | 付费     | 自定义 | 低     |
| 七牛云 | 10G 免费 | 自定义 | 低     |
| 自定义 | 高昂 | 自定义 | 自定义 |

4 个图床的缺点：

| 图床   | 缺点                     |
| ------ | ------------------------ |
| SM.MS  | 失败率高可用性很差       |
| 阿里云 | 配置繁琐，费用昂贵       |
| 七牛云 | 配置繁琐，需购买长期域名 |
| 自定义 | 搭建后台繁琐 |

这里我推荐我经常用的一个图源保存网站：

[https://pic./](https://pic./)

### 5.4 下拉列表(部分主题支持本格式)

代码如下  

```
<details>
<summary>展开查看</summary>
<pre><code>
内容
</code></pre>
</details>
```

效果如下：  

<details>
<summary>展开查看</summary>
<pre><code>
内容
</code></pre>
</details>

### 5.5 更多文档

更多文档请参考 [markdown-nice-docs](https://docs.mdnice.com)


