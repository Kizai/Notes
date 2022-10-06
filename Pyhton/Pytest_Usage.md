# Pytest 测试框架用法

- 更新时间：2022-05-05
- 版本号：1.0.0

- 版本更新内容：
  - 1.0.0：学习Pytest测试框架的使用

### 创建笔记的原因
为了快速学习pytest单元测试框架的用法，方便对接口进行统一的单元测试。

### 一、环境安装：
pytest 是 python 中的第三方库，使用之前需要先安装，在命令行中运行以下安装命令 ：
```sh
$ pip install pytest
```
检查安装是否成功以及安装的版本，命令行命令如下：
```sh
$ pytest --version
```
执行上述命令，能够输出版本信息，那就说明安装成功啦。

### 二、Python 测试发现的约定
pytest实现以下标准测试发现：
- 如果未指定参数，则收集从```testpaths``` （如果已配置）或当前目录开始。或者，命令行参数可用于目录、文件名或节点 ID 的任意组合。
- 递归到目录，除非它们匹配```norecursedirs```。
- 在这些目录中，搜索```test_*.py```或文件，由它们的[测试包名称](https://docs.pytest.org/en/latest/explanation/goodpractices.html#test-package-name)```*_test.py```导入。
- 从这些文件中，收集测试项目：
  - ```test```类外的前缀测试函数或方法
  - ```test```前缀测试类中的前缀测试函数或方法Test（没有__init__方法）
- 有关如何自定义测试发现[更改标准 (Python) 测试发现](https://docs.pytest.org/en/latest/example/pythoncollection.html)的示例。
- 在 Python 模块中，pytest还使用标准[unittest.TestCase](https://docs.pytest.org/en/latest/how-to/unittest.html#unittest-testcase)子类化技术发现测试。

### 三、推荐使用的目录结构
```
/rootdir 
  /src 
    /mypkg
      api.py 
      constants.py 
      manager.py 
      models.py 
      tasks.py 
  /tests 
    /integ_tests  
        test_manager.py 
    /unit_tests 
        test_manager.py 
requirements.py 
application.py
```
如何运行这些测试？
```powershell
python -m pytest tests/（所有测试）
python -m pytest -k filenamekeyword（测试匹配关键字）
python -m pytest tests/utils/test_sample.py（单个测试文件）
python -m pytest tests/utils/test_sample.py： :test_answer_correct (单一测试方法) 
python -m pytest --resultlog=testlog.log tests/ (日志输出到文件) 
python -m pytest -s tests/ (打印输出到控制台)
```
### 四、编写测试样例
现在的目录结构：
```
├── application.py
├── src
└── tests
    ├── integ_tests
    └── unit_tests
        └── test_application.py
```
application.py
```python
from time import sleep  

def is_windows():    
    # This sleep could be some complex operation instead
    sleep(5)    
    return False  
def get_operating_system():    
    return 'Windows' if is_windows() else 'Linux'
```
test_application.py
```python
from application import get_operating_system

def test_get_operating_system():
    assert get_operating_system() == 'Linux'
```
运行单元测试，成功的案例：
```powershell
$ python -m pytest tests/
================= test session starts =================
platform linux -- Python 3.8.10, pytest-7.1.1, pluggy-1.0.0
rootdir: /home/lemu-devops/Temporary/Pytest
plugins: html-3.1.1, mock-3.7.0, metadata-2.0.1
collected 1 item                                      

tests/unit_tests/test_application.py .（这个.代表测试成功，测试通过）          [100%]

================== 1 passed in 5.01s ==================
```
修改测试代码：
test_application.py
```python
from application import get_operating_system

def test_get_operating_system():
    assert get_operating_system() == 'Windows'
```
运行单元测试，失败的案例：
```powershell
$ python -m pytest tests/
========================= test session starts =========================
platform linux -- Python 3.8.10, pytest-7.1.1, pluggy-1.0.0
rootdir: /home/lemu-devops/Temporary/Pytest
plugins: html-3.1.1, mock-3.7.0, metadata-2.0.1
collected 1 item                                                      

tests/unit_tests/test_application.py F（F代表测试失败，不通过）                          [100%]

============================== FAILURES ===============================
______________________ test_get_operating_system ______________________

    def test_get_operating_system():
>       assert get_operating_system() == 'Windows'
E       AssertionError: assert 'Linux' == 'Windows'
E         - Windows
E         + Linux

tests/unit_tests/test_application.py:4: AssertionError
======================= short test summary info =======================
FAILED tests/unit_tests/test_application.py::test_get_operating_system
========================== 1 failed in 5.06s ==========================
```