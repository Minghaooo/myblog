---
title: CS400-040-Exception
date: 2020-07-10 00:54:29
tags: JAVA
---

### try, catch

```
// ... means normal code
...
try {
   ...   
   // If error detected
      throw objectOfExceptionType;
   ...
}
catch (exceptionType excptObj) {
   // Handle exception, e.g., print message
}
```
try 里面是正常执行的代码，如果执行到了 throw 那一行，代表错误产生了。
如果错误产生了，那么转到catch，如果错误类型是catch 后面括号里的，那么执行下面的程序。相当于return 给了cache()， return 后面的函数自然就不执行了。