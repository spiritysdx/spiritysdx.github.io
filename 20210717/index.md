# 直接通过jupyternotebook安装库


## 我的前提条件，实际能使用jupyternotebook都可

```Ubuntu 20.0```系统

```miniconda3```和```jupyter```

## 直接通过jupyter notebook安装库

代码：

查找解释器位置

```python
import os
os.sys.executable
```

示例结果：

```
'/usr/bin/python3'
```

安装示例模板

```python
import os
libs = {
    "requests","jieba","beautifulsoup4","matplotlib","numpy","pandas","openpyxl","tensorflow","仿照格式这里填写要安装的包"
}
try:
    for lib in libs:
        #这里的地址是上一步显示的解释器位置
        os.system('/usr/bin/python3 -m pip install '+lib)
        print("Successful")
except:
    print('error')
```


