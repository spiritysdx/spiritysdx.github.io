# hugo博客部署码云


#### <font color=cyan></font>本地搭建博客
  创建新文章  
  hugo new 你的文档名/你的文章名.md

### <font color=cyan></font>在码云中创建库
  1.链接一定是：/(填你的用户名)  
  2.选择公开/私有都行  
  3.注意不要初始化库(三个选项都不要选)  
  4.创建库  
  5.复制库链接(https类型)  

### <font color=cyan></font>部署到云端
  1.在config.toml中改参数:baseURL = "https://你的用户名.gitee.io"  
  2.生成public: (初始化你的博客)  
  打开文件根目录的cmd或git bash here输入  
  ```
  hugo --theme=你的主题
  ```
  3.进入public的文档，在该文档下打开git bash here  
  4.相继输入以下代码：  
  ```
  git init
  git add .
  git commit -m "first commit"
  git remote add origin (https://gitee.com/......)(刚才复制的网址填这里)
  (假如是我就会填入https://gitee.com/spiritlhl/spiritLHL.git)
  git push -u origin master 
  (无响应则再次运行同一代码)
  ```
  5.进入之前你创建的库所在页面，刷新  
  6.鼠标打开服务，选择Gitee Pages，点击进入  
  7.选择强制使用HTTPS  
  8.启用服务  
  9.等待你的博客链接创建出来  

你的博客:https://你的用户名.gitee.io/  
假如是我的博客:[https://spiritlhl.gitee.io/](https://spiritlhl.gitee.io/)

## <font color=red>注意，你的每一次提交都需要在Gitee Pages中手动更新，自动更新需要购买，这一点上我推荐使用Github</font>

## <font color=cyan>Github只需要你上传就自动更新页面了，免费给你使用该功能</font>

欢迎访问  
[10分钟教你简单搭建个人博客hugo篇](https://www.bilibili.com/video/BV13c411h7k7/)  
[8分钟教你简单部署hugo博客到码云gitee--含github博客转码云gitee的方法](https://www.bilibili.com/video/BV17p4y1C78w/)  
里面有我搭建博客和部署云端的流程及效果。






