## Unity中的基本概念

###一、场景（Scene）

一个游戏一般包含多个场景，每个场景实现不同的功能，把它们组合起来就是一个完整的游戏了。

###二、游戏物体（GameObject）

在Unity游戏中不论是布景还是人物，所有的东西都称之为“GameObject”游戏对象（2D游戏一般称之为“精灵”）。所以游戏场景是由游戏对象组成的，一个场景就相当于是一个独立的世界。

###三、组件（Component）

每一个GameObject都可以拥有自己的各种信息，而这些信息都是以“组件”（Component）的方式存在的。游戏对象是由一个到多个组件组成的，我们可以将组件看成是组成一台机器的零部件。Unity游戏是通过组件的方式进行开发的，所以想要操作游戏对象也都是通过操作对应的组件对象。

组件分为预定义组件和自定义组件，预定义组件以MonoBehavior的成员变量的形式出现。自定义组件可以通过MonoBehavior的成员函数获得。

#### 预定义组件

预定义组件以MonoBehavior的成员变量的形式出现，如果GameObject中不存在某组件，则该成员变量为null。

| 组件名称      | 变量名       | 组件作用         |
| --------- | --------- | ------------ |
| Transform | transform | 设置对象位置，旋转，缩放 |
| Rigidbody | rigidbody | 设置物理引擎的刚体属性  |
| Renderer  | renderer  | 渲染物体模型       |
| Light     | light     | 设置灯光属性       |
| Camera    | camera    | 设置相机属性       |
| Collider  | collider  | 设置碰撞体属性      |
| Animation | animation | 设置动画属性       |
| Audio     | audio     | 设置声音属性       |

#### 自定义组件

Unity允许为GameObject添加自定义组件（脚本也属于自定义组件之一），在MonoBehavior里，这些自定义组件获取方式如下：

| 函数名                     | 作用                   |
| ----------------------- | -------------------- |
| GetComponent            | 得到组件                 |
| GetComponents           | 得到组件列表（用于多个同类型组件的时候） |
| GetComponentInChildren  | 得到对象或对象子物体上的组件       |
| GetComponentsInChildren | 得到对象或对象子物体的组件列表      |

###四、脚本

在Unity中创建的每一个脚本文件，都会包含一个与脚本文件名相同且继承**MonoBehaviour**的public类，这类脚本在Unity中同样是作为一种组件（自定义组件），可挂载到GameObject上实现某些自定义的功能。

同时，作为一个组件，脚本无法脱离GameObject而独立运行，脚本必须添加到游戏对象上才能生效。

**注意：**一个脚本文件可以产生多个实例，每一个实例都可以独立地被添加到GameObject上，在同一个脚本内编码时可使用transform.tag来区分不同GameObject的脚本实例。

####MonoBehavior

MonoBehavior是Unity脚本编程中最重要的类。它提供的功能使我们能够轻松地控制游戏对象.

其中事件分为必然事件和普通事件，这些事件以MonoBehavior成员函数的形式出现。

- 必然事件：

  | 名称            | 触发条件                       |
  | ------------- | -------------------------- |
  | Awake()       | 脚本实例被创建时调用                 |
  | Start()       | Update函数第一次运行之前调用          |
  | FixedUpdate() | 每个固定物理时间间隔调用一次，一般用于物理状态的更新 |
  | Update()      | 每帧调用一次，用于更新游戏场景的状态         |
  | LateUpdate()  | 用于更新游戏场景和状态，和相机有关的更新一般放在这里 |

- 普通事件

  | 名称                      | 触发条件                      |
  | ----------------------- | ------------------------- |
  | OnMouseEnter            | 鼠标移入GUI控件或者碰撞体时调用         |
  | OnMouseOver             | 鼠标停留在GUI控件或者碰撞体时调用        |
  | OnMouseExit             | 鼠标移出GUI控件或者碰撞体时调用         |
  | OnMouseDown             | 鼠标在GUI控件或者碰撞体上按下时调用       |
  | OnMouseUp               | 鼠标按键释放时调用                 |
  | OnTriggerEnter          | 当其他碰撞体进入触发器时调用            |
  | OnTriggerStay           | 当其他碰撞体离开触发器时调用            |
  | OnCollisionEnter        | 当碰撞体或者刚体与其他碰撞体或者刚体接触时调用   |
  | OnCollisionExit         | 当碰撞体或者刚体与其他碰撞体或者刚体停止接触时调用 |
  | OnCollisonStay          | 当碰撞体或者刚体与其他碰撞体或者刚体保持接触时调用 |
  | OnControllerColliderHit | 当控制器移动时与碰撞体发生碰撞时调用        |
  | OnBecameVisible         | 对于任意一个相机可见时调用             |
  | OnBecameInvisible       | 对于任意一个相机不可见时调用            |
  | OnEnable                | 对象启用或者激活时调用               |
  | OnDisable               | 脚本禁用或者取消激活时调用             |
  | OnDestroy               | 脚本销毁时调用                   |
  | OnGUI                   | 渲染GUI和处理GUI消息时调用          |

####在脚本访问其他GameObject

##### 1、代码中获取

```C#
//	通过名称获取
GameObject go = GameObject.Find("name");

//	通过TAG获取
GameObject go = GameObject.FindWithTag("TagName");
```

但是需要注意的是，所有的Find方法都是比较耗时的（内部通过递归实现），不应该在Update函数中使用。

##### 2、借助Unity Edit获取（最常用）

在脚本类中直接将GameObject声明为public变量，并在Unity的Inspector面板中对GameObject直接赋值。

#### 通过脚本访问其他组件

脚本等组件其实都是类，而他对应的实例是需要依附在GameObject上的，所以需要访问组件就需要先获取到GameObject，然后通过GetComponent函数访问组件。

```C#
GameObject go = GameObject.Find("name");
A a = go.GetComponent("A");
a.dosomething();
```

