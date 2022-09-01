- > 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/qq_42780289/article/details/98049574)
### git fetch 命令的使用

*   **从远程主机克隆**

Git 的 clone 命令会为你自动将远程主机命名为 origin，拉取它的所有数据，创建一个指向它的 master 分支的指针，并且在本地将其命名为 origin/master。同时 Git 也会给你一个与 origin 的 master 分支在指向同一个地方的本地 master 分支，这样你就有工作的基础。

![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly93czEuc2luYWltZy5jbi9sYXJnZS8wMDZWckpBSmd5MWc1azA4dGl6ZXNqMzBudzBlb3Q5NS5qcGc)

*   **本地有提交，远程也有别人的推送**

远程库有人推送，提交了 C0 和 C1：  
![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly93czEuc2luYWltZy5jbi9sYXJnZS8wMDZWckpBSmd5MWc1azBsOTZzbHlqMzBvbTA2bjc0Yi5qcGc)  
本地提交了 D0 和 D1：

只要你不与 origin 服务器连接，你的 origin/master 指针就不会移动。  
![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly93czEuc2luYWltZy5jbi9sYXJnZS8wMDZWckpBSmd5MWc1azBsenVsbG5qMzBvaDA4OXdlbS5qcGc)

*   **同步**

如果要同步远程库到你的工作，运行 git fetch origin 命令。

```
$ git fetch origin

```

这个命令查找 “origin” 是哪一个服务器，从中抓取本地没有的数据，并且更新本地数据库，移动 origin/master 指针指向新的、更新后的位置。  
![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly93czEuc2luYWltZy5jbi9sYXJnZS8wMDZWckpBSmd5MWc1azByZG5pNGRqMzBtazBmdmRnZS5qcGc)  
要特别注意的一点是 fetch 抓取到新的远程跟踪分支时，本地的工作区（workspace）不会自动生成一份可编辑的副本，抓取结果是直接送到版本库（Repository）中。如下图：  
![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly93czEuc2luYWltZy5jbi9sYXJnZS8wMDZWckpBSmd5MWc1azB3enQwZ3ZqMzBtZDA2Zzc2dy5qcGc)  
打个比方，在远程库 origin 新建了一个分支 dev，git fetch 后本地不会生成一个新的分支 dev（可用 git branch 查看），只有一个不可以修改的 origin/dev 指针。

*   **在 origin/master 后继续工作**

如果想要在 origin/master 分支上工作，可以新建分支 test 并将其建立在远程跟踪分支之上：

```
$ git checkout -b test origin/master

```

这会给你新建一个用于工作的本地分支 test，并且起点位于 origin/master。  
![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly93czEuc2luYWltZy5jbi9sYXJnZS8wMDZWckpBSmd5MWc1azMzcnp2b3FqMzBtbDBiaW14Zi5qcGc)

*   **合并**  
  ![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly93czEuc2luYWltZy5jbi9sYXJnZS8wMDZWckpBSmd5MWc1azM3Ync4OHJqMzBtbTA5b3dlcC5qcGc)  
  如果想把拉取的结果合并到本地分支，需要手动合并。使用如下命令：

```
$ git chekout master
$ git merge origin/master

```

![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly93czEuc2luYWltZy5jbi9sYXJnZS8wMDZWckpBSmd5MWc1azE3enBuaHhqMzBvaTA5dWFhYy5qcGc)  
然而，看到上面的合并结果会想到命令 git pull 。在大多数情况下它的含义是一个 git fetch 紧接着一个 git merge 命令。即 git pull 是 git fetch 和 git merge 的两步的和。

但是由于 git pull 的使用经常令人困惑，所以通常单独显式地使用 fetch 与 merge 命令会更好一些。