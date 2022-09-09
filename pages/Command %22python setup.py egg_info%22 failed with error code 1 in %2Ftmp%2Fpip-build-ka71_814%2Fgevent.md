title:: Command "python setup.py egg_info" failed with error code 1 in /tmp/pip-build-ka71_814/gevent

- 卸载所有包
	- pip3 freeze > requirements.txt
	- pip3 uninstall -r requirements.txt
- 重新安装
	- pip3 install -r requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple （原本的requirements.txt）
	- 报错
	  ```
	  src/gevent/libev/corecext.c:95:10: fatal error: Python.h: No such file or directory
	       #include "Python.h"
	                ^~~~~~~~~~
	      compilation terminated.
	      error: command 'gcc' failed with exit status 1
	  
	      ----------------------------------------
	  Command "/usr/bin/python3.6 -u -c "import setuptools, tokenize;__file__='/tmp/pip-build-w438_6w1/gevent/setup.py';f=getattr(tokenize, 'open', open)(__file__);code=f.read().replace('\r\n', '\n');f.close();exec(compile(code, __file__, 'exec'))" install --record /tmp/pip-34n3ea3u-record/install-record.txt --single-version-externally-managed --compile" failed with error code 1 in /tmp/pip-build-w438_6w1/gevent/
	  
	  ```
- 安装python3-devel
	- yum install python3-devel
- 再次安装成功
	- pip3 install -r requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple