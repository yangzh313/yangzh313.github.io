title:: 安装git
> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/a10703060237/article/details/89704924)

https://blog.51cto.com/superleedo/2057841

1. 安装 git
=========

*_Note: _不要用 yum 安装 git__， [yum](https://so.csdn.net/so/search?q=yum&spm=1001.2101.3001.7020) 源安装 git 最新版本是 1.8.3，该版本太老，之后用 git commit 有可能会报错：git: ‘interpret-trailers’ is not a git command. See ‘git --help’.  
cannot [insert](https://so.csdn.net/so/search?q=insert&spm=1001.2101.3001.7020) change-id line in .git/COMMIT_EDITMSG  
必须安装更新版本，比如 git-2.12.2， 需要掌握手动编译安装 git 的方法。

后续章节使用 gerrit, 会安装 gitweb, git-review， **yum 安装 gitweb, git-review 会同时安装 git（老版本 1.8.3）， 会覆盖我们安装的新版本**，因此我们在安装 git 之前安装 gitweb, git-review.

1) 安装 git 依赖包
-------------

```
yum install -y curl-devel expat-devel gettext-devel openssl-devel zlib-devel gcc perl-ExtUtils-MakeMaker

```

2）安装 gitweb
-----------

```
yum install -y gitweb

```

3）安装 git-review
---------------

把代码提交到 gerrit 上进行 review，会用到 git review 命令，可以很简单去把修改 push 到 gerrit，这需要我们单独安装 git-review 插件。

```
yum install -y git-review

```

4) 卸载已安装的 git
-------------

如果环境已经用 yum 装过 git， gitweb，或 git-review，则**不能用 yum remove git** 来卸载 git， 因为这样会同时卸载依赖包 gitweb, git-review.

卸载老 git 而不卸载依赖包 gitweb， git-review ：

```
rpm -e --nodeps git

```

这个命令，就只删除 git 这个包，不会删除依赖包 gitweb, git-review

5) 下载并安装 git
------------

a. 进入目录 / usr/local/, 将 git 下载到该目录

```
	wget https://mirrors.edge.kernel.org/pub/software/scm/git/git-2.12.2.tar.gz

```

b. 解压 git-2.12.2.tar.gz

```
	tar -xzvf git-2.12.2.tar.gz

```

#c. 配置 git 安装路径:

```
#	cd /usr/local/git-2.12.2
	#./configure prefix=/usr/

```

#d. 编绎并且安装

make && make install
====================

编译 git 源码，进入 cd cd /usr/local/git-2.12.2 目录

make prefix=/usr/local/git all

安装 git 至 / usr/bin/git 路径

make prefix=/usr/local/git install

e. 把 git 加到环境变量

```
vim /etc/profile
把:/usr/local/git/bin 添加到$PATH后面
source /etc/profile

```

e. 检查 git 版本是不是安装的版本 2.12.2

```
	git version

```

![](https://img-blog.csdnimg.cn/20190430094248652.png){:height 92, :width 527}

2. 初次运行 git 前的配置
================

配置 git 用户
---------

```
cd ~
touch .git-credentials
vi .git-credentials
http://root:123456@$CentOSIP

git config --global credential.helper store
git config --global user.name $username
git config --global user.email $mail
git config --list    //查看配置信息

```

Git 提供了一个叫做 git config 的工具（译注：实际是 git-config 命令，只不过可以通过 git 加一个名字来呼叫此命令。），专门用来配置或读取相应的工作环境变量。而正是由这些环境变量，决定了 Git 在各个环节的具体工作方式和行为。这些变量可以存放在以下三个不同的地方：

a. /etc/gitconfig 文件：系统中对所有用户都普遍适用的配置。若使用 git config 时用 --system 选项，读写的就是这个文件。  
b. ~/.gitconfig 文件：用户目录下的配置文件只适用于该用户。若使用 git config 时用 --global 选项，读写的就是这个文件。  
c. 当前项目的 Git 目录中的配置文件（也就是工作目录中的 .git/config 文件）：这里的配置仅仅针对当前项目有效。每一个级别的配置都会覆盖上层的相同配置，所以 .git/config 里的配置会覆盖 /etc/gitconfig 中的同名变量。

3. git 服务器的搭建
=============

大家应该都用过 svn, git, github, gitlab 等等吧，github 是公用仓库，如果私有仓库是要付费的，gitlab 私有仓库不用付费，大家都可以选择。

除了 github, gitlab 之外，我们也可以在自己的服务器上搭建自己的 git 服务器。  
这样一来呢，速度变快了，而且更加方便管理，安全方面也得到了提升。

1） 创建初始化 Git 仓库
---------------

首先我们需要选定一个目录作为 git 仓库， 进入到任意想要放置仓库的目录下，一般来说 git 仓库的名称都是以. git 结尾。

```
cd /opt/   
git init --bare devops_test.git   #初始化项目

```

2) 配置证书登陆
---------

