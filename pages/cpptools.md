- > 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.51cto.com](https://blog.51cto.com/knight02/5383530)
- > 清 C 盘, vscode-cpptools ipch 文件夹高达 10G，又到了日常扣 C 盘的环节 (系统缓存, qq,wx 等缓存都清完了但是 C 盘还是小得有点可怜) 持
- 又到了日常扣 C 盘的环节 (系统缓存, qq,wx 等缓存都清完了但是 C 盘还是小得有点可怜)
- 持续性竭泽而渔
- 在系统目录下发现了一个 vscode-cpptools, 其中的 ipch 竟然高达十几个 G
- **C:\Users\(你的用户名)\AppData\Local\Microsoft\vscode-cpptools**
- **查询官方文档**
  > ​
  > 
  > What is the ipch folder?
  > ------------------------
  > 
  > The language server caches information about included header files to improve the performance of IntelliSense. When you edit C/C++ files in your workspace folder, the language server will store cache files in the ​`​ipch​`​​ folder. By default, the ​`​ipch​`​​ folder is stored under the user directory. Specifically, it is stored under ​`​%LocalAppData%/Microsoft/vscode-cpptools​`​​ on Windows, and for Linux and macOS it is under ​`​~/.vscode-cpptools​`​. By using the user directory as the default path, it will create one cache location per user for the extension. As the cache size limit is applied to a cache location, having one cache location per user will limit the disk space usage of the cache to that one folder for everyone using the default setting value.
  > 
  > VS Code per-workspace storage folders were not used because the location provided by VS Code is not well known and we didn't want to write GB's of files where users may not see them or know where to find them.
  > 
  > With this in mind, we knew that we would not be able to meet the needs of every different development environment, so we provided settings to allow you to customize the way that works best for your situation.
  > 
  > ### ​`​"C_Cpp.intelliSenseCachePath": <string>​`​
  > 
  > This setting allows you to set workspace or global overrides for the cache path. For example, if you want to share a single cache location for all workspace folders, open the VS Code settings, and add a User setting for **IntelliSense Cache Path**.
  > 
  > ### ​`​"C_Cpp.intelliSenseCacheSize": <number>​`​
  > 
  > This setting allows you to set a limit on the amount of caching the extension does. This is an approximation, but the extension will make a best effort to keep the cache size as close to the limit you set as possible. If you are sharing the cache location across workspaces as explained above, you can still increase/decrease the limit, but you should make sure that you add a User setting for **IntelliSense Cache Size**.
  > 
  > How do I disable the IntelliSense cache (ipch)?
  > -----------------------------------------------
  > 
  > If you do not want to use the IntelliSense caching feature that improves the performance of IntelliSense, you can disable the feature by setting the **IntelliSense Cache Size** setting to 0 (or ​`​"C_Cpp.intelliSenseCacheSize": 0"​`​ in the JSON settings editor).
- 简而言之这是**智能感知缓存的路径**, 编辑 C/C++ 文件时，vscode 的语言服务会将缓存文件存储在该文件夹中 (每编译一次都对应着 ipch 里的一个文件夹)
- 在 vscode 的设置中搜索 C_Cpp.intelliSenseCache, 把目录改到其他盘, 而在 c 盘中的缓存可以直接删除 (无异常情况)
- > 如果不想使用智能感知缓存功能，可以通过将缓存大小设置设置为 0（或在 JSON 设置编辑器中）来禁用该功能。​`​"C_Cpp.intelliSenseCacheSize": 0"​`​
- ![](https://s2.51cto.com/images/blog/202206/13113638_62a6b0c6947af69061.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_30,g_se,x_10,y_10,shadow_20,type_ZmFuZ3poZW5naGVpdGk=/format,webp/resize,m_fixed,w_1184)