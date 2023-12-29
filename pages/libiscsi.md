- [Installation](https://github.com/gostor/libiscsi)
	- ./autogen.sh
		- ```
		  yangzh22@HY03-WX01-1350:libiscsi$ ./autogen.sh
		  autoreconf: not found
		  A required command is missing. Unable to continue.
		  yangzh22@HY03-WX01-1350:libiscsi$ sudo apt install autoconf
		  
		  lib/Makefile.am:35: error: Libtool library used but 'LIBTOOL' is undefined
		  lib/Makefile.am:35:   The usual way to define 'LIBTOOL' is to add 'LT_INIT'
		  lib/Makefile.am:35:   to 'configure.ac' and run 'aclocal' and 'autoconf' again.
		  lib/Makefile.am:35:   If 'LT_INIT' is in 'configure.ac', make sure
		  lib/Makefile.am:35:   its definition is in aclocal's search path.
		  autoreconf: error: automake failed with exit status: 1
		  yangzh22@HY03-WX01-1350:libiscsi$ sudo apt install libtool
		  ```
	- ./configure
	- make
	- sudo make install
	- iscsi-test-cu
		- ```
		  iscsi-test-cu is a CUnit based test tool for scsi and iscsi.
		  
		  iscsi-test-cu depends on the CUnit library and will only build if libcunit can be found during configure.
		  yangzh22@HY03-WX01-1350:libiscsi$ sudo apt install libcunit1 libcunit1-doc libcunit1-dev
		  ```
- ### iSCSI URL Format
	- ```
	  iSCSI devices are specified by a URL format of the following form :
	  iscsi://[<username>[%<password>]@]<host>[:<port>]/<target-iqn>/<lun>[?<argument>[&<argument>]*]
	  or
	  iser://[<username>[%<password>]@]<host>[:<port>]/<target-iqn>/<lun>[?<argument>[&<argument>]*]
	  ```
- ### ReadCapacity
	- ```
	  [root@host-10-142-144-87 ~]# yum list installed|grep iscsi
	  libiscsi.x86_64                      1.9.0-7.el7                    @base
	  libiscsi-devel.x86_64                1.9.0-7.el7                    @base
	  [root@host-10-142-144-87 ~]# uname -a
	  Linux host-10-142-144-87 3.10.0-1160.el7.x86_64 #1 SMP Mon Oct 19 16:18:59 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
	  
	  
	  func getReadCapacity(task C.struct_scsi_task) (C.struct_scsi_readcapacity10, error) {
	  	cap := C.struct_scsi_readcapacity10{}
	  
	  	if task.cdb[0] != C.SCSI_OPCODE_READCAPACITY10 {
	  		return cap, errors.New("unexpected opcode")
	  	}
	  	// struct_scsi_readcapacity16 instead
	      // if task.datain.size != 8 {
	  	// 	return cap, errors.New("unexpected size")
	  	// }
	  
	  	dataread := unsafe.Slice(task.datain.data, task.datain.size)
	  	databytes := []byte(string(dataread))
	  	cap.lba = C.uint(binary.BigEndian.Uint32(databytes[:4]))
	  	cap.block_size = C.uint(binary.BigEndian.Uint32(databytes[4:]))
	  	return cap, nil
	  }
	  
	  [root@host-10-142-144-87 libiscsi-go]# go run main.go
	  2023/12/25 14:51:01 conDetails:  {iqn.1994-05.com.xxx:qbsn3it6lotb iscsi://10.10.13.9/iqn.2010-01.com.example:CloudBackup/2}
	  2023/12/25 14:51:01 device:  {0x1dc1a30 iqn.2010-01.com.example:CloudBackup 10.10.13.9 2}
	  2023/12/25 14:51:05 task: &{status:2 cdb_size:10 xfer_dir:1 expxferlen:8 cdb:[37 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0] residual_status:0 residual:0 sense:{error_type:112 key:6 ascq:10496} datain:{size:20 data:0x1dc2650} mem:<nil> ptr:0x1dc2ca8 itt:3 cmdsn:2 lun:0 iovector_in:{iov:<nil> niov:0 nalloc:0 offset:0 consumed:0 _:[0 0 0 0]} iovector_out:{iov:<nil> niov:0 nalloc:0 offset:0 consumed:0 _:[0 0 0 0]}}
	  ```