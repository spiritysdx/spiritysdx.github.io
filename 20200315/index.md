# 安装python第三方库的小技巧

### <font color=cyan>直接放代码</font>

```python
import os
libs = {
    "requests","jieba","beautifulsoup4",\
    "django","flask",\
    "此处填写你需要下载的库的名称，注意大小写并拼写正确，样式如上面例子","pandas"
}
try:
    for lib in libs:
        os.system('pip install '+lib)
        print("Successful")
except:
    print('error')
```
os.system(command)

command 为要执行的命令，近似于Windows下cmd窗口中输入的命令。
