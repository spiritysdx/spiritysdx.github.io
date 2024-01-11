# 深度学习环境安装(李沐老师相关)


由于之前有写过一键安装jupyter的shell脚本，所以这里只需要找一个服务器就够了

https://github.com/spiritLHLS/one-click-installation-script#%E4%B8%80%E9%94%AE%E5%AE%89%E8%A3%85jupyter%E7%8E%AF%E5%A2%83

```bash
curl -L https://raw.githubusercontent.com/spiritLHLS/one-click-installation-script/main/install_scripts/jupyter.sh -o jupyter.sh && chmod +x jupyter.sh && bash jupyter.sh
```

又由于之前玩Linux积攒了很多机器，找了一台[腾讯云](https://curl.qcloud.com/MNyiQAB1)广州的4C3C80G和[OVH](https://hosting.skrime.eu/a/server)法国的4C2C25G的机器测试了一下

发现d2l的安装包所需内存比较大，起步需要至少4G内存才保险，所以这里又分别给各机器开了4G(输入1后输入4096)的SWAP

```bash
curl -L https://raw.githubusercontent.com/spiritLHLS/addswap/main/addswap.sh -o addswap.sh && chmod +x addswap.sh && bash addswap.sh
```

开完SWAP后一键安装完毕jupyter，需要配置相关环境(暂时我只使用CPU计算)

装pytorch的时候，可能```solve environment```这一步可能需要转圈圈跑大概15~20分钟，因为要处理依赖问题，装d2l也大概需要10分钟左右，至于安装过程很快的(网络带宽足够大的话)

```bash
conda install -c conda-forge nodejs
conda install -c conda-forge jupyterlab_rise
conda install pytorch==1.12.0 torchvision==0.13.0 torchaudio==0.12.0 cpuonly -c pytorch
conda install -c conda-forge d2l
conda install matplotlib-inline
```

不用cpu的可在 [https://pytorch.org/](https://pytorch.org/) 中查看对应平台的安装命令

比如对应 [https://zh.d2l.ai/chapter_installation/index.html#d2l](https://zh.d2l.ai/chapter_installation/index.html#d2l) 的是 [https://pytorch.org/get-started/previous-versions/#v1120](https://pytorch.org/get-started/previous-versions/#v1120)

由于执行官方安装程序可能会缺失matplotlib-inline包，如

```python
# 执行
%matplotlib inline
import torch
from torch.distributions import multinomial
from d2l import torch as d2l
# 报错说
# ---------------------------------------------------------------------------
# ModuleNotFoundError                       Traceback (most recent call last)
# <ipython-input-3-8a74a3716418> in <module>
#       2 import torch
#       3 from torch.distributions import multinomial
# ----> 4 from d2l import torch as d2l

# ~/miniconda3/envs/jupyter-env/lib/python3.8/site-packages/d2l/torch.py in <module>
#      34 from IPython import display
#      35 from matplotlib import pyplot as plt
# ---> 36 from matplotlib_inline import backend_inline
#      37 
#      38 d2l = sys.modules[__name__]

# ModuleNotFoundError: No module named 'matplotlib_inline'
```

所以安装过程中需要执行```conda install matplotlib-inline```

如果运行过程中缺什么包的话查找 [https://anaconda.org/](https://anaconda.org/) 可得安装命令

系统模板还得是ubuntu20用的顺手，因为腾讯云的debian11和ubuntu22老有网络问题，懒得去搞镜像或者CDN加速了，都是国情因素导致的，带宽又小又容易被阻断，因为法国的测试都是没问题的，带宽大下东西就是爽，所以懒得管啦，还是用国外服务器得了

```
mkdir d2l-zh && cd d2l-zh
curl https://zh-v2.d2l.ai/d2l-zh-2.0.0.zip -o d2l-zh.zip
unzip d2l-zh.zip && rm d2l-zh.zip
cd pytorch
```

相关的jupyternotebook的文件压缩包如上，实测还得是下到本地才传得上腾讯云，当然OVH的直接用没问题，原因大概是国情因素导致的TLS阻断，源站属于Github的pages服务搭建的所以被阻断了，又加上国内带宽口子贼小，还是算了吧，最终用法国的服务器好点，口子大还没各种网络问题


