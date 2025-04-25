# 解决selenium爬虫加密代理问题(含socks5等一切代理)


## 前言

爬虫过程中出现了cloudflare的高风险质询验证阻拦或者recaptcha的阻拦，想要破除验证要么使用干净的代理保证低风险爬虫，要么使用hcaptcha求解器求解，后者需要部署的东西过于复杂，为了提高效率，本文介绍第一种方法破除质询验证

cloudflare的风险得分查询(详见我项目的对应函数)

[https://github.com/spiritLHLS/ecs/blob/main/qzcheck.py#L32](https://github.com/spiritLHLS/ecs/blob/main/qzcheck.py#L32)


Recaptcha的风险得分查询

[https://antcpt.com/score_detector/](https://antcpt.com/score_detector/)

## 四种解决方法

仅以chromedriver做例子，至于geckodriver，edgedriver可以类比使用

### 原生支持解决

chromedriver的有头模式下，使用透明代理(无用户和密码)

```python
def input_dependence():
    opt = ChromeOptions()
    opt.headless = False
    opt.add_argument('--proxy-server=socks5://ip:port')
    driver = Chrome(executable_path=r"./chromedriver", options=opt)
    return driver
```

chromedriver的有头模式下，使用透明代理(有用户和密码，带身份验证)

自定义插件，加载插件

```python
def create_proxy_auth_extension(proxy_host, proxy_port,
                                proxy_username, proxy_password,
                                scheme='http', plugin_path=None):
    if plugin_path is None:
        plugin_path = r'./{}.zip'.format(str(proxy_host).replace(".", ""))
    manifest_json = """
        {
            "version": "1.0.0",
            "manifest_version": 2,
            "name": "Chrome Proxy",
            "permissions": [
                "proxy",
                "tabs",
                "unlimitedStorage",
                "storage",
                "",
                "webRequest",
                "webRequestBlocking"
            ],
            "background": {
                "scripts": ["background.js"]
            },
            "minimum_chrome_version":"22.0.0"
        }
        """
    background_js = string.Template(
        """
        var config = {
            mode: "fixed_servers",
            rules: {
                singleProxy: {
                    scheme: "${scheme}",
                    host: "${host}",
                    port: parseInt(${port})
                },
                bypassList: ["foobar.com"]
            }
          };
        chrome.proxy.settings.set({value: config, scope: "regular"}, function() {});
        function callbackFn(details) {
            return {
                authCredentials: {
                    username: "${username}",
                    password: "${password}"
                }
            };
        }
        chrome.webRequest.onAuthRequired.addListener(
            callbackFn,
            {urls: [""]},
            ['blocking']
        );
        """
    ).substitute(
        host=proxy_host,
        port=proxy_port,
        username=proxy_username,
        password=proxy_password,
        scheme=scheme,
    )
    with zipfile.ZipFile(plugin_path, 'w') as zp:
        zp.writestr("manifest.json", manifest_json)
        zp.writestr("background.js", background_js)
    return plugin_path

# 传入的i为socks5://user:password@ip:port
def input_dependence(i):
    global driver, proxy_auth_plugin_path, status_p 
    # 启动浏览器内核
    opt = ChromeOptions()
    opt.headless = False
    ipp = str(i)
    if "@" in ipp:
        ip = ipp.split("@")[1].split(":")[0]
        port = ipp.split("@")[1].split(":")[1]
        username = ipp.split("@")[0].split(":")[0]
        passwd = ipp.split("@")[0].split(":")[1]
        proxy_auth_plugin_path = create_proxy_auth_extension(
            proxy_host=ip,
            proxy_port=port,
            proxy_username=username,
            proxy_password=passwd)
        # 如报错 chrome-extensions
        #opt.add_argument("--disable-extensions")
        opt.add_extension(proxy_auth_plugin_path)
    driver = Chrome(executable_path=r"./chromedriver", options=opt)
```

以上加载插件的方式是需要有头模式下运行，linux下使用xvfb模拟有头环境使用

```
xvfb-run python3 test.py
```

### 使用中间件

本地架设通道转发有身份验证的代理为无身份验证的代理

推荐项目：

[https://github.com/qwj/python-proxy](https://github.com/qwj/python-proxy)

```bash
pip install pproxy
```

[https://github.com/snail007/goproxy](https://github.com/snail007/goproxy)

使用中间件在本地架设代理转发，需要注意本地使用```127.0.0.1```而服务器使用```0.0.0.0```，且保证本机的端口是开放的

另一个优点是代理不再局限于socks5一种代理方式，涵盖vmess，ss，trojan等等你只要找得到的对应中间件，那么代理都能使用

当然，这种代理也保证了有头与无头模式下皆可使用，无需使用xvfb之类的工具

转发代理至于本地:8080后，可使用

```python
def input_dependence():
    opt = ChromeOptions()
    opt.headless = False
    opt.add_argument('--proxy-server=socks5://127.0.0.1:8080')
    driver = Chrome(executable_path=r"./chromedriver", options=opt)
    return driver
```

简单的说代理的范围取决于使用了什么中间件，只要中间件支持协议，那么代理就能用

### 使用selenium的扩展模块

项目：

[https://github.com/wkeeling/selenium-wire](https://github.com/wkeeling/selenium-wire)

```bash
pip install selenium-wire
```

使用这种方法可以在无头和有头模式下使用代理，且无需本地架设中间件程序，只需要替换原有的selenium为seleniumwire即可

缺点是只能使用socks5的有身份验证的代理，无法支持更多代理方式

```python
from seleniumwire import webdriver

options = {
   'proxy': {
        'http': 'socks5://user:password@ip:port',
        'https': 'socks5://user:password@ip:port',
        'no_proxy': 'localhost,127.0.0.1'
    }
}
driver = webdriver.Chrome(seleniumwire_options=options)
```

上述方法即可简单的使用含用户名和密码的s5代理了

值得注意的是seleniumwire只是修补了webdriver下的包，其他selenium包以及webdriver的子包仍然使用selenium导入

```python
# Sub-packages of webdriver must still be imported from `selenium` itself
from selenium.webdriver.support.ui import WebDriverWait
```

```python
from seleniumwire.webdriver import ActionChains
```

替换

```python
from selenium.webdriver import ActionChains
```

区别就在这里

### 使用全局代理

非常方便的傻瓜式方法，缺点是需要保证代理使用过程中，python程序可以正常运行

## 后言

使用代理过程中可能会出现各种个月的bug以及爆错，还是建议多看看仓库说明仓库的issues，实在不行上google查询，一般都能解决
