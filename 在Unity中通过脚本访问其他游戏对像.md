####在Unity中通过脚本访问其他游戏对像

#####1、代码中获取

```C#
// 通过名称获取
GameObject go = GameObject.Find("name");

// 通过TAG获取
GameObject go = GameObject.FindWithTag("tag");
```

但是这里需要注意，所有的Find方法都是比较耗时的（内部通过递归实现），不应该在Update函数中使用。

##### 2、借助Unity Edit获取

在脚本类中直接将GameObject声明为public变量，并在Unity的Inspector面板中对GameObject直接赋值。



#### 通过脚本访问其他组件

脚本等组件其实都是类，而他对应的实例是需要依附在GameObject上的，所以需要访问组件就需要先获取到GameObject，然后通过GetComponent函数访问组件

```C#
GameObject go = GameObject.Find("name");
A a = go.GetComponent("A");
a.dosomething();
```

