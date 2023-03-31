- > 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [mp.weixin.qq.com](https://mp.weixin.qq.com/s/jkXFsnUIrt4MkspD-uiuYg)
  
  ![](https://mmbiz.qpic.cn/mmbiz_png/icdBCpcEToIib6fclY9h0sicSY9iadYfwicKcJ6Kttg5gMibHtfoGpSYNjkX9ibRbJOu821Q3RouJ0l9wPEyGSvTZlUgA/640?wx_fmt=png)
  
  **【导语】**使用 NFS 文件系统，出现客户端 HANG 死，或者 df 命令 HANG 死的情况并不少见，原因也十分复杂，往往不容易定位，本文结合作者实践提出了有效的分析方法，希望能够对大家解决此类问题有所助益。
  
  **【作者】白鳝（徐戟），**南京基石数据技术有限责任公司技术总监，在软件开发、系统运维、信息系统优化、信息系统国产化替代等领域从事技术研究近 30 年，曾主持开发了国内首套电信级联机实时计费系统、国内首套三检合一的检验检疫管理系统、银行综合大前置平台（IPP）等大型系统。著有《Oracle RAC 日记》、《Oracle DBA 优化日记》和《DBA 的思想天空》等技术专著。信息无障碍研究会专职顾问，深圳市鲲鹏产业联盟高级顾问，Oracle ACE，POSTGRESQL ACE DIRECTOR。个人微信公众号：白鳝的洞穴
  
  有个朋友咨询一个问题，他通过 NFS MOUNT 了一个分布式文件系统，发现对这个文件系统 df 的时候经常 hang 死，他检查了一番系统，发现内存使用率很高，大部分物理内存都被 cache 占用了。他想通过分析 cache 中都有哪些数据，通过这个分析来确认 cache 占用率高和 NFS hang 之间是否存在关联关系。当时我的建议是首先不要把问题直接定位到 OS 内存上，如果系统不存在严重的 SWAP，哪怕物理内存使用率达到 98%，也是关系不大的。
  
  实际上使用 NFS 文件系统，出现客户端 hang 死，或者 df 命令 HANG 死的情况并不少见，我这些年里也遇到过多次。我遇到的 NFS hang 死问题的原因也十分复杂，不过大多数都与 NFS BUG、网络问题、系统资源消耗过高、IO 负载过大等有关。NFS 客户端访问 NFS 文件 hang 住，一般来说有三种可能性，一种是客户端出现问题，第二种是服务端出现问题，第三种是客户端和服务端都存在问题。似乎这个总结有点太笼统了，也有点投机取巧，不过穷举法是我们在针对未知问题分析的最重要的方法。如果我们不能穷举所有的可能性，那么在问题诊断分析的时候就可能无法定位。在我多年的经验里，很多当时认为十分灵异的问题，都是因为我们以前的知识面不足，无法穷举到真正的故障路径。
  
  ![](https://mmbiz.qpic.cn/mmbiz_png/icdBCpcEToIib6fclY9h0sicSY9iadYfwicKcVfJnTQB3m8ibMqLox2PT80Gv9ss7LWmxFk7NQBsIuc7qqic4Oxfia1Wibg/640?wx_fmt=png)
  
  上面的思维导图是我这些年遇到过的一些 NFS HANG 的情况，其中遇见最多的还是和网络有关的，自从 NFS V3/V4 版本之后，NFS 或者 OS 自身 BUG 导致的问题逐渐减少，而网络方面遇到的问题逐渐变多起来。特别是防火墙问题，最近五六年里，我遇到的大型系统中的 NFS 问题大部分和防火墙有关。随着企业对网络安全的要求越来越高，防火墙也不断地在调整策略，有时候有一条策略就会导致 NFS 的网络包被防火墙过滤，从而导致 NFS 的故障。五六年前我遇到过一个客户的 NFS 故障，只要访问一个文件 NFS 就会 HANG 死，访问其他文件就啥事没有。当时实在没办法了只能用网络抓包的方式去跟踪，最后发现客户最近上了一个敏感词过滤的策略，因为这个文件名带有敏感词汇，因此网络就丢包了。实际上 IP 存储专网建设十分重要，关键系统使用 NFS 的时候一定要使用 IP 存储专网，最好不要经过防火墙，不过很多大企业对于跨机房访问的网络安全管理十分变态，防火墙是必须要经过的，一个 NFS 访问经过至少两个防火墙，一旦网络安全管理员与存储管理员之间没有很好的沟通，那么因为防火墙导致的 NFS 问题那就少不了了。另外我们以前遇到的大多数 NFS 故障场景都没有使用 IPSAN 专网，NFS 使用的网络是和业务网混合使用的，业务网出现的任何问题，都会引起 NFS 文件系统的抖动，从而导致 NFS 服务不稳定。是否 Socket hang 的问题可以通过 netstat 命令来看是不是存在大量的 TCP 连接处于 CLOSEWAIT 状态，如果存在这种情况，那么很可能你遇到了 socket hang。Socket hang 大多数和 IO 负载过高、客户端或者服务器端 OS 相应慢或者某个 OS bug 有关。也可能和 TCP 的 keepalive 参数设置有关。另外一点是我们是用 NFS 的环境中，可能还安装了数据库服务器、中间件服务器等，这些系统要求调整 OS 的网络参数，而这些参数的调整很可能并不适合 NFS 服务。其中常见存在冲突的几个参数如下：
  
  *   net.ipv4.tcp_keepalive_time
      
  *   net.ipv4.tcp_keepalive_intvl
      
  *   net.ipv4.tcp_keepalive_probes
      
  *   net.ipv4.tcp_tw_recycle = 0
      
  *   net.ipv4.tcp_tw_reuse = 0
      
  *   net.ipv4.tcp_retries1 = 3
      
  *   net.ipv4.tcp_retries2 = 15
      
  
  实际上 NFS HANG 的问题分析因为涉及的原因很多，所以往往还是不容易定位的，不过如果 df 命令 hang，而且是可重现的，那么分析起来还是有方法的。最好的防范就是使用 strace 去跟踪 df 命令的堆栈。看看 hang 在什么地方，就比较容易定位问题了。最后要说明的是，messages 日志是一定要首要分析的，不管是客户端还是服务端的 messages 日志都应该尽早去看。虽然说很可能我们看到的只是 NFS 命令超时这类的比较笼统的信息。
  
  > 原题：NFS 为什么会 HANG，原因应该如何分析？
  > 
  > ****如有任何问题，可点击文末****阅读原文****，到社区提问****  
  > 
  > **觉得本文有用，请****转发****或点击** **“在看”****，让更多同行看到**
  
   资料 / 文章推荐：
  
  *   [企业 IT 运维事中故障定位方法及工具](http://mp.weixin.qq.com/s?__biz=MjM5NTk0MTM1Mw==&mid=2650674646&idx=1&sn=bff3c31d4f0b535bb7eac510a593cbac&chksm=befa6b50898de2465ecd2681666c1ea82a91155765d8394880649659101db7a90dcfdadfbb0e&scene=21#wechat_redirect)  
      
  *   [企业运维故障复盘步骤及改进方法](http://mp.weixin.qq.com/s?__biz=MjM5NTk0MTM1Mw==&mid=2650656564&idx=1&sn=9da561f0f2def4f958feefe756ad23ef&chksm=bef9b1b2898e38a4bb5bfe48dcd49f9ea9fd4e6eec59df9ffd57f4e82f9f7938c111259ba056&scene=21#wechat_redirect)   
      
  *   [IT 运维中的事件、故障排查处理思路](http://mp.weixin.qq.com/s?__biz=MjM5NTk0MTM1Mw==&mid=2650656584&idx=2&sn=d24c1001f0e042e9a0b2f58b077b2b9f&chksm=bef9b1ce898e38d86f8754f75dc4a051b719a3843c38fa1e2a9f20cd482eac8fcbcfb44a5d6c&scene=21#wechat_redirect)
      
  
  欢迎关注社区以下  **“运维” 技术主题** ，将会不断更新优质资料、文章。地址：https://www.talkwithtrend.com/Topic/4549
  
  **下载 twt 社区客户端 APP**
  
  ![](https://mmbiz.qpic.cn/mmbiz_png/icdBCpcEToIicEhGeibB7bXWzcpZw2W1MtYUE1nEjaJ9GUdRLWxKpriaKibjWLXeTMtgKOUib4dLibBWojKDpX5hIMkxQ/640?wx_fmt=png)  
  
  ![](https://mmbiz.qpic.cn/mmbiz_png/icdBCpcEToIicEhGeibB7bXWzcpZw2W1MtY4XZqa0GbU2yZxwDk8sqkNpnTBPlXPicSUlltBa8ODoeX6T6ftVicOdqg/640?wx_fmt=png)
  
  长按识别二维码即可下载
  
  或到应用商店搜索 “twt”
  
  长按二维码关注公众号
  
  ![](https://mmbiz.qpic.cn/mmbiz_jpg/icdBCpcEToI8sSChzbkLyCCuqNd1gwh4XY2JjYj0yicYY5cNjR2f44zRcISdax4mfAOAzLqrJSSQ5Dz8DV7Jx3uQ/640?wx_fmt=jpeg)
  
  * 本公众号所发布内容仅代表作者观点，不代表社区立场