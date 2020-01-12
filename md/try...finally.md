
```buildoutcfg
try:
     // do something
except Exception as e:
    // dual with exception
finally:
    // finally job
```
对于 finally 这一块，很多教程都会说到，无论 try 和 except 中的内容是否被执行到，finally 中的内容都会被执行。


经测试发现，finally语句块确实是无论如何都会被执行到的，
即使是 try 或者 except 中包含有 return 语句。  
程序会在执行完 finally 语句块后，再回到 return 语句