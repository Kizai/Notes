# Python logging模块用法介绍

## 基本功能介绍
- Python logging 模块定义了为应用程序和库实现灵活的事件日志记录的函数和类。
- 程序开发过程中，很多程序都有记录日志的需求，并且日志包含的信息有正常的程序访问日志还可能有错误、警告等信息输出，Python 的 logging 模块提供了标准的日志接口，可以通过它存储各种格式的日志,日志记录提供了一组便利功能，用于简单的日志记录用法。
  - 使用 Python Logging 模块的主要好处是所有 Python 模块都可以参与日志记录
  - Logging 模块提供了大量具有灵活性的功能

**日志记录函数以它们用来跟踪的事件的级别或严重性命名。下面描述了标准级别及其适用性（从高到低的顺序）：**

| 日志等级(level) |                                            描述                                             |
| :-------------: | :-----------------------------------------------------------------------------------------: |
|      DEBUG      |                          最详细的日志信息，典型应用场景是 问题诊断                          |
|      INFO       | 信息详细程度仅次于DEBUG，通常只记录关键节点信息，用于确认一切都是按照我们预期的那样进行工作 |
|     WARNING     | 当某些不期望的事情发生时记录的信息（如，磁盘可用空间较低），但是此时应用程序还是正常运行的  |
|      ERROR      |                  由于一个更严重的问题导致某些功能不能正常运行时记录的信息                   |
|    CRITICAL     |                    当发生严重错误，导致应用程序不能继续运行时记录的信息                     |

**日志级别等级排序(从高到低)**：critical > error > warning > info > debug

**级别越高打印的日志越少，反之亦然，即**
- debug : 打印全部的日志( notset 等同于 debug )
- info : 打印 info, warning, error, critical 级别的日志
- warning : 打印 warning, error, critical 级别的日志
- error : 打印 error, critical 级别的日志
- critical : 打印 critical 级别

## Logging 模块日志记录方式
Logging 模块提供了两种日志记录方式：
  - 一种方式是使用 Logging 提供的模块级别的函数
  - 另一种方式是使用 Logging 日志系统的四大组件记录

### Logging 定义的模块级别函数

| 函数                                   | 说明                                 |
| :------------------------------------- | :----------------------------------- |
| logging.debug(msg, *args, **kwargs)    | 创建一条严重级别为DEBUG的日志记录    |
| logging.info(msg, *args, **kwargs)     | 创建一条严重级别为INFO的日志记录     |
| logging.warning(msg, *args, **kwargs)  | 创建一条严重级别为WARNING的日志记录  |
| logging.error(msg, *args, **kwargs)    | 创建一条严重级别为ERROR的日志记录    |
| logging.critical(msg, *args, **kwargs) | 创建一条严重级别为CRITICAL的日志记录 |
| logging.log(level, *args, **kwargs)    | 创建一条严重级别为level的日志记录    |
| logging.basicConfig(**kwargs)          | 对root logger进行一次性配置          |

**上面的参数是**：```msg``` 是消息格式字符串，而 ```args``` 是用于字符串格式化操作合并到```msg``` 的参数。（请注意，这意味着您可以在格式字符串中使用关键字以及单个字典参数。）当未提供 ```args``` 时，不会对 ```msg``` 执行 ```％``` 格式化操作。

