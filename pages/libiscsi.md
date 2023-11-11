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
-
-