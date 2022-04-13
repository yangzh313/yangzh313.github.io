# HttpRunner支持并发执行

## Pytest多进程并发执行

[Pytest(十六)多进程并发执行](https://cloud.tencent.com/developer/article/1972491)

安装pytest-xdist

```Shell
# pip install pytest-xdist
```

-n 指定并发数

```Shell
pytest test_11.py -n 2 -s
```

## HttpRunner并发

指定2个测试用例

```Python
@pytest.mark.parametrize("param", [{
        "resourceMeta": info_list[1]
    },{
        "resourceMeta": info_list[3]
    }])
    def test_start(self, param):
        super().test_start(param)
```

发起2个并发的任务

```Shell
./hrun -n 2 --alluredir=/root/allure-server/allure-results/--capture=tee-sys testcases/v1.4/case_PERFORMANCE_7_multi_backup_restore_test.py
```

结果

```Shell
2022-04-13 09:49:36.164 | INFO     | httprunner.make:__make:515 - make path: /root/zy/stability_test/testcases/v1.4/case_PERFORMANCE_7_multi_backup_restore_test.py
2022-04-13 09:49:36.164 | INFO     | httprunner.make:format_pytest_with_black:170 - format pytest cases with black ...
Usage: black [OPTIONS] SRC ...

One of 'SRC' or 'code' is required.
2022-04-13 09:49:36.364 | INFO     | httprunner.cli:main_run:56 - start to run tests with pytest. HttpRunner version: 3.1.4
======================================= test session starts =======================================
platform linux -- Python 3.6.8, pytest-6.2.1, py-1.10.0, pluggy-0.13.1
rootdir: /root/zy/stability_test
plugins: allure-pytest-2.8.29, forked-1.4.0, xdist-2.5.0
gw0 [2] / gw1 [2]
```
