# 自建青龙代理拉取Github仓库或文件

# 前言

因为青龙拉取部分作者仓库通过```https://ghproxy.com/```代理拉取频繁，导致了请求量比较大的仓库被该代理加速作者封禁。所以可以自建代理的链接替换默认的```https://github.com/```，自己用自己的反代不会封禁，自建代理实现```ql repo```或```ql raw```拉取实时的GitHub仓库或文件。

# 步骤

### 1.注册cloudflare账号一枚

注册链接：[https://dash.cloudflare.com/sign-up](https://dash.cloudflare.com/sign-up)

注册不需要特殊条件，只是访问缓慢，当然魔法后更快打开页面就是了。

邮箱也没有要求，我是163邮箱注册。

注册完密码后点击对应邮箱的信件验证账号。

### 2.创建worker

第四个选项是Workers

![](https://cdn.jsdelivr.net/gh/spiritLHL/tuchuang@master/p1.png)

点击创建

![](https://cdn.jsdelivr.net/gh/spiritLHL/tuchuang@master/p2.png)

改动默认前缀，方便自己记住

![](https://cdn.jsdelivr.net/gh/spiritLHL/tuchuang@master/p3.jpg)

记住改后的网址，这个打码部分记住备用

![](https://cdn.jsdelivr.net/gh/spiritLHL/tuchuang@master/p4.png)

创建后需要更改内容，点击更改

![](https://cdn.jsdelivr.net/gh/spiritLHL/tuchuang@master/p5.png)

记得删除原有的更改

![](https://cdn.jsdelivr.net/gh/spiritLHL/tuchuang@master/p6.png)

你需要粘贴的代码如下，将该代码复制粘贴覆盖原有的代码

```
'use strict'

/**
 * static files (404.html, sw.js, conf.js)
 */
const ASSET_URL = 'https://github.com'

const JS_VER = 10
const MAX_RETRY = 1

/** @type {RequestInit} */
const PREFLIGHT_INIT = {
  status: 204,
  headers: new Headers({
    'access-control-allow-origin': '*',
    'access-control-allow-methods': 'GET,POST,PUT,PATCH,TRACE,DELETE,HEAD,OPTIONS',
    'access-control-max-age': '1728000',
  }),
}

/**
 * @param {any} body
 * @param {number} status
 * @param {Object<string, string>} headers
 */
function makeRes(body, status = 200, headers = {}) {
  headers['--ver'] = JS_VER
  headers['access-control-allow-origin'] = '*'
  return new Response(body, {status, headers})
}


/**
 * @param {string} urlStr 
 */
function newUrl(urlStr) {
  try {
    return new URL(urlStr)
  } catch (err) {
    return null
  }
}


addEventListener('fetch', e => {
  const ret = fetchHandler(e)
    .catch(err => makeRes('cfworker error:\n' + err.stack, 502))
  e.respondWith(ret)
})


/**
 * @param {FetchEvent} e 
 */
async function fetchHandler(e) {
  const req = e.request
  const urlStr = req.url
  const urlObj = new URL(urlStr)
  const path = urlObj.href.substr(urlObj.origin.length)

  if (urlObj.protocol === 'http:') {
    urlObj.protocol = 'https:'
    return makeRes('', 301, {
      'strict-transport-security': 'max-age=99999999; includeSubDomains; preload',
      'location': urlObj.href,
    })
  }

  if (path.startsWith('/http/')) {
    return httpHandler(req, path.substr(6))
  }

  switch (path) {
  case '/http':
    return makeRes('请更新 cfworker 到最新版本!')
  case '/ws':
    return makeRes('not support', 400)
  case '/works':
    return makeRes('it works')
  default:
    // static files
    return fetch(ASSET_URL + path)
  }
}


/**
 * @param {Request} req
 * @param {string} pathname
 */
function httpHandler(req, pathname) {
  const reqHdrRaw = req.headers
  if (reqHdrRaw.has('x-jsproxy')) {
    return Response.error()
  }

  // preflight
  if (req.method === 'OPTIONS' &&
      reqHdrRaw.has('access-control-request-headers')
  ) {
    return new Response(null, PREFLIGHT_INIT)
  }

  let acehOld = false
  let rawSvr = ''
  let rawLen = ''
  let rawEtag = ''

  const reqHdrNew = new Headers(reqHdrRaw)
  reqHdrNew.set('x-jsproxy', '1')

  // 此处逻辑和 http-dec-req-hdr.lua 大致相同
  // https://github.com/EtherDream/jsproxy/blob/master/lua/http-dec-req-hdr.lua
  const refer = reqHdrNew.get('referer')
  const query = refer.substr(refer.indexOf('?') + 1)
  if (!query) {
    return makeRes('missing params', 403)
  }
  const param = new URLSearchParams(query)

  for (const [k, v] of Object.entries(param)) {
    if (k.substr(0, 2) === '--') {
      // 系统信息
      switch (k.substr(2)) {
      case 'aceh':
        acehOld = true
        break
      case 'raw-info':
        [rawSvr, rawLen, rawEtag] = v.split('|')
        break
      }
    } else {
      // 还原 HTTP 请求头
      if (v) {
        reqHdrNew.set(k, v)
      } else {
        reqHdrNew.delete(k)
      }
    }
  }
  if (!param.has('referer')) {
    reqHdrNew.delete('referer')
  }

  // cfworker 会把路径中的 `//` 合并成 `/`
  const urlStr = pathname.replace(/^(https?):\/+/, '$1://')
  const urlObj = newUrl(urlStr)
  if (!urlObj) {
    return makeRes('invalid proxy url: ' + urlStr, 403)
  }

  /** @type {RequestInit} */
  const reqInit = {
    method: req.method,
    headers: reqHdrNew,
    redirect: 'manual',
  }
  if (req.method === 'POST') {
    reqInit.body = req.body
  }
  return proxy(urlObj, reqInit, acehOld, rawLen, 0)
}


/**
 * 
 * @param {URL} urlObj 
 * @param {RequestInit} reqInit 
 * @param {number} retryTimes 
 */
async function proxy(urlObj, reqInit, acehOld, rawLen, retryTimes) {
  const res = await fetch(urlObj.href, reqInit)
  const resHdrOld = res.headers
  const resHdrNew = new Headers(resHdrOld)

  let expose = '*'
  
  for (const [k, v] of resHdrOld.entries()) {
    if (k === 'access-control-allow-origin' ||
        k === 'access-control-expose-headers' ||
        k === 'location' ||
        k === 'set-cookie'
    ) {
      const x = '--' + k
      resHdrNew.set(x, v)
      if (acehOld) {
        expose = expose + ',' + x
      }
      resHdrNew.delete(k)
    }
    else if (acehOld &&
      k !== 'cache-control' &&
      k !== 'content-language' &&
      k !== 'content-type' &&
      k !== 'expires' &&
      k !== 'last-modified' &&
      k !== 'pragma'
    ) {
      expose = expose + ',' + k
    }
  }

  if (acehOld) {
    expose = expose + ',--s'
    resHdrNew.set('--t', '1')
  }

  // verify
  if (rawLen) {
    const newLen = resHdrOld.get('content-length') || ''
    const badLen = (rawLen !== newLen)

    if (badLen) {
      if (retryTimes < MAX_RETRY) {
        urlObj = await parseYtVideoRedir(urlObj, newLen, res)
        if (urlObj) {
          return proxy(urlObj, reqInit, acehOld, rawLen, retryTimes + 1)
        }
      }
      return makeRes(res.body, 400, {
        '--error': `bad len: ${newLen}, except: ${rawLen}`,
        'access-control-expose-headers': '--error',
      })
    }

    if (retryTimes > 1) {
      resHdrNew.set('--retry', retryTimes)
    }
  }

  let status = res.status

  resHdrNew.set('access-control-expose-headers', expose)
  resHdrNew.set('access-control-allow-origin', '*')
  resHdrNew.set('--s', status)
  resHdrNew.set('--ver', JS_VER)

  resHdrNew.delete('content-security-policy')
  resHdrNew.delete('content-security-policy-report-only')
  resHdrNew.delete('clear-site-data')

  if (status === 301 ||
      status === 302 ||
      status === 303 ||
      status === 307 ||
      status === 308
  ) {
    status = status + 10
  }

  return new Response(res.body, {
    status,
    headers: resHdrNew,
  })
}


/**
 * @param {URL} urlObj 
 */
function isYtUrl(urlObj) {
  return (
    urlObj.host.endsWith('.googlevideo.com') &&
    urlObj.pathname.startsWith('/videoplayback')
  )
}

/**
 * @param {URL} urlObj 
 * @param {number} newLen 
 * @param {Response} res 
 */
async function parseYtVideoRedir(urlObj, newLen, res) {
  if (newLen > 2000) {
    return null
  }
  if (!isYtUrl(urlObj)) {
    return null
  }
  try {
    const data = await res.text()
    urlObj = new URL(data)
  } catch (err) {
    return null
  }
  if (!isYtUrl(urlObj)) {
    return null
  }
  return urlObj
}
```


测试请求是否反代成功

![](https://cdn.jsdelivr.net/gh/spiritLHL/tuchuang@master/p7.png)

反代显示这个显示成功

![](https://cdn.jsdelivr.net/gh/spiritLHL/tuchuang@master/p8.png)

记得点击部署，否则不会生效

![](https://cdn.jsdelivr.net/gh/spiritLHL/tuchuang@master/p9.png)

保存并部署

![](https://cdn.jsdelivr.net/gh/spiritLHL/tuchuang@master/p10.png)

部署完毕就可以关掉页面了，记住之前的网址即可。

## 使用方法

很简单

刚刚你记住的网址相当于```github.com```，只需要替换掉即可。

如果拉取失败，不考虑别的，打开你刚刚记住的网址，在浏览器中打开，查找作者仓库，找作者仓库的raw链接或仓库文件下载链接，如图。

这个是单文件的链接，你右键选则复制链接即可。↓↓↓

![](https://cdn.jsdelivr.net/gh/spiritLHL/tuchuang@master/p11.png)

这个是仓库的拉取链接，反代的可能打开缓慢加载，可以先去Github找这个链接，再替换其中的```github.com```即可。

也是右键右键选则复制链接即可，再替换。↓↓↓

![](https://cdn.jsdelivr.net/gh/spiritLHL/tuchuang@master/p12.png)

优点是相当于反代直连，仓库有任何改动你拉取的都不是缓存的内容，更新及时。

每天100,000(10万)次免费请求额度，根本用不完。

实际上你已经搭了一个反代github网站，你点击那个网址打开的网站就是github，除了不能登陆以外，其他所有操作和你魔法后访问GitHub一样，又快又好。

我写几个范例你们就明白了

### 范例

假设你刚刚记住的链接是这个：

"github.yourname.workers.dev"

那么拉取仓库从

```
ql repo https://github.com/spiritLHL/qinglong_auto_tools.git "scripts_"
```

或

```
ql repo https://ghproxy.com/https://github.com/spiritLHL/qinglong_auto_tools.git "scripts_"
```

变为

```
ql repo https://github.yourname.workers.dev/spiritLHL/qinglong_auto_tools.git "scripts_"
```

**拉取单文件不大一样，不能直接替换```raw.githubusercontent.com```，还需要在作者名字后加上```/raw/```，小白建议打开你那个```github.yourname.workers.dev```搜索查找脚本，点击raw按钮找到反代raw的链接**

拉取单文件从

```
ql raw https://raw.githubusercontent.com/spiritLHL/qinglong_auto_tools/master/scripts_restore_env.py
```

或

```
ql raw https://ghproxy.com/https://raw.githubusercontent.com/spiritLHL/qinglong_auto_tools/master/scripts_restore_env.py
```

变为

```
ql raw https://github.yourname.workers.dev/spiritLHL/qinglong_auto_tools/raw/master/scripts_rearblack.py
```
ps:实际就是替换后在仓库名字后面加上```/raw/```，不打开查找直接替换后加上也行。

pps:有些作者单文件拉取链接里有```/blob/```，得改成```/raw/```。

# 后言

仓库:[https://github.com/spiritLHL/qinglong_auto_tools](https://github.com/spiritLHL/qinglong_auto_tools)

频道：[https://t.me/qinglong_auto_tools](https://t.me/qinglong_auto_tools)

群组：[https://t.me/qinglong_auto_tools_chat](https://t.me/qinglong_auto_tools_chat)