在 ```kwargs``` 中会检查四个关键字参数： ```exc_info``` ，```stack_info``` ，```stacklevel``` 和 ```extra``` 。
[more](https://docs.python.org/zh-cn/3/library/logging.html)

#### 打印日志的例子：
```py
import logging

#　打印日志级别
def test_logging():
    logging.debug('Python debug')
    logging.info('Python info')
    logging.warning('Python warning')
    logging.error('Python Error')
    logging.critical('Python critical')

test_logging()
```
输出结果：
```sh
WARNING:root:Python warning
ERROR:root:Python Error
CRITICAL:root:Python critical
```
- 问题：为什么没有全部把log输出到终端？
- 原因：因为logging默认的级别是Warning,所以低于Warning的日志并不会输出到终端上，而等于或者大于Warning的日志就会打印出来。

### 设置日志显示级别
通过 logging.basicConfig() 可以设置 root 的日志级别，和日志输出格式。

**logging.basicConfig() 关键字参数：**

| 关键字   | 描述                                                                                            |
| :------- | :---------------------------------------------------------------------------------------------- |
| filename | 创建一个 FileHandler，使用指定的文件名，而不是使用 StreamHandler。                              |
| filemode | 如果指明了文件名，指明打开文件的模式（如果没有指明 filemode，默认为 ‘a’）。                     |
| format   | handler 使用指明的格式化字符串。                                                                |
| datefmt  | handler 使用指明的格式化字符串。                                                                |
| level    | 指明根 logger 的级别。                                                                          |
| stream   | 使用指明的流来初始化 StreamHandler。该参数与 ‘filename’ 不兼容，如果两个都有，’stream’ 被忽略。 |

**format 格式**

| 格式           | 描述                   |
| :------------- | :--------------------- |
| %(levelno)s    | 打印日志级别的数值     |
| %(levelname)s  | 打印日志级别名称       |
| %(pathname)s   | 打印当前执行程序的路径 |
| %(filename)s   | 打印当前执行程序名称   |
| %(funcName)s   | 打印日志的当前函数     |
| %(lineno)d     | 打印日志的当前行号     |
| %(asctime)s    | 打印日志的时间         |
| %(thread)d     | 打印线程 ID            |
| %(threadName)s | 打印线程名称           |
| %(process)d    | 打印进程 ID            |
| %(message)s    | 打印日志信息           |

**注意：Logging.basicConfig() 需要在开头就设置，在中间设置并无作用**

#### 实例
```py
import logging

#　打印日志级别
def test():
    logging.basicConfig(level=logging.DEBUG)
    logging.debug('Python debug')
    logging.info('Python info')
    logging.warning('Python warning')
    logging.error('Python Error')
    logging.critical('Python critical')
    logging.log(2,'test')
test()
```
**输出:**
```py
DEBUG:root:Python debug
INFO:root:Python info
WARNING:root:Python warning
ERROR:root:Python Error
CRITICAL:root:Python critical
```
可以看到输出log全部都输出到终端了。

### 将日志信息记录到文件
#### 实例
```py
import logging

# 日志信息记录到文件,这时日志不会输出到终端上
logging.basicConfig(filename='example.log', level=logging.DEBUG)
logging.debug('This message should go to the log file')
logging.info('So should this')
logging.warning('And this, too')
```
运行上面的代码，可以看到根目录下有个example.log的文件，文件内容如下：
```txt
DEBUG:root:This message should go to the log file
INFO:root:So should this
WARNING:root:And this, too
```

#### 多个模块记录日志信息
如果程序包含多个模块，则用以下实例来显示日志信息： 实例中有两个模块，一个模块通过导入另一个模块的方式用日志显示另一个模块的信息：

**模块**
```py
import logging
import mylib
def main():
    logging.basicConfig(filename='log/myapp.log',level=logging.DEBUG)
    logging.info('Started')
    mylib.do_something()
    logging.info('Finished')

if __name__ == '__main__':
    main()
```

**[mylib.py](../mylib.py) 模块**
```py
import logging

def do_something():
    logging.info('Doing something')
```

执行 myapp.py 模块会打印相应的日志，在文件 log/myapp.log 中显示信息如下：
```txt
INFO:root:Started
INFO:root:Doing something
INFO:root:Finishe
```

#### 显示信息的日期及更改显示消息格式
```py
import logging
# 显示消息时间
logging.basicConfig(format='%(asctime)s %(message)s')
logging.warning('is when this event was logged.')

logging.basicConfig(format='%(asctime)s %(message)s', datefmt='%m/%d/%Y %I:%M:%S %p')
logging.warning('is when this event was logged.')
```
结果：
```log
2019-10-16 18:57:45,988 is when this event was logged.
2019-10-16 18:57:45,988 is when this event was logged
```