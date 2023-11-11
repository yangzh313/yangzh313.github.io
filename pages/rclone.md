category:: [[program]]

- [Microsoft OneDrive (rclone.org)](https://rclone.org/onedrive/)
- [rclone mount](https://rclone.org/commands/rclone_mount/)
- [mount on boot](https://www.tofzl.com/index.php/archives/20/)
	- ```
	  yangzh22@HY03-WX01-1350:~$ sudo cat /etc/systemd/system/rclone.service                          [Unit]
	  Description=Rclone
	  AssertPathIsDirectory=/mnt/onedirve/code
	  After=network-online.target
	  
	  [Service]
	  Type=simple
	  ExecStart=rclone mount onedrive:Resources/code /mnt/onedirve/code --vfs-cache-mode full
	  ExecStop=fusermount -u /mnt/onedirve/code
	  Restart=on-abort
	  User=yangzh22
	  
	  [Install]
	  WantedBy=default.target
	  
	  
	  yangzh22@HY03-WX01-1350:~$ sudo systemctl start rclone
	  yangzh22@HY03-WX01-1350:~$ sudo systemctl status rclone
	  ● rclone.service - Rclone
	       Loaded: loaded (/etc/systemd/system/rclone.service; enabled; vendor preset: enabled)
	       Active: active (running) since Wed 2023-11-01 17:36:23 CST; 4s ago
	     Main PID: 5213 (rclone)
	        Tasks: 8 (limit: 4697)
	       Memory: 20.4M
	       CGroup: /system.slice/rclone.service
	               └─5213 rclone mount onedrive:Resources/code /mnt/onedirve/code --vfs-cache-mode fu>
	  
	  Nov 01 17:36:23 HY03-WX01-1350 systemd[1]: Started Rclone.
	  
	  
	  yangzh22@HY03-WX01-1350:~$ df -h
	  Filesystem               Size  Used Avail Use% Mounted on
	  onedrive:Resources/code  1.1T   87G  993G   9% /mnt/onedirve/code
	  
	  ```
-