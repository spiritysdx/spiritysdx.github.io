# 在seleniumwire中解决Not Secure问题


### 解决上一篇文章的一些问题

解决方法：

[https://github.com/wkeeling/selenium-wire/issues/622](https://github.com/wkeeling/selenium-wire/issues/622)

下载证书

[https://github.com/wkeeling/selenium-wire/raw/master/seleniumwire/ca.crt](https://github.com/wkeeling/selenium-wire/raw/master/seleniumwire/ca.crt)

将该 ca.crt 添加到受信任的证书中

google浏览器打开```chrome://settings/security```

点击管理证书，选择```受信任的的根证书颁发机构```分区，选择导入

导入后再运行seleniumwire开启浏览器则不会出现Not Secure的问题了


