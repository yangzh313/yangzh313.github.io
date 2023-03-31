- References:
	- [Testing I/O Performance with Sysbench - Alibaba Cloud Community](https://www.alibabacloud.com/blog/testing-io-performance-with-sysbench_594709)
	- [How to Benchmark Performance of MySQL Using SysBench - IT Tutorial](https://ittutorial.org/how-to-benchmark-performance-of-mysql-using-sysbench/#:~:text=Sysbench%20is%20a%20multi-threaded%20benchmark%20tool%20based%20on,actual%20impact%20of%20load%20on%20our%20MySQL%20server.)
- Installation
	- ```
	  curl -s https://packagecloud.io/install/repositories/akopytov/sysbench/script.rpm.sh | sudo bash
	  yum -y install sysbench
	  ```
- 常用操作：
	- mysql
	  ```
	  sysbench /usr/share/sysbench/oltp_insert.lua --mysql-host=127.0.0.1 --mysql-port=3307 --mysql-user=root --mysql-password='' --mysql-db=sbtest --db-driver=mysql --tables=100 --table-size=43700000 --report-interval=30 --threads=10 --time=30000 prepare run cleanup
	  ```
		-