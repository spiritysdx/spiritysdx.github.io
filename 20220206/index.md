# 图片验证码增强技术(提高识别正确率)


### 直接使用原始图片做验证码识别正确率较低，使用增强技术后能大大提高识别率


黑底填充白底

```python
from PIL import Image
import os


def Convert(filename):
    """
    将图像中白色像素转变为黑色像素
    """
    img = Image.open(os.getcwd()+ "\\"+ filename)
    img = img.convert("RGBA")
    pixdata = img.load()
    for y in range(img.size[1]):
        for x in range(img.size[0]):
            if all(pixdata[x, y][i] > 220 for i in range(4)):
                pixdata[x, y] = 0, 0, 0
    try:
        os.remove("result.png")
    except Exception as e:
        print(e)
    # result.png 黑色填充白底彩色图
    img.save("result.png")
    print("Successfully: " + filename)


if __name__ == "__main__":
    # origin.png 彩色验证码图片
    Convert(r"origin.png")
```

彩色变黑白二值化增强

```python
from PIL import Image
from PIL import ImageEnhance


##增强图形识别率的处理
# origin.png 彩色验证码图片
i2=Image.open(r"origin.png")
imgry = i2.convert('L')   #图像加强，二值化，PIL中有九种不同模式。分别为1，L，P，RGB，RGBA，CMYK，YCbCr，I，F。L为灰度图像
sharpness =ImageEnhance.Contrast(imgry)#对比度增强
i3 = sharpness.enhance(3.0)  #3.0为图像的饱和度
try:
    os.remove("result.png")
except Exception as e:
    print(e)
# result.png 黑白灰度图
i3.save("result.png")
```
