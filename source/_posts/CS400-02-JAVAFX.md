---
title: CS400-02-JAVAFX
date: 2020-07-02 22:18:54
tags: JAVA
---
### what is an event?
 普通编程是串行的，但是在GUI中我们不知道下一次要做什么，需要根据用户的行为来决定。
 Events are asynchronous, can happen any time.
 <!--more-->
Event is triggered when the user does something with the GUI.
The code is called "event holder" holder.
```
public interface EventHander<T extends Event> extends Eventlistener{
    public void handle(T event);

}

```

### GUI component
#### Stage 