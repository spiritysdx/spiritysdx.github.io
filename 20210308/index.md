# C++函数分文件编写(VScode2021配置教程)


# vscode2021新版的C++函数分文件编写配置

如图所示，需要下载这个插件


```
C/C++ Project Generator
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210308091652596.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NwaXJpdExITA==,size_16,color_FFFFFF,t_70#pic_center)
安装并启用后，使用方法如下
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210308091718789.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NwaXJpdExITA==,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210308091732634.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NwaXJpdExITA==,size_16,color_FFFFFF,t_70#pic_center)
创建工程文件后目录结构如下

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210308091749652.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NwaXJpdExITA==,size_16,color_FFFFFF,t_70#pic_center)
main.cpp文件使用自定义的swap函数，swap函数的头文件放include文件夹，源文件放src文件夹

下面是运行c++程序生成可执行exe文件的步骤，如图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210308091819482.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NwaXJpdExITA==,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210308091827806.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NwaXJpdExITA==,size_16,color_FFFFFF,t_70#pic_center)
在终端运行，得到结果

## 注意，千万不要用右上角的run code执行！
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210308091905135.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NwaXJpdExITA==,size_16,color_FFFFFF,t_70#pic_center)

↓↓↓我在我的博客首发这篇文章，欢迎在我的自建博客查看

[我的博客文章](https://www.spiritlhl.top/blog/c++%E5%87%BD%E6%95%B0%E5%88%86%E6%96%87%E4%BB%B6%E7%BC%96%E5%86%99/)

如果运行后显示中文乱码可以试试下面的代码

```cpp
#include<iostream>
#include <windows.h>//用于函数SetConsoleOutputCP(65001);更改cmd编码为utf8
using namespace std;

int main()
{
	SetConsoleOutputCP(65001);
	cout<<"中文正常显示"<<endl;
	system("pause");
    return 0;
}
```



