# 阿里云、腾讯云、甲骨文云、华为云、UCLOUD的监控卸载


## 前言

众所周知的原因，服务器大厂的监控一直是介于保护隐私和侵犯隐私的边缘的，所以为了去除掉这些烦人的监控，我开发了下面的脚本进行一键卸载

## 脚本

脚本仓库：https://github.com/spiritLHLS/one-click-installation-script

一键删除平台监控

- 一键移除大多数云服务器监控
- 涵盖阿里云、腾讯云、华为云、UCLOUD、甲骨文云、京东云

国际：

```
curl -L https://raw.githubusercontent.com/spiritLHLS/one-click-installation-script/main/install_scripts/dlm.sh -o dlm.sh && chmod +x dlm.sh && bash dlm.sh
```

国内

```
curl -L https://cdn0.spiritlhl.top/https://raw.githubusercontent.com/spiritLHLS/one-click-installation-script/main/install_scripts/dlm.sh -o dlm.sh && chmod +x dlm.sh && bash dlm.sh
```

或

```
curl -L https://ghproxy.com/https://raw.githubusercontent.com/spiritLHLS/one-click-installation-script/main/install_scripts/dlm.sh -o dlm.sh && chmod +x dlm.sh && bash dlm.sh
```

## 后言

卸载后，无论怎么重启服务器监控都不会启动了，目前网上的其他教程重启后仍旧会自启动监控，这是本脚本对比它们的优化之处
