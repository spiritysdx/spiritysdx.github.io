# 青龙面板搭建教程

# 2.8版本青龙面板搭建教程

### step1 需要有一个云服务器，推荐我自用的腾讯云无忧计划：

[优惠渠道](https://curl.qcloud.com/pgLkw0ot)

优点：每月续费同价，可随时重置服务器系统。

我的配置： 

```
1核2g内存的北京-轻量云服务器

每月续费15元

系统应用重置为Ubuntu20.0版本
```

[学生机优惠](https://curl.qcloud.com/pgLkw0ot)

优点：一年98，可同价位续费三次

缺点：有资格限制，需要实名认证在25岁以下

腾讯云买完后进入轻量云服务器后台有个防火墙管理，添加端口号```5700,8888```，给面板开放端口。

### step2 装宝塔页面，方便文件管理，不用到处cd开文件夹

宝塔安装页面：[点我跳转](https://www.bt.cn/bbs/thread-19376-1-1.html)

等待宝塔安装完成并进入宝塔面板，等待默认LAMP安装完成。

### step3 在软件商店搜索docker，安装docker管理器

### step4 在终端输入进入管理员模式

终端输入，一行一行执行

```
sudo -i  

yum update

curl -sSL https://get.daocloud.io/docker | sh

sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

sudo systemctl start docker
```

### step5 拉取镜像

终端输入
```
docker pull whyour/qinglong:latest
```

### step6 安装青龙

终端输入
```
docker run -dit \
-v $pwd/ql/config:/ql/config \
-v $pwd/ql/log:/ql/log \
-v $pwd/ql/db:/ql/db \
-v $pwd/ql/scripts:/ql/scripts \
-v $pwd/ql/jbot:/ql/jbot \
-p 5700:5700 \
-e ENABLE_HANGUP=true \
-e ENABLE_WEB_PANEL=true \
--name qinglong \
--hostname qinglong \
--restart always \
whyour/qinglong:latest
```

等待运行完成后，终端输入
```
firewall-cmd --zone=public --add-port=5700/tcp --permanent
```

### step6 查看密码

先浏览器打开```http://你服务器外网IP:5700```

默认帐号密码均为admin，输入登录后提示查看密码

在宝塔面板的文件页面搜索ql，找到文件夹后在```ql/config```文件夹里找到```auth.json```,里面是账号和密码。

### step7 更新密码

你服务器ip:5700  

是你的青龙面板地址，登陆青龙面板后在设置里修改面板用户名和密码。

如果忘了用户名和密码可以自己看```step6```中的```auth.json```找回。

### step8 新建任务

定时任务里```添加任务```

名称命名(命名任务保证搜索容易查找拉取库)

```
任务1 XXX
任务2 XXX
……
```

命令内容

一行一个新建定时，定时写

```0 6 * * * ```

定时拉取脚本更新

```
ql repo https://github.com/passerby-b/JDDJ.git
ql repo https://github.com/ZCY01/daily_scripts.git "jd_"
ql repo https://github.com/longzhuzhu/nianyu.git "qx" “main”
ql repo https://github.com/whyour/hundun.git "quanx" "tokens|caiyun|didi|donate|fold|Env"
ql repo https://github.com/huiyi9420/monk-coder_dust.git "i-chenzhe|normal|member|car" "backup"
ql repo https://github.com/zooPanda/zoo.git "zoo"
ql repo https://github.com/star261/jd.git "scripts" "code"
ql repo https://github.com/panghu999/jd_scripts.git "jd_|jx_|getJDCookie" "activity|backUp" "^jd[^_]|USER"
```

```确认```完后点```操作列```中的```开始按钮```执行一次，等待完成后点```操作列```中的```禁用按钮```禁用，建议禁用，避免作者删库跑路，也可以不禁用，但不建议新手如此操作。

**[7月6日更新的JD脚本仓库合集，拉取库点这个用最新的](https://www.spiritlhl.top/35/)**

### step9 添加ck

在环境变量里

变量名  

```
JD_COOKIE
```

变量值

一行一个ck

格式

```
pt_key=1111111111;pt_pin=111111;
pt_key=2222222222;pt_pin=222222;
```

备注随便写，没影响

### step10 配置推送

在配置文件里看注释说明配置

推荐：

```
## Push Plus
export PUSH_PLUS_TOKEN=""
export PUSH_PLUS_USER=""
```

其中 PUSH_PLUS_TOKEN 是[http://pushplus.plus/](http://pushplus.plus/) 注册登录后提供的Token，必填

PUSH_PLUS_USER 选填，一对一则不填，一对多必填，填入pushplus群组编号

完事了

### ck获取教程

[获取教程](https://shimo.im/docs/CTwhjpG6ydvC3qJJ/)

注意：这里不推荐用扫码登陆，因为cookie有效期只有一天，用手机跟验证码登陆后，cookie可以保持很久，大概有效期1个月多。

按照教程获取到的ck，使用它的```pt_key```=```抓到的对应值内容;```,```pt_pin```=```抓到的对应值内容;```，填入```step9```。

### 本地ck获取

某大佬仓库：

[https://github.com/scjtqs/jd_cookie](https://github.com/scjtqs/jd_cookie)

### 个人挂机项目收集

↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓

**[相关挂机项目点我跳转](https://git.spiritlhl.workers.dev/spiritLHL/Hang-up-items)**

# 欢迎请站长喝一杯

![](https://i.loli.net/2021/07/15/UPk5VbzAIC6OM7y.jpg)

