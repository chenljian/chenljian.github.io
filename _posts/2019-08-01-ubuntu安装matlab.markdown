---
layout:     post
title:      "Ubuntu 14.04安装Matlab 2015b"
subtitle:   "破解版Matlab，提供下载"
date:       2019-08-01 13:00:01
author:     "Chen"
header-img: "img/post-default-header.jpg"
catalog: true
tags:
    - Linux
---

> “That's the beauty of music. They can't take that away from you. ”


<p id = "build"></p>
## 安装包下载

链接: https://pan.baidu.com/s/1PwuiV8J6THyl07vxg9K-jw 提取码: h2th 
里面主要是Matlab R2015b 和 crack，如图

![](/img/in-post/post-eleme-pwa/Architecture.png)

## 安装步骤

挂载ISO镜像文件

```
sudo mkdir /media/matlab
sudo mount -o loop R2015b_glnxa64.iso /media/matlab
```

开始安装

```
cd /media/matlab
sudo ./install
```

选择不联网安装

![](/img/in-post/post-eleme-pwa/Lighthouse-after.png)

序列号在crack的readme.txt中

![](/img/in-post/post-eleme-pwa/Lighthouse-before.png)

选择安装路径

![](/img/in-post/post-eleme-pwa/msite-After-Optim.png)

激活MATLAB

安装完成之后需要激活，首先将一些.so文件复制到安装目录的/bin/glnxa64中

```
sudo cp [Your crack directory]/R2015b/bin/glnxa64/* /usr/local/MATLAB/R2015b/bin/glnxa64
```

然后root权限首次运行matlab

```
cd /usr/local/MATLAB/R2015b/bin
sudo ./matlab
```

选择不联网手动激活

![](/img/in-post/post-eleme-pwa/msite-Before-Optim.png)

导入license文件，选择Crack目录下的license_standalone.lic

![](/img/in-post/post-eleme-pwa/nextTick-&-Load.png)

接着卸载ISO镜像，镜像也删了

```
 sudo umount /media/matlab
```

然后就可以开始用MATLAB了

------

## 卸载

```
sudo rm -rf /usr/local/MATLAB/R2015b
sudo rm /usr/local/bin/matlab /usr/local/bin/mcc /usr/local/bin/mex /usr/local/bin/mbuild
```

仅供参考，有些文件可能不存在

------

参考文献：https://blog.csdn.net/hejunqing14/article/details/50265049 (里面还有快捷方式的教程)
图片来源：https://blog.csdn.net/Jesse_Mx/article/details/53956358 (图片上的matlab是2016，借用其图片是因为自己安装的时候没有截图)

—— Chen


