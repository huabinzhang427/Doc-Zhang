Python-四舍五入

```python
# x = 3.14159 
# >>> 3 (not 3.0)
# x = 27.63 
# >>> 28 (not 28.0)
# x = 3.5 
# >>> 4 (not 4.0)

x = 3.14159

#ENTER CODE BELOW HERE
x = x + 0.5
x_str = str(x)
x_point = x_str.find('.')
print x_str[:x_point]
```

数字四舍五入处理 `x+0.5`，字符串截取输出 `x_str[:x_point]`。