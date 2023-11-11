title:: NFS shares are mounted as "nobody" | TrueNAS Community

- > 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [www.truenas.com](https://www.truenas.com/community/threads/nfs-shares-are-mounted-as-nobody.44736/)
  
  > Dear all, we need to mount a NFS partition on a cPanel system in order to store backups. We have an ......
  
  *   [#1](https://www.truenas.com/community/threads/nfs-shares-are-mounted-as-nobody.44736/post-300916)
  
  Dear all,
  
  we need to mount a NFS partition on a cPanel system in order to store backups.  
  We have an issue with permission because all data on the NFS partition are reset to "nobody" user.  
  Because of this setting cPanel create a backup with partial failure status (due to permissions).
  
  On my older NFS storage server i used to just apply the flag "no_root_squash" and mount it with noexec options. But i cannot replicate this behaviour on FREENAS.
  
  Can somebody help me to re-config the server in order to have right permission on the client filesystem.
  
  Thanks!
  
   [![](https://www.truenas.com/community/data/avatars/m/0/115.jpg?1451165056)](https://www.truenas.com/community/members/jgreco.115/) 
  
  *   [#3](https://www.truenas.com/community/threads/nfs-shares-are-mounted-as-nobody.44736/post-301382)
  
  So you've set the maproot user to root, and the maproot group to wheel? Or not? If not, go into the advanced options for the share and set it that way. Root by default gets mapped to nobody because root is relatively powerful.
  
  *   [#4](https://www.truenas.com/community/threads/nfs-shares-are-mounted-as-nobody.44736/post-301403)
  
  I've set mapall user to root and mapall group to wheel because only root can access to this system.  
  Then I just need to map root as root.
  
  .... i'm gonna to try your solutions.  
  Maybe it was just that easy! :)
  
  *   [#5](https://www.truenas.com/community/threads/nfs-shares-are-mounted-as-nobody.44736/post-301408)
  
  .... No  
  try to move from "mapall" to "maproot" and get the same result.
  
  /backup is mounted as nobody:nobody
  
   [![](https://www.truenas.com/community/data/avatars/m/0/115.jpg?1451165056)](https://www.truenas.com/community/members/jgreco.115/) 
  
  *   [#6](https://www.truenas.com/community/threads/nfs-shares-are-mounted-as-nobody.44736/post-301436)
  
  If mapall doesn't work, it isn't clear to me what the actual issue here is. mapall is the "big gun".
  
  *   [#7](https://www.truenas.com/community/threads/nfs-shares-are-mounted-as-nobody.44736/post-301439)
  
  Everything seems fine on the server.  
  But when I mount the NFS partition on the client I see it as "nobody:nobody" on clientside.  
  I can use it read/write, but I need that the client sees those file as "root:root" and not "nobody:nobody".  
  Access to that directory must be root only.
  
   [![](https://secure.gravatar.com/avatar/76736d1e8aea3569a85856f35635bc5a?s=96)](https://www.truenas.com/community/members/nello.27416/) 
  
  *   [#8](https://www.truenas.com/community/threads/nfs-shares-are-mounted-as-nobody.44736/post-306292)
  
  > Everything seems fine on the server.  
  > But when I mount the NFS partition on the client I see it as "nobody:nobody" on clientside.  
  > I can use it read/write, but I need that the client sees those file as "root:root" and not "nobody:nobody".  
  > Access to that directory must be root only.
  
  Did you figure out how to do this?
  
   [![](https://www.truenas.com/community/data/avatars/m/63/63940.jpg?1475152207)](https://www.truenas.com/community/members/martin-cheatle.63940/) 
  
  *   [#9](https://www.truenas.com/community/threads/nfs-shares-are-mounted-as-nobody.44736/post-316720)
  
  Most likely you have configured the NFS service to enable NFSv4 and you have a different domain on your server and client. If you don't need v4 turn that feature off in the services sectiion and restart the NFS deamon. You should then be able to mount fine form any v3 client.
  
   [![](https://secure.gravatar.com/avatar/088279b24967aeab01cc54351c2d455b?s=96)](https://www.truenas.com/community/members/bmoreitdan.90100/) 
  
  *   [#10](https://www.truenas.com/community/threads/nfs-shares-are-mounted-as-nobody.44736/post-500954)
  
  This thread lead me to find the answer needed for this issue with 11.2-RELEASE.
  
  In the Services > NFS > there's an option called"NFSv3 ownership model for NFSv4" which allows me to establish a v4 connection but effectively have the no_root_squash functionality. Also, I needed to mapall to root:wheel.
  
  *   [#11](https://www.truenas.com/community/threads/nfs-shares-are-mounted-as-nobody.44736/post-554006)
  
  Same here with 11.2.u6. I had the same files shared smb rw for windows users and nfs ro for linux users and while root user on linux would be able to access it, non-root user wouldn't be able to read the folder at all. What bmoreitdan did is what I ended up with to make it accessable.
  
  *   [#12](https://www.truenas.com/community/threads/nfs-shares-are-mounted-as-nobody.44736/post-655854)
  
  > In the Services > NFS > there's an option called"NFSv3 ownership model for NFSv4" which allows me to establish a v4 connection but effectively have the no_root_squash functionality. Also, I needed to mapall to root:wheel.
  
  Just wanted to say in 2021 that this is what I needed to use TrueNAS for my NFS4 mounts.
  
  I host my container persistent storage on a general purpose Linux server using NFS4, and it was necessary to get "rw,no_root_squash,sync,no_subtree_check" like functionality in order for my container workloads to transition to TrueNAS backed storage.
  
  So far so good.
-