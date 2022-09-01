- > 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/weixin_42256397/article/details/97283989)
  
  【背景】
  
  服务器中开通 [NFS](https://so.csdn.net/so/search?q=NFS&spm=1001.2101.3001.7020) 服务供别人访问服务器上的文件
  
  期间，NFS 服务器端已经配置了写权限：
  
  /home/share/image *(rw,[sync](https://so.csdn.net/so/search?q=sync&spm=1001.2101.3001.7020))  
  然后客户端去 mount：
  
  mount -t nfs 121.41.120.185:/home/share/image /root/nfs_client_root/  
  可以看到文件了：
  
  root@chantyou:nfs_client_root# ls -l  
  total 240  
  -rw-r–r-- 1 root root 70545 May 28 2013 mmexport1369703131812.jpg  
  -rw-r–r-- 1 root root 82168 Sep 12 2013 mmexport1378988706739.jpeg  
  -rw-r–r-- 1 root root 85510 Nov 18 2013 p_large_0fOT_43d9000068f01263.jpg  
  但是却无法写入：
  
  root@chantyou:nfs_client_root# sudo touch test_client_write.txt  
  touch: cannot touch ‘test_client_write.txt’: Permission denied  
  【解决过程】
  
  root@iZ23lqgttauZ:image# ls /home/share/ -l  
  total 4  
  drwxr-xr-x 2 root root 4096 Jul 23 15:16 image  
  root@iZ23lqgttauZ:image# ls /home/share/image/ -l  
  total 240  
  -rw-r–r-- 1 root root 70545 May 28 2013 mmexport1369703131812.jpg  
  -rw-r–r-- 1 root root 82168 Sep 12 2013 mmexport1378988706739.jpeg  
  -rw-r–r-- 1 root root 85510 Nov 18 2013 p_large_0fOT_43d9000068f01263.jpg  
  root@iZ23lqgttauZ:image#  
  好像：
  
  NFS 服务器端，没有给文件夹：
  
  /home/share/image/
  
  开通，g=group，o=other 的写的权限。
  
  所以去开通试试：  
  3lqgttauZ:image# chmod go+w /home/share/image/  
  root@iZ23lqgttauZ:image# ls /home/share/ -l  
  total 4  
  drwxrwxrwx 2 root root 4096 Jul 23 15:16 image  
  root@iZ23lqgttauZ:image#  
  然后 NFS 客户端再去试试写入是否可行：  
  真的可以了：
  
  root@chantyou:nfs_client_root# sudo touch test_client_write.txt  
  root@chantyou:nfs_client_root#
  
  【总结】
  
  NFS 的 server 端，虽然当前用户 root，对于 NFS 共享出来的文件夹是有自己的写入权限，但是没有开通自己组 group 和其他人 other 的写权限：
  
  root@iZ23lqgttauZ:image# ls /home/share/ -l  
  total 4  
  drwxr-xr-x 2 root root 4096 Jul 23 15:16 image  
  所以 NFS 客户端，去写入，应该属于 other 的权限，没法写出，出现 Permission denied
  
  解决办法是：
  
  NFS 客户端的共享出来的文件夹，开通 other 的写入权限：
  
  root@iZ23lqgttauZ:image# chmod go+w /home/share/image/  
  root@iZ23lqgttauZ:image# ls /home/share/ -l  
  total 4  
  drwxrwxrwx 2 root root 4096 Jul 23 15:16 image  
  然后 NFS 的客户端就可以正常写入了：
  
  root@chantyou:nfs_client_root# sudo touch test_client_write.txt
  
  就是权限的问题。
### 方法二

加了 no_root_squash，也可以解决问题。  
no_root_squash: 有 root 的权限，不建议使用
-
- ## nfs v4
- [[NFS shares are mounted as "nobody" | TrueNAS Community]]
-
-