配置所有需要登陆的用户的公钥，公钥位于 id_rsa.pub 文件中，如果没有的话可以使用 ssh-keygen 生成。然后将我们的公钥添加到服务器的 authorized_keys 文件中。

```
cd ~  
ssh-keygen -t rsa     //生成证书
cat .ssh/id_rsa.pub > authorized_keys
chmod 644 authorized_keys

```

把私钥加入 git SSH Keys

3）克隆仓库
------

```
cd /tmp/
git clone root@$CentOSIP:/opt/devops_test.git

```

4) 提交更新到 git 仓库
---------------

把我们的文件加入到本地仓库中，然后执行如下命令将项目提交到服务器！

```
cd devops_test
把需要提交的文件放到该目录
git add .
git commit -m "xxxxx"
git push origin/master
或者
git review 

```

**Note:　养成好的习惯，在每次准备修改代码前一定先执行 git pull 命令将本地仓库同步到最新版本！！！减少冲突的可能。**

4. issues
=========

Issue 1: git: ‘interpret-trailers’ is not a git command. See ‘git --help’.
--------------------------------------------------------------------------

cannot insert change-id line in .git/COMMIT_EDITMSG  
很奇怪，加上 issue3 的 solution gitdir，[commit](https://so.csdn.net/so/search?q=commit&spm=1001.2101.3001.7020)-msg 后，就是出现这个问题，平时不会有，在网上也找不到答案。

**solution:**  
check whether it is existed in git installation path: such as /usr/libexec/git-core  
we can get the git-ls-tree, git-mailinfo, git-ls-remote and so on

**Update git version to git-2.12.2:**  
a. 下载 git 包到目录 / usr/src/， 并解压  
wget https://mirrors.edge.kernel.org/pub/software/scm/git/git-2.12.2.tar.gz  
b. 卸载老 git 而不卸载依赖包 gitweb， git-review ：  
rpm -e --nodeps git 这个命令，就只删除 git 这个包，不会删除依赖包，  
c. 配置 git 安装路径

```
cd /usr/src/git-2.12.2
./configure prefix=/usr/local/git/

```

d. 编译并且安装

```
make && make install

```

issue 2:bash: git-upload-pack: command not found
------------------------------------------------

代码服务器上的 git 安装路径是 / usr/local/git，不是默认路径，根据提示，在 git 服务器上， 建立链接文件：

**Solution：**  
硬连接 ln /usr/local/git/bin/git-upload-pack /usr/sbin/

issue 3: pemission denied(publickey) while git clone ssh://gerrit@xx.xx.xx.xx:29418/devops_test.git
---------------------------------------------------------------------------------------------------

**solution:**  
删除~/.ssh 下的 keys, ssh-keygen-t rsa 生成，然后通过 cat id_rsa.pub 拷入 gerrit setting 中的 public 选项中

Issue 4: git review: Could not connect to gerrit. We don’t know where your gerrit is. Please manually create a remotenamed “gerrit” and try again.
--------------------------------------------------------------------------------------------------------------------------------------------------

Enter your gerrit username: gerrit  
Trying again with ssh://gerrit@xx.xx.xx.xx:29418/devops_test.git  
<traceback object at 0x7f4daf6fec68>  
We don’t know where your gerrit is. Please manually create a remote  
named “gerrit” and try again.

**Solution:**  
手动添加节点 gerrit 节点  
git remote add gerrit ssh://gerrit@xx.xx.xx.xx:29418/devops_test.git

issue 5: git review: ! [remote rejected] HEAD -> refs/publish/master (no common ancestry)error: failed to push some refs to ‘ssh://gerrit@xx.xx.xx.xx:29418/devops_test.git’
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------

**Solution:**  
a solution is to start in a new folder, then clone the (empty) project from gerrit. This folder then have a common ancestry with this gerrit project. Then you could work to add your source files into this new folder using git add until satisfied,(that mundane work is the non-easy part) leaving out any old .git folder and finally try a push. This pushes your project onto gerrit with git history beginning from this point on.

Issue 6: ! [remote rejected] HEAD -> refs/publish/master (commit 1e94fe0: missing Change-Id in message footer)
--------------------------------------------------------------------------------------------------------------

error: failed to push some refs to ‘ssh://gerrit@xx.xx.xx.xx:29418/devops_test.git’

**Solution:**  
依次在项目路径下输入如下命令，即可解决：  
gitdir=$(git rev-parse --git-dir);  
# 将 xxxxx@xxxx 替换成相应用户名、服务器即可（该命令从服务器拷贝 commit-msg 文件）  
scp -p -P 29418 xxxxx@xxxx:hooks/commit-msg ${gitdir}/hooks/  
再提交一次即可生成 change-id  
git commit –amend
