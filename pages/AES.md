- > 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [cloud.tencent.com](https://cloud.tencent.com/developer/article/1420428)
  
  > https://blog.csdn.net/K346K346/article/details/89387460
  
  发布于 2019-05-05 11:26:30 阅读 6.8K0
### 相关文章

本文要点在于 Python 扩展库 pycrypto 实现了大量密码学算法，可以拿来直接使用。 import string import random from Cry...

*   ### DES、AES、RSA 等常用加解密算法说明
  
  加密一般分为可逆加密和不可逆加密，其中可逆加密一般又分为对称加密和非对称加密，以下为常用加密算法：
  
*   ### Java 加解密 AES、DES、TripleDES、MD5、SHA
  
*   ### python 使用 AES 加解密 No module named Crypto.Cipher
  
  首先说明：pycryptodome pycrypto 这两个库是同一个库，但是 pycrypto 已经不维护了
  
*   ### 使用 openssl 命令加解密 aes-128-cbc 的简单示例
  
  版权声明：欢迎传播, 请标明出处。 https://blog.csdn.net/u201011221/article/details/8278...
  
*   ### Java 实现 AES ECP PKCS5Padding 加解密工具类
  
  如果我们将加密后的字节数组，直接 new String() 获得一个字符串，然后解密这个字符串，会发现解密失败哦
  
*   ### [Go] go 语言使用 dgrijalva/jwt-go 实现加解密 jwt
  
*   ### PHP 7.1 中 AES 加解密方法 mcrypt_module_open() 的替换方案
  
  前言 mcrypt 扩展已经过时了大约 10 年，并且用起来很复杂。因此它被废弃并且被 OpenSSL 所取代。 从 PHP 7.2 起它将被从核心代码中移除并且移到 P...
  
*   ### 通过 Go 实现 AES 加密和解密工具
  
  AES(advanced encryption standard) 使用相同密钥进行加密和解密，也就是对称加密。其他的对称加密如 DES，由于 DES 密钥长度只有 5...
  
*   ### JAVA——Base64 编解码原理及 AES 加解密算法的使用
  
  Base64 编码原理：将要编码的二进制（字符串、图片等都可以转换成二进制格式表示）把 3 个 8 位字节以 4 个 6 位的字节表示，然后把每个 6 位字节都转换成一个单独的数字并...