## 标准错误输出

优先打印到终端

```python
import sys

def FindGclientRoot( ):
  print('111111', file=sys.stderr) # 可以保证优先打印出来，如果不加后面的，则所有线程完毕再打印。
```

## 带颜色的输出

```shell
print("\033[31m"+type_name+"\033[0m")
```

参考资料：

[Python基础之控制台输出颜色](https://blog.csdn.net/qq_33567641/article/details/82769523)