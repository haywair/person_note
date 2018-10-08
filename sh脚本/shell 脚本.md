### shell 脚本（变量）

----

变量 

```
bak=hello
echo $bak
echo ${bak}
```

​	

----

只读变量

```shell
baseurl=wwwroot
readonly baseurl    // 设置变量只读属性
baseurl=rootwww     // 报错，只读变量无法重新赋值
```

----

删除变量

```shell
baseurl=helloworl
echo $baseurl
unset baseurl   // 删除变量
echo $baseurl   // 无任何输出 

```

----







### shell脚本 字符串

#### 单引号

* 单引号里的任何字符都会远洋输出，单引号字符串中的变量是无效的 
* 单引号字符串中不能出现一个的 单引号，但可以成对出现，作为拼接字符 使用

#### 双引号

* 双引号里可以有变量
* 双引号里可以出现转义字符 



for 循环示例

* 列出所有的 etc 文件

```
for file in $(ls /etc); do
	echo $file
done    
```

