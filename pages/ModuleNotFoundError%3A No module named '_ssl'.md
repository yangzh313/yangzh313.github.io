title:: ModuleNotFoundError: No module named '_ssl'

- > 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/qq_39715000/article/details/125009276)
  
  当装好 python3 导入 [ssl](https://so.csdn.net/so/search?q=ssl&spm=1001.2101.3001.7020) 模块时报以下错误： ModuleNotFoundError: No module named '_ssl'
  =======================================================================================================================================
  
  > import _ssl # if we can't import it, let the error propagate ImportError: No module named _ssl
  
  ![](https://img-blog.csdnimg.cn/c9a7202a96c94b0a9a8b88af0d33c9ac.png)
  
  参考试了好多文章，发现都不太好使，最终找到了解决方案，安装 [openssl](https://so.csdn.net/so/search?q=openssl&spm=1001.2101.3001.7020) 的依赖
  
  **解决办法：**
  =========
  
  **centos 安装 python3.7 时，查阅知需要的 openssl 版本最低为 1.0.2，但是 centos 默认的为 1.0.1，所以需要重新更新 openssl**
  
  一、安装依赖库：
  ========
  
  如果有网，直接在线安装依赖库，如果部署环境没网，可以参考最后一节，五、注 1：openssl 或 [nginx](https://so.csdn.net/so/search?q=nginx&spm=1001.2101.3001.7020) 离线依赖安装过程
  
  > yum install -y zlib zlib-dev openssl-devel sqlite-devel bzip2-devel libffi libffi-devel gcc gcc-c++
  
  二、安装新版本的 openssl
  ================
  
  注意！openssl 配置是用 config，而不是 configure，另外 openssl 编译安装依赖 zlib 动态库，所以一定要 shared zlib 自行到官网查阅最新版本~
  
  1、openssl 安装包官网下载地址：
  --------------------
  
  http://www.openssl.org/source/
  
  也可在该文章最后面一节内容，注 1：步骤中所述下载到 openssl1.0.2 版本，可以满足 Centos7.8 环境。
  
  > **wget http://www.openssl.org/source/openssl-1.1.1.tar.gz **           （如果有网就可以直接下载，没网就需要单独下载安装包通过终端工具从 windows 上传到 centos 中）
  > 
  > **tar -zxvf openssl-1.0.2k.tar.gz **                                               （ 解压对应版本安装包）
  > 
  > **cd openssl-1.0.2k   **                                                             （进入对应的解压目录）
  > 
  > **./config --prefix=/usr/local/openssl shared zlib** 
  > 
  >  (prefix 为配置安装目录，shared zlib 安装依赖库）这一步最重要，一定要 shared
  > 
  > **make && make install**                                                （编译、安装）
  
  2、设置环境变量 LD_LIBRARY_PATH（一般用户环境变量即可生效，二选一即可）
  --------------------------------------------
  
          这一步一定要有！！LD_LIBRARY_PATH 环境变量主要用于指定查找共享库（动态链接库）时除了默认路径之外的其他路径。当执行函数动态链接. so 时，如果此文件不在缺省目录下‘/lib'and ‘/usr/lib'，那么就需要指定环境变量 LD_LIBRARY_PATH
### （1）用户环境变量：

> **echo "export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/openssl/lib" >> /usr/local/.bash_profile**     (在用户环境变量文件中添加 openssl 的环境变量)
> 
> **source /usr/local/.bash_profile   **                     （重新加载该路径下的用户环境变量文件）

**注：在实际服务器上应用过程中发现，该环境变量只会当前 shell 脚本生效，如果下次重新进入 shell 脚本会失效，故如果嫌麻烦每次启动服务都要用的话，可直接设置到 path 环境变量中。**（该步涉及到 Centos 系统使用习惯，一般程序环境变量设置入用户变量即可，path 专门保存系统变量，但一般用户环境变量会失效，放入 path 会全局生效）。
### （2）系统环境变量：

如果选择将其设置为全局的在 path 系统环境变量中设置命令如下：

> **echo "export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/openssl/lib" >>  /etc/profile**    
> 
> (在系统环境变量文件中添加 openssl 的环境变量)
> 
> **source /etc/profile**                                 （重新加载该路径下的系统环境变量文件）

三、解压 python3.7，并安装，一定要指定刚才安装的 1.1.1 版本的 openssl！！！
==================================================

> **tar -zxvf Python-3.7.0.tgz                                                                        (解压安装包)**
> 
> **./configure --prefix=/usr/local//python3 --with-openssl=/usr/local//openssl** 
> 
>  **(配置安装目录，指定 openssl 环境的安装目录)**
> 
> **make && make install                                                                                （编译、安装）**

四、至此 python3.7 就安装完了，来检验下 ssl 模块能否被导入吧：
=======================================

1、创建 python 软连接
---------------

> > **ln -s /usr/local/python3/bin/pip3  /usr/bin/pip**     
> > 
> >   (pip 命令行调用指向 pip3)
> > 
> > **ln -s /usr/local/python3/bin/python3  /usr/bin/python**           
> > 
> >   (python 命令行调用指向 python3, 如果同时安装有其他版本 python 可通过该命令指定用到的 python 版本)
> > 
> > **ln -s /usr/local/python3/bin/python3  /usr/bin/python3**     
> > 
> >   (python3 命令行调用指向 python3, 由于某些程序执行时要求是 python3 命令调用，该步可根据需要进行设置)
> > 
> > **python --version**                                                
> > 
> >  查看当前 python 安装版 

![](https://img-blog.csdnimg.cn/6cdbf45113294f17a7dafcf65eccb0a2.png)

 2、导入 ssl 模块
------------

> python3                      （进入 python3 命令行）
> ---------------------------------------------
> 
> import ssl                        （导入 ssl 模块）
> 
> import _ssl                        （导入_ssl 模块）
> 
> exit()                                        （退出 python3 命令行）

![](https://img-blog.csdnimg.cn/0c9f8d49fd594b8fb3e00a3376112563.png)

3、查看 pyhon 的软链接指向。
------------------

![](https://img-blog.csdnimg.cn/4bc0a109725f44fa99425f2856376bea.png)

**以上 Centos 安装 python3 导入 ssl 时解决 ModuleNotFoundError: No module named '_ssl'问题的全部解决过程！！！**

五、注 1：openssl 或 nginx 离线安装依赖库（没网可离线，也可在线）：
==========================================

**第（1）步在线安装和第（2）步离线安装，二选一安装即可。**
### （1）如果是**在线安装**直接用 yum 命令就可以安装，依赖安装成功即可。

> yum install -y zlib zlib-dev openssl-devel sqlite-devel bzip2-devel libffi libffi-devel gcc gcc-c++
### （2）如果是**离线安装**直接下载下面这个我传到百度云上的安装依赖包就可以，里面有依赖包和 openssl1.0.2 版本。
### **该离线包同样可以用于 nginx 离线安装环境依赖。**

**安装包下载方式：百度云、CSDN、openssl 官网均可。**

A、链接：https://pan.baidu.com/s/1mdwddtYEo-_qr3OUmBtxMw   
提取码：qy2k

![](https://img-blog.csdnimg.cn/2c89ea745b04487d811498841e1784d3.png)​

B、CSDN 下载资源，已设置免积分下载

[Linux 系统 centos7 环境下安装 openssl1.0.2 所需安装包及 nginx 离线安装所需全部依赖包，均可使用 - Linux 文档类资源 - CSDN 文库](https://download.csdn.net/download/qq_39715000/85482728 "Linux系统centos7环境下安装openssl1.0.2所需安装包及nginx离线安装所需全部依赖包，均可使用-Linux文档类资源-CSDN文库")

C、最新版 openssl 安装包也可在 openssl 官网下载。

[/source/index.html![](https://www.openssl.org/favicon.ico)https://www.openssl.org/source/](https://www.openssl.org/source/ "/source/index.html")

(3) 依赖包解压如下： 

![](https://img-blog.csdnimg.cn/58ddc0df883c4ce1b90811d083861540.png)​

具体打开文件夹如下：

![](https://img-blog.csdnimg.cn/1cf58afa3edf4996ab1e64d50a7e0681.png)​

 安装命令如下：

> **rpm -ivh   xxxx.rpm        --nodeps**
> 
> 命令含义注释：rpm -ivh (rpm 包的安装命令)，--nodeps 跳过环境依赖
> 
> 例如：
> 
> **rpm -ivh openssl-devel-1.0.2k-19.el7.x86_64.rpm --nodeps**

 --------------------------------------------------------- 以下无正文 ------------------------------------------------------

**注: 仅供学习，记录问题和参考，共勉！**

**参考文章：**

1、[centos 解决 python3.7 安装时 No module named _ssl - 小小日常 - 博客园](https://www.cnblogs.com/xiaoxiaorichang/p/14781778.html "centos 解决python3.7 安装时No module named _ssl - 小小日常 - 博客园")

2、[​​​​​​关于服务器上安装新版 Python 报错的问题_Ricardo5566 的博客 - CSDN 博客​​​​​​](https://blog.csdn.net/Ricardo5566/article/details/122487102 "​​​​​​关于服务器上安装新版Python报错的问题_Ricardo5566的博客-CSDN博客​​​​​​")

3、[python3 虚拟环境中解决 ModuleNotFoundError: No module named '_ssl'_北极冰熊的博客 - CSDN 博客​​​​​​ ​​​​​​](https://blog.csdn.net/qq_26870933/article/details/84336109 "python3虚拟环境中解决 ModuleNotFoundError: No module named '_ssl'_北极冰熊的博客-CSDN博客​​​​​​ ​​​​​​")

4、[CentOS7 设置环境变量_C 语言实战技术的博客 - CSDN 博客](https://blog.csdn.net/m0_45133894/article/details/104753656 "CentOS7设置环境变量_C语言实战技术的博客-CSDN博客")

5、[configure --prefix=/ 的作用和用法_今天也是橙汁的博客 - CSDN 博客](https://blog.csdn.net/weixin_43689342/article/details/114628288 "configure --prefix=/的作用和用法_今天也是橙汁的博客-CSDN博客")

6、[Linux ./configure --prefix 命令是什么意思？](https://www.shuzhiduo.com/A/xl56KWZ9dr/ "Linux ./configure --prefix 命令是什么意思？")

7、[Linux 更改 python 软连接 - 知乎](https://zhuanlan.zhihu.com/p/377947642 "Linux更改python软连接 - 知乎")

8、[linux 安装和卸载 python3_悠悠 - 我心的博客 - CSDN 博客_linux 卸载 python3](https://blog.csdn.net/liu_yulong/article/details/119728016 "linux安装和卸载python3_悠悠-我心的博客-CSDN博客_linux卸载python3")