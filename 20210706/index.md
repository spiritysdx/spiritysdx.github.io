# JD脚本仓库合集

## 一、使用说明

- 更新一个整库脚本

```
ql repo <repourl> <path> <blacklist> <dependence> <branch>
```

--------------------------

- 更新单个脚本文件


```
ql raw <fileurl>
```

----------------


## 二、拉取整库实例【以下仓库排名不分先后，纯粹随机排列】

（一）某已退圈并不愿透露姓名大佬库 的现存备份托管库`（下列方案选1个就行，排名不分先后）

1. panghu999维护版仓库

名称：
拉取胖虎代维护

命令：
```
ql repo https://github.com/panghu999/jd_scripts.git "jd_|jx_|getJDCookie" "activity|backUp|jd_delCoupon|format_" "^jd[^_]|USER"
```
定时规则：
```
0 */6 * * *
```
2. JDHelloWorld维护版仓库

名称：
拉取Hello

命令：
```
ql repo https://github.com/JDHelloWorld/jd_scripts.git "jd_|jx_|getJDCookie" "activity|backUp|jd_delCoupon" "^jd[^_]|USER"
```
定时规则：
```
5 */6 * * *
```
3. chinnkarahoi维护版仓库

名称：
拉取chinnkarahoi

命令：
```
ql repo https://github.com/chinnkarahoi/jd_scripts.git "jd_|jx_|getJDCookie" "activity|backUp|jd_delCoupon" "^jd[^_]|USER"
```
定时规则：
```
10 */6 * * *
```
4. `he1pu`（自动提交助力码-京喜工厂、种豆得豆、东东工厂、东东农场、健康社区、京喜财富岛、东东萌宠、闪购盲盒，随机从数据库中选取助力码互助）

名称：
拉取he1pu

命令：
```
ql repo https://github.com/he1pu/JDHelp.git "jd_|jx_|getJDCookie" "activity|backUp|jd_delCoupon" "^jd[^_]|USER"
```
定时规则：
```
15 */6 * * *
```

（二）`longzhuzhu（龙珠）`仓库

名称：
拉取龙珠

命令：
```
ql repo https://github.com/longzhuzhu/nianyu.git "qx"
```
定时规则：
```
20 */6 * * *
```

（三）`whyour/hundun（混沌）`仓库

名称：
拉取混沌

命令：
```
ql repo https://github.com/whyour/hundun.git "quanx" "tokens|caiyun|didi|donate|fold|Env"
```

定时规则：
```
25 */6 * * *
```

（四）`passerby-b`仓库

名称：
拉取passerby-b

命令：
```
ql repo https://github.com/passerby-b/JDDJ.git "jddj_" "scf_test_event" "jddj_cookie"
```
定时规则：
```
30 */6 * * *
```
（五）`Wenmoux（温某人）`仓库

名称：
拉取温某人

命令：
```
ql repo https://github.com/Wenmoux/scripts.git "other|jd" "" "" "wen"
```
定时规则：
```
0 */4 * * *
```
（六）`panghu999/panghu（胖虎原创）`仓库

名称：
拉取胖虎原创

命令：
```
ql repo https://github.com/panghu999/panghu.git "jd_"
```
定时规则：
```
0 */4 * * *
```
（七）`zoopanda（动物园）`仓库

名称：
拉取动物园

命令：
```
ql repo https://github.com/zooPanda/zoo.git "zoo"
```
定时规则：
```
0 */4 * * *
```
（八）`hyzaw（ddo）`仓库

名称：
拉取ddo

命令：
```
ql repo https://github.com/hyzaw/scripts.git "ddo_"
```
定时规则：
```
0 */4 * * *
```
（九）`Ariszy (原名Zhiyi-N)`仓库

名称：
拉取执意

命令：
```
ql repo https://github.com/Ariszy/Private-Script.git "JD"
```
定时规则：
```
0 */5 * * *
```
（十）`ZCY01`仓库

名称：
拉取ZCY01

命令：
```
ql repo https://github.com/ZCY01/daily_scripts.git "jd_"
```
定时规则：
```
0 */5 * * *
```
（十一）`monk-dust/dust（藏经阁）`oreomeow备份版仓库

名称：
拉取藏经阁

命令：
```
ql repo https://github.com/Oreomeow/dust.git "i-chenzhe|normal|member|car" "backup"
```
定时规则：
```
0 */5 * * *
```
（十二）`star261`仓库

名称：
拉取star

命令：
```
ql repo https://github.com/star261/jd.git "scripts" "code"
```
定时规则：
```
0 */5 * * *
```
（十三）`curtinlv（TopStyle）`仓库

名称：
拉取TopStyle

命令：
```
ql repo https://github.com/curtinlv/JD-Script.git
```
定时规则：
```
0 */5 * * *
```
（十四）`moposmall`仓库

名称：
拉取moposmall

命令：
```
ql repo https://github.com/moposmall/Script.git "Me"
```
定时规则：
0 */6 * * *
（十五） `photonmang`（宠汪汪及兑换、点点券修复）

名称：
拉取photonman

命令：
```
ql repo https://github.com/photonmang/quantumultX.git "JDscripts"
```
定时规则：
```
0 */6 * * *
```
（十六）`cdle`

名称：
拉取cdle

命令：
```
ql repo https://github.com/cdle/jd_study.git
```
定时规则：
```
0 */6 * * *
```
## 三、单脚本（定时规则都设置为 0 */8 * * * 即可）

（一）翻翻乐提现单文件

```
ql raw https://raw.githubusercontent.com/jiulan/platypus/main/scripts/jd_ffl.js
```
（二）`curtinlv`（上面拉过仓库的可以不用拉了）

```
15 8 * * *
```
>> 赚京豆

```
ql raw https://raw.githubusercontent.com/curtinlv/JD-Script/main/jd_zjd.py
```

>> 入会

```
ql raw https://raw.githubusercontent.com/curtinlv/JD-Script/main/OpenCard/jd_OpenCard.py
```

>> 关注

```
ql raw https://raw.githubusercontent.com/curtinlv/JD-Script/main/getFollowGifts/jd_getFollowGift.py
```

# 欢迎请站长喝一杯

![](https://i.loli.net/2021/07/15/UPk5VbzAIC6OM7y.jpg)
