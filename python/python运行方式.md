### python运行方式（推荐  ipython）

---



#### python解释器

CPython 官方版本的 c 语言实现

1. 解释器执行 python  文件

```
# 使用 python 2.x 解释器
python xxx.py

# 使用 python 3.x 解释器
python3 xxx.py
```

2. 交互式运行 python

   优点: 适合于学习/验证

   确定：代码不能保存、不适合运行大型程序

   退出：exit() 命令   ctrl + d 快捷键

3. IPython 交互方式运行python

   * 支持自动补全
   * 自动缩进
   * 支持 bash shell 命令
   * 内置许多有用的功能和函数
   * python 2.x 使用的解释器是ipython, python3.x 使用 的解释器是ipython3
   * 退出：1） exit命令，无括号 2）快捷键 ctrl + d