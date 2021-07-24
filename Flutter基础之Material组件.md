# 我来用Material组件
* Material应用程序以**MaterialApp**的widget开始
* 该widget在应用程序的根部创建一些有用的widget
* 包括一个**Navigator**，它管理由字符串标识的Widget栈（页面路由栈），它可以让页面之间平滑的过渡

## Scaffold（脚手架）
* 它是Material中的主要的**布局组件**
* appBar属性，定义导航条
* body属性，占据屏幕的大部分
* floatingActionButton属性，漂浮在上层的行为按钮

## Widget传递、填充
从几个实例中，可以看到，自定义的widget，需要许多其他widget来进行填充，这个思想贯穿始终