title:: etc/profile

- 系统每个用户设置环境信息，当用户第一次登录时，该文件被执行。是系统全局针对终端环境的设置，login时最先被系统加载，调用/etc/bashrc以及/etc/profile.d目录下的sh。如果一个软件包在系统上只安装一份，供所有开发者使用，建议在/etc/profile.d下创建一个新的xxx.sh来配置环境变量。