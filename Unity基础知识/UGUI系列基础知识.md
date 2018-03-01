##UGUI系列基础知识

###与NGUI的比较

#### UGUI的Atlas和NGUI的Atlas的区别

- NGUI是必须先打出图集然后才能开始做界面，这样你始终要去考虑你的UI图集（比如图集会不会超1024，图集该如何规划等）
- 而UGUI的原则则是，让开发者不用去关心自己的图集。在做界面时只用小图，最终打包时Unity会自动把你的小图合并在一张大图集中。（当然你也可以自己设置图集）

在Unity的工具栏中，Editor->Project Settings 下面有sprite packer的模式。Disabled表示不启用它，Enabled For Builds 表示只有打包的时候才会启用它，Always Enabled 表示永远启用它。 这里的启用它就表示是否将小图自动打成图集。

####不同点

使用UGUI最大的一个不同点在于，UGUI不支持在一个图集中动态获取其中一个图集。

解决方案：

- 先将散图打成图集，再将散图复制到Resources目录中，这样就可以使用直接使用 Resources.Load() 来加载散图；
- 自己写方法实现：生成散图和图集的映射关系（Make - AtlasInfo），先将图集的子图都加载到缓存中，自定义数据结构保存起来，加载时根据散图的名称映射出图集的名称，再取出与之对应的子图的Sprite和图集的Material，赋值给Image控件的sprite和material属性。




### NGUI模块的优化

在NGUI的优化方面，UIPanel.LateUpdate为性能优化的重中之重，它是NGUI中CPU开销最大的函数。

NGUI中CPU开销最耗时的几个函数有： UICamera.Update()、UIRect.Update()、UIPanel.LateUpdate()、UIRect.Start()。

UIPanel.LateUpdate的优化在于对UIPanel的布局，原则如下：

- 尽可能将动态UI元素和静态UI元素分离到不同的UIPanel中，从而尽可能将因为变动的UI元素引起的重构控制在较小的范围内；
- 尽可能让动态UI元素按照同步性进行划分，即运动频率不同的UI元素尽可能分离放在不同的UIPanel中；
- 控制同一个UIPanel中动态UI元素的数量，数量越多，所创建的Mesh越大，从而使得重构的开销显著增加。




### 自适应相关

Canvas部件，在属性Render Mode中有三个选项

- Screen Space - Overlay： 此模式不需要UI摄像机，UI将永远出现在所有摄像机的最前面，但这样就不能再该UI前放个特效或UI了。
- Screen Space - Camera：基本和NGUI的原理一样
- World Space：这个就是完全3D的UI了

每个相机都有深度选项，这个属性对应渲染顺序，深度值越大，显示就越靠前，可以通过设置这个值和相机的显示范围实现多相机显示。



























