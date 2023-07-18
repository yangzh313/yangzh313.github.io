- description
	- 公钥已上传到gerrit
	- ```
	  git clone -b ar-dbagent-1.5-dev "ssh://*@gerrit-infrastructure.archeros.cn:29418/ar-dbagent"
	  Cloning into 'ar-dbagent'...
	  *@gerrit-infrastructure.archeros.cn: Permission denied (publickey).
	  fatal: Could not read from remote repository.
	  
	  Please make sure you have the correct access rights
	  and the repository exists.
	  
	  ssh -V
	  OpenSSH_8.9p1 Ubuntu-3, OpenSSL 3.0.2 15 Mar 2022
	  ```
- 原因
	- OpenSSH8.8考虑到cryptographically broken，仅用了使用SHA-1哈希算法的RSA签名方法，这是一个客户端限制。
- 解决
	- 提供能被OpenSSH 8.8认可的密钥类型，比如Ed25519
	- ```
	  yangzh22@HY03-WX01-1350:~/projects/ar-dbagent-1.5-dev$ ssh-keygen -t ed25519 -C "*.com"
	  Generating public/private ed25519 key pair.
	  Enter file in which to save the key (/home/yangzh22/.ssh/id_ed25519):
	  Enter passphrase (empty for no passphrase):
	  Enter same passphrase again:
	  Your identification has been saved in /home/yangzh22/.ssh/id_ed25519
	  Your public key has been saved in /home/yangzh22/.ssh/id_ed25519.pub
	  The key fingerprint is:
	  SHA256:/qQzBKmsaqB6R+HKTpKuTt3CRydldzDddj03nc5EgMQ *.com
	  The key's randomart image is:
	  +--[ED25519 256]--+
	  |         o+.o.o.+|
	  |          oE o *+|
	  |       + . .. = +|
	  |    . = . .    o |
	  |   o = oS        |
	  |..o B o..        |
	  |=o.B o .. .      |
	  |++= +   o+       |
	  |O*..    .o.      |
	  +----[SHA256]-----+
	  yangzh22@HY03-WX01-1350:~/projects/ar-dbagent-1.5-dev$
	  yangzh22@HY03-WX01-1350:~/projects/ar-dbagent-1.5-dev$ ssh-agent bash
	  yangzh22@HY03-WX01-1350:~/projects/ar-dbagent-1.5-dev$ ssh-add
	  Identity added: /home/yangzh22/.ssh/id_rsa (/home/yangzh22/.ssh/id_rsa)
	  Identity added: /home/yangzh22/.ssh/id_ed25519 (*.com)
	  yangzh22@HY03-WX01-1350:~/projects/ar-dbagent-1.5-dev$ cat ~/.ssh/id_ed25519.pub
	  ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAINjI6YDzU/RGDWUob/PeEMjWVEwACqvwIpOWYLec41CT *.com
	  yangzh22@HY03-WX01-1350:~/projects/ar-dbagent-1.5-dev$
	  yangzh22@HY03-WX01-1350:~/projects/ar-dbagent-1.5-dev$ git clone -b ar-dbagent-1.5-dev "ssh://*@gerrit-infrastructure.archeros.cn:29418/ar-dbagent"
	  Cloning into 'ar-dbagent'...
	  remote: Counting objects: 84, done
	  remote: Finding sources: 100% (84/84)
	  remote: Total 695 (delta 28), reused 668 (delta 28)
	  Receiving objects: 100% (695/695), 235.26 KiB | 1.15 MiB/s, done.
	  Resolving deltas: 100% (325/325), done.
	  ```
- 参考
	- [(49条消息) ubuntu-2204 gerrit ssh 报错Permission denied (publickey).分析及解决_halazi100的博客-CSDN博客_gerrit permission denied (publickey)](https://blog.csdn.net/halazi100/article/details/124496131)
	- [(49条消息) ssh公钥问题（Could not open a connection to your authentication agent.）_沧澜阁云归处的博客-CSDN博客](https://blog.csdn.net/benisarookie/article/details/113114604)
	-