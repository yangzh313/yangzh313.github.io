- > 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [zhuanlan.zhihu.com](https://zhuanlan.zhihu.com/p/417259374)
  
  > 本文使用 [Zhihu On VSCode](https://zhuanlan.zhihu.com/p/106057556) 创作并发布
  
  Table of Contents
  -----------------
  
  1.  [前言](https://zhuanlan.zhihu.com/p/417259374/edit#orga285da9)
  2.  [Hugo 的安装和使用](https://zhuanlan.zhihu.com/p/417259374/edit#org3d6f2eb)
  
  1.  [安装 Hugo](https://zhuanlan.zhihu.com/p/417259374/edit#org982ed93)
  2.  [创建个人网站](https://zhuanlan.zhihu.com/p/417259374/edit#org3679967)
  3.  [使用 Hugo 主题](https://zhuanlan.zhihu.com/p/417259374/edit#org8eceeff)
  4.  [个人配置和网站生成](https://zhuanlan.zhihu.com/p/417259374/edit#org068749b)
  
  1.  [GitHub Pages](https://zhuanlan.zhihu.com/p/417259374/edit#orgcae6cf9)
  
  1.  [使用 GitHub Action 自动发布文章](https://zhuanlan.zhihu.com/p/417259374/edit#orged17b2c)
  
  前言
  --
  
  相信很多人都有搭建个人网站的需求，可能是想写自己的博文，传达一些思想给社区。也有可能你是一名科研工作者，需要搭建个人学术主页展示科研成果。我也或多或少出于以上原因，选择搭建了个人网站，搭建过程中出现了许多问题，因此记录和分享，避免下次踩坑。Anyway，本文记录使用搭建个人博客的全过程，包括网站工具 [Hugo](https://gohugo.io/) 的介绍和使用设置，以及如何将个人网站发布在免费托管平台，也就是 GitHub Pages 上。
  
  我选择 Hugo 的原因主要有三点：
  
  *   简单易用；
  *   [Hugo](https://gohugo.io/) 能够快速地构建个人网站；
  *   拥有丰富的主题，可供挑选 [Hugo Themes](https://jamstackthemes.dev/ssg/hugo/)；
  
  选择 GitHub Pages 的原因就更简单了，免费又好用。
  
  Hugo 的安装和使用
  -----------
  
  Hugo 宣传号称是世界上最快构建网站的框架，也是最流行的静态网站生成工具之一。
  
  安装 Hugo
  -------
  
  由于我的操作系统是 MacOs 因此安装起来特别简单：
  
  ```
  brew install hugo
  
  
  ```
  
  其他平台可参考 [Hugo Install](https://gohugo.io/getting-started/installing)。
  
  创建个人网站
  ------
  
  ```
  hugo new site quickstart
  
  
  ```
  
  使用 Hugo 主题
  ----------
  
  我使用的是 [jane](https://github.com/xianmin/hugo-theme-jane), 将主题 `clone` 到 `theme` 目录下：
  
  ```
  cd quickstart
  git clone https://github.com/xianmin/hugo-theme-jane.git --depth=1 themes/jane
  
  
  ```
  
  使用示例文本和默认的站点设置：
  
  ```
  cp -r themes/jane/exampleSite/content ./
  cp themes/jane/exampleSite/config.toml ./
  
  
  ```
  
  启动 Hugo 服务器，在 [http://localhost:1313/](http://localhost:1313/) 将会看到示例 jane 主题的示例网站：
  
  ```
  hugo server
  
  
  ```
  
  个人配置和网站生成
  ---------
  
  配置文件在网站根目录 `quickstart` 下 `config.toml` , 根据自身需求进行修改。在 jane 主题下的 `exampleSite` 文件夹中的文件可作为参考。默认的文章将存储在 `./content/post` 中，每当写完文章，运行 `hugo` 命令，Hugo 将自动生成静态网站到 `public` 文件夹中，我们只需要将该文件夹的内容发布在网络上即可。
  
  更多关于主题的配置可以参考 [jane README.md](https://github.com/xianmin/hugo-theme-jane/blob/master/README-zh.md)。
  
  GitHub Pages
  ------------
  
  我个人经常使用 GitHub，也见到很多大佬利用 GitHub pages 挂载自己的个人网站，发现配置起来也很简单，因此选择使用 GitHub pages 来进行配置，关于 GitHub pages 可以查看[官网](https://pages.github.com/)，主要包括四个步骤：
  
  1.  创建一个与 `username` 同名的 **空** `username.github.io` 仓库，不包含任何内容，如 `readme.md，比如我的用户名为`xxx, 因此我创建了一个仓库，名为`xxx.github.io`;
  2.  克隆仓库到本地  
      git clone [https://github.com/xxx/xxx.github.io](https://github.com/xxx/xxx.github.io)  
      
  3.  添加个人网站内容到该仓库  
      # copy 生成的网站内容到仓库文件夹下  
      cp -rf quickstart/public/* [http://xxx.github.io/](http://xxx.github.io/)  
      
  4.  将文件内容同步更新到 GitHub 服务器上  
      cd [http://xxx.github.io](http://xxx.github.io)  
      git add .  
      git commit -m "init the website"  
      git push  
      此时，通过进入 [https://xxx.github.io](https://kinredon.github.io/) 即可访问自己的个人网站。
  
  上面的步骤略显麻烦，每次需要将生成在 `public` 文件夹下的目录拷贝到 xxx`.github.io` 目录下，然后发布到远程服务器。根据 [Host on GitHub](https://gohugo.io/hosting-and-deployment/hosting-on-github/)，发布到 GitHub Pages 有两种方式, 一种是直接使用仓库目录下的 `doc` 目录作为原本 `public` 目录，详情可以参考 [hugo 博客搭建](https://patrolli.github.io/xssq/post/hugo_%E5%8D%9A%E5%AE%A2%E6%90%AD%E5%BB%BA/)。我采用的方式是利用 GitHub Action 自动完成上述过程。
  
  使用 GitHub Action 自动发布文章
  -----------------------
  
  这里主要参考 [搭建个人 blog](https://vinurs.me/posts/1a329bf3-fbb7-4006-9714-d3b072826376/), 使用 `master` 分支发布文章，使用一个新的 `source` 分支进行写作，写作完成后上传 `source` ，GitHub Action 自动将 `source` 分支的 `publish` 文件夹拷贝到 `master` 分支，从而完成文章的发布。
  
  主要步骤如下：
  
  1.  在 GitHub 上的个人网站仓库`xxx.github.io` 新建 `source` 分支  
      
  
  ![](https://pic2.zhimg.com/v2-a1efccf71116ca7bf1e3aff4889e5445_b.png)
  
  1.  清除文件夹 `xxx.github.io` 中的内容，并将个人网站 `quickstart` 中的所有内容 copy 到 `xxx.github.io` ：  
      git clone --branch=source [https://github.com/xxx/xxx.github.io.git](https://github.com/xxx/xxx.github.io.git)  
      rm -rf [http://xxx.github.io/](http://xxx.github.io/)*  
      cp -rf quickstart/* [http://xxx.github.io](http://xxx.github.io)  
      
  2.  生成 `ACTIONS_DEPLOY_KEY`  
      ssh-keygen -t rsa -b 4096 -C "$(git config user.email)" -f gh-pages -N ""  
      将生成的私钥文件 `gh-pages` (注意不是公钥 `gh-pages.pub`) 中的内容复制填写到 GitHub 仓库设置中，即在 `xxx.github.io` 项目主页中，找到 Repository Settings -> Secrets -> 添加这个私钥的内容并命名为 `ACTIONS_DEPLOY_KEY` 。  
      然后在 `xxx.github.io` 项目主页中，找到 Repository Settings -> Deploy Keys -> 添加这个公钥的内容，命名为 `ACTIONS_DEPLOY_KEY` ，并勾选 Allow write access。
  3.  配置 workflow  
      创建 workflow 文件  
      mkdir -p .github/workflows/  
      touch .github/workflows/main.yml  
      在 `main.yaml` 中撰写 workflow，内容如下：  
      name: github pages  
      on:  
      push:  
      branches:
	- source  
	      jobs:  
	      build-deploy:  
	      runs-on: ubuntu-18.04  
	      steps:
	- uses: actions/checkout@master
	- name: Checkout submodules  
	      shell: bash  
	      run: |  
	      auth_header="$(git config --local --get http.[https://github.com/.extraheader](https://github.com/.extraheader))"  
	      git submodule sync --recursive  
	      git -c "http.extraheader=$auth_header" -c protocol.version=2 submodule update --init --force --recursive --depth=1
	- name: Setup Hugo  
	      uses: peaceiris/actions-hugo@v2  
	      with:  
	      hugo-version: 'latest'  
	      extended: true
	- name: Build  
	      run: hugo --gc --minify --cleanDestinationDir
	- name: Deploy  
	      uses: peaceiris/actions-gh-pages@v3  
	      with:  
	      deploy_key: ${{secrets.ACTIONS_DEPLOY_KEY}}  
	      publish_dir: ./public  
	      publish_branch: main  
	      注意，如果你的仓库是 `master` 分支作为主分支，将 `publish_branch` 后面的 `main` 修改为 `master` ;
	  4.  将 source 分支发送到远程  
	      发送脚本 `deploy.sh` :  
	      #!/bin/bash  
	      git add .  
	      git commit -m "update article"  
	      git push  
	      推送到远程分支：  
	      sh deploy.sh  
	      推送成功后，可以在项目主页中的 action 里面查看自动部署是否成功，即 [https://github.com/xxx/xxx.github.io/actions](https://github.com/kinredon/kinredon.github.io/actions)；
	-