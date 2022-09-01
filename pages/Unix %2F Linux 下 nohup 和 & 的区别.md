title:: Unix / Linux 下 nohup 和 & 的区别

- [Unix / Linux 下 nohup 和 & 的区别 - 简单教程，简单编程 (twle.cn)](https://www.twle.cn/t/332#:~:text=%E5%8C%BA%E5%88%AB%EF%BC%9F%20%26%20%E5%92%8C%20nohup%20%E9%83%BD%E8%83%BD%E6%8A%8A%E4%B8%80%E4%B8%AA%E4%BB%BB%E5%8A%A1%E6%94%BE%E5%9C%A8%E5%90%8E%E5%8F%B0%E8%BF%90%E8%A1%8C%EF%BC%8C%E4%BD%86,%26%20%E5%91%BD%E4%BB%A4%E4%BC%9A%E9%9A%8F%E7%9D%80%E9%80%80%E5%87%BA%E8%BF%9C%E7%A8%8B%20Shell%20%E8%80%8C%E8%87%AA%E5%8A%A8%E5%81%9C%E6%AD%A2%E4%BB%BB%E5%8A%A1%EF%BC%8C%E8%80%8C%20nohup%20%E5%88%99%E4%BC%9A%E4%B8%80%E7%9B%B4%E7%BB%A7%E7%BB%AD%E8%BF%90%E8%A1%8C%EF%BC%8C%E9%99%A4%E9%9D%9E%E6%98%BE%E7%A4%BA%E6%9D%80%E6%8E%89%E6%88%96%E8%80%85%E9%87%8D%E5%90%AF%E7%94%B5%E8%84%91)
- 信号：
	- SIGINT：发送给前台进程组中的所有进程。常用于终止正在运行的程序，一般由CTRL+C触发。
	- SIGTSTP：发送给前台进程组中的所有进程，常用于挂起并暂停一个进程，一般由CTRL+Z触发。
	- SIGHUP：当用户退出Shell时，由该Shell开启的所有进程都会接收到此信号，默认动作为终止进程。
- &：
	- 给任何命令加上&会让该命令在后台执行
	- 返回一个进程号和任务ID，jobs命令查看所有后台任务
	- 关闭Shell会关闭后台任务
- nohup：
	- nohup并不会把程序放到后台运行
	- 真正作用是让程序忽略SIGHUP信号，前提是进程首先是在后台运行的，而不是暂停或者挂起的
- nohup和&：
	- 将任务放到后台运行并忽略SIGHUP信号