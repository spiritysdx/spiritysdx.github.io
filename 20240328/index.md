# Golang的重定向比Python的重定向次数要多


## 前言

之前有使用 FastAPI 开发过一套后端，但占用属实有点大，最近使用 Gin-Vue-Admin 进行重构，在构建爬虫模块的时候，发现了一些有趣的事情，Python的重定向比Golang的重定向次数要少

## 复现

```golang
package main

import (
  "fmt"
  "net/http"
)

func main() {
  url := "https://my.speedypage.com/aff.php?aff=4&pid=58"

  // 创建一个 HTTP 客户端
  client := &http.Client{
    CheckRedirect: func(req *http.Request, via []*http.Request) error {
      // 打印重定向信息
      fmt.Printf("Redirected to: %s, Status Code: %d\n", req.URL, req.Response.StatusCode)
      return nil
    },
  }

  // 发送 GET 请求
  response, err := client.Get(url)
  if err != nil {
    fmt.Println("Error:", err)
    return
  }
  defer response.Body.Close()

  // 打印最终的 URL 和状态码
  fmt.Println("Final URL:", response.Request.URL)
  fmt.Printf("Final Status Code: %d\n", response.StatusCode)
}
```

输出为

```
Redirected to: https://my.speedypage.com/cart.php?a=add&pid=58, Status Code: 302
Redirected to: https://my.speedypage.com/store/sg-vps/sg-kvm-1g, Status Code: 302
Redirected to: https://my.speedypage.com/cart.php?a=confproduct&i=0, Status Code: 302
Redirected to: https://my.speedypage.com/cart.php, Status Code: 302
Redirected to: https://my.speedypage.com/store/web-hosting, Status Code: 302
Final URL: https://my.speedypage.com/store/web-hosting
Final Status Code: 200
```

于此同时

```python
import requests


def main():
  url = "https://my.speedypage.com/aff.php?aff=4&pid=58"
  redirect_count = 0
  response = requests.get(url, allow_redirects=True)
  print("Final URL:", response.url)
  for redirect in response.history:
    redirect_count += 1
    print(
        f"Redirected to: {redirect.url}, Status Code: {redirect.status_code}")
  print(
      f"Total Redirects: {redirect_count}, Final Status Code: {response.status_code}"
  )


if __name__ == "__main__":
  main()
```

输出为

```
Final URL: https://my.speedypage.com/cart.php?a=confproduct&i=0
Redirected to: https://my.speedypage.com/aff.php?aff=4&pid=58, Status Code: 302
Redirected to: https://my.speedypage.com/cart.php?a=add&pid=58, Status Code: 302
Redirected to: https://my.speedypage.com/store/sg-vps/sg-kvm-1g, Status Code: 302
Total Redirects: 3, Final Status Code: 200
```

发现了没有，这里的302重定向，在python中是3次，在golang中是5次，最后的200状态码的最终地址居然是不一样的！

## 原因

查询了相关资料，没有进行实际的摸索，但我猜测是重定向过程中，某些参数设置失效或不同造成的，极大可能和

<https://stackoverflow.com/questions/76095713/a-request-made-in-go-lang-gets-blocked-when-python-requests-and-curl-works>

是同样或类似的原因，默认很多参数配置同Python的不一致(非cookie)，导致302重定向没有在应该在的目标地址返回200状态码。

## 修复方法

不要使用默认的http包构建请求，使用第三方封装的gorequests就能解决这个问题，虽然问题原因不清楚，但是解决了（

```golang
package main

import (
  "fmt"
  "github.com/parnurzeal/gorequest"
)

func main() {
 url := "https://my.speedypage.com/aff.php?aff=4&pid=58"
 request := gorequest.New()
 resp, body, err := request.Get(url).End()
 if err != nil {
  fmt.Println("Error creating request:", err)
  return
 }
 fmt.Println("Final URL:", resp.Request.URL)
 fmt.Println("Final Status Code:", resp.StatusCode)
 fmt.Println("Response Body:", string(body))
}
```

## 后言

后续有空也许会追加对这方面的摸索，看看具体是啥原因造成的

