- STANDARD：
  ```
  [root@archcnstcm4380 truenas_share]# dd if=/dev/urandom of=test.txt bs=4096 count=1000000
  1000000+0 records in
  1000000+0 records out
  4096000000 bytes (4.1 GB) copied, 54.7376 s, 74.8 MB/s
  ```
- ALWAYS:
  ```
  [root@archcnstcm4380 truenas_share]# dd if=/dev/urandom of=test.txt bs=4096 count=1000000
  1000000+0 records in
  1000000+0 records out
  4096000000 bytes (4.1 GB) copied, 51.8944 s, 78.9 MB/s
  ```
- DISABLED:
  ```
  root@archcnstcm4380 truenas_share]# dd if=/dev/urandom of=test.txt bs=4096 count=1000000
  1000000+0 records in
  1000000+0 records out
  4096000000 bytes (4.1 GB) copied, 50.7063 s, 80.8 MB/s
  ```