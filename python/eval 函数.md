### eval 函数

---

eval() --------- 将字符串当成有效的 表达式来求值并返回计算结果

```
# 基本的数学计算
In[1]: eval('1 + 1')
out[1]: 2

# 字符串重复
In[2]: eval("'*' * 10")
out[2]: **********

# 将字符串转化成列表
In[3]: type(eval("[1,2,3,4,5]"))
out[3]: list

# 将字符串转化成字典
In[4]: type(eval("{'name':'xioaming','age':18}"))
out[4]: dict




```



eval 的注意事项

在开发时千万不要直接使用 eval 转化 input 输出结果

