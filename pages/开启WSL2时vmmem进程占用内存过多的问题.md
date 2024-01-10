- > 
  
  该问题目前似乎并没有完美的的解决方案，但有缓解的办法。
  
  关于该问题的进一步讨论请参考https://github.com/microsoft/WSL/issues/4166
- ### 缓解方案
  
  **一、限制VM的内存使用**
- 按下**Windows** + **R** 键，输入 **%UserProfile%** 并运行进入用户文件夹
- 新建文件 **.wslconfig** ，然后使用记事本编辑
- 填入以下内容并保存, memory为wsl2分配的内存上限，可根据自身电脑配置设置
  
  ```
  [wsl2]
  memory=2GB  # Limits VM memory in WSL 2GB, also can be set to other values
  swap=0
  localhostForwarding=true
  processors=2 # Makes the WSL 2 VM use two virtual processors, also can be set to other values
  ```
  
  设置该文件并**重新启动WSL**后，不管vmmem内存使用情况如何，仍然会消耗掉限额的内存，但至少它不会再继续增长了，也可以设置为其他值，如512MB、1GB等，即可以将其控制在某个范围之内。
  
  **二、关掉WL2 VM**
  
  在不使用WSL2时，在PowerShell执行`wsl --shutdown`，从而关掉WL2 VM。
  
  <!-- notionvc: a308210c-19c3-403e-ab30-85e7cdd4003b -->