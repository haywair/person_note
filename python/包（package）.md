### 包（package）

----

* 概念

  1. 包是一个 包含多个模块的 特殊目录

  2. 目录下 有一个特殊的文件 

     ```
     __init__.py
     ```

  3. 包名的命名方式和 变量名一致，小写字母 加下划线

* pycharm 中创建包的方式

  ```
  1. pycharm 在项目中点击新建 --》directory --> 手动创建 __init.py
  
  2. pycharm 在项目中点击新建 --》python package --------> pycharm自动创建包文件夹和__init__.py文件
  ```


```
# __init__.py文件中创建导入包列表及其它格式
# from + 点符号 + 模块文件名

from . import send_message
from . import receive_message
```

* 制作发布压缩包步骤

  1. 项目根目录下创建 setup.py

     ```python
     from distutils.core import setup
     setup(name='mylib',
     	version='1.0',
     	py_modules=['mylib'],  # py_modules中填写需要发布的模块文件列表
     )	
     ```

  2. 构建模块

     ```
     python3 setup.py build
     ```

  3. 生成发布压缩包

     ```
     python3 setup.py sdist
     ```



* 安装包

  ```
  1.  tar -zxvf hm_message-1.0.tar.gz
  2. sudo python3 setup.py install
  ```

* 卸载包

```
* 查看包文件目录文件地址  包文件名.__file__
* 使用  rm -r 命令删除需要卸载的包文件就可以了
```



* pip 安装第三方模块 pip pip3

  ```
  sudo pip install 模块名
  ```
