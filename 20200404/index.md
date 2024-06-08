# hugo博客在GitHub上进行部署


#### <font color=cyan>本地搭建博客</font>
  创建新文章  
  hugo new 你的文档名/你的文章名.md

#### <font color=cyan>创建库</font>
  1.名称一定是：你的用户名.github.io  
  2.选择本地存储复制

#### <font color=cyan>部署到云端</font>
  1.在config.toml中改参数  
  2.生成public:  
  打开文件根目录的cmd或git bash here输入  
  ```
  hugo  
  ```
  3.进入public   
  4.在该页面下打开git bash here  
  5.相继输入以下代码  
```
git init 
git add -A
git commit -am"init"
git remote add origin https://github.com/你的用户名/你的用户名.github.io.git
（假如是我：git remote add origin https://github.com/spiritLHL/spiritLHL.github.io.git）
git push -f origin master
（无响应则再次运行同一代码，最多两次）
```
#### <font color=cyan>你的博客:https://你的用户名.github.io/</font>  
假如是我的博客：[https://spiritLHL.github.io/](https://spiritLHL.github.io)    
欢迎访问  
[10分钟教你简单搭建个人博客hugo篇](https://www.bilibili.com/video/BV13c411h7k7/)   
[10分钟教你简单部署hugo博客(github篇)](https://www.bilibili.com/video/BV1WA411h76h/)  
里面有我搭建博客和部署云端的流程及效果。

下载资源链接:   
[https://pan.baidu.com/s/1As27iCyZ5-q3QvgOE4zdPw](https://pan.baidu.com/s/1As27iCyZ5-q3QvgOE4zdPw)  
提取码：f34x

# 注意

如果要绑定第三方域名给github-pages，每次更新博客都需要重新绑定！

