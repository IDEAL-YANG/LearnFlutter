# 我理解的Widget

根据官方文档描述，大致理解如下：

* Flutter应用的启动从**runApp**函数开始
* 直接接受一个**Widget**并使其作为树根
* 框架会强制根widget覆盖整个屏幕
* 开发时，通常会创建新的widget：
	1. 要么是无状态的**StatelessWidget**，
	2. 要么是有状态的**StatefulWidget**，
	3. 具体取决于你的widget是否需要管理一些状态
* 一个widget
	1. 主要工作是实现**build**函数，构建自身
	2. 通常由较低级别的widget组成
	3. 框架依次构建这些widget，直到最底层的子widget（通常称为**RenderObject**，它会计算并描述widget的几何形状）

## 基础Widget

常用的有：

* Text: 创建带格式的文本
* Row、Column: 布局类Widget
	1. 具有弹性空间
	2. 可以在水平（Row）方向创建灵活布局
	3. 可以在垂直（Column）方向创建灵活布局
	4. 设计是基于Web开发中的[Flexbox](./Flutter基础之Flexbox)布局模型
* Stack: 取代线性布局
	1. 与Android中的LinearLayuout相似
	2. 允许子widget堆叠
	3. 可以使用**Positioned**来定位，相对于**Stack**的上下左右四条边的位置
	4. 设计是基于Web开发中的**绝对定位（absolute positioning）**布局模型
* Container: 创建矩形视觉元素
	1. 可以装饰为一个**BoxDecoration**，如background、一个边框、或者一个阴影
	2. 可以具有边距（margins）、填充（padding）和应用于其大小的约束（constraints）
	3. 可以使用矩阵，在三维空间中变换

## 一些代码的理解

### extends StatelessWidget
表示继承关系，表示该widget为无状态widget

### EdgeInsets.symmetric(horizontal: 8.0)
表示EdgeInsets的快捷方法，对称指定水平或者垂直边缘值

### decoration
表示对一个视觉元素，进行打扮或者叫化妆

### onPressed: null
表示禁用该事件

### Colors.blue[500]
表示使用系统内置颜色的某一种

### Icons.menu
表示使用系统内置小图标

### Expanded(child: title)
表示可以占据剩余空间，可以拥有多个children，但是需要配合**flex**参数来确定它们占据剩余空间的比例

### Theme.of(context).primaryTextTheme.headline6
表示使用当前上下文里配置的主题，取系统的主要文本主题中的**headline6**

### new Material
表示使用材料widget，在UI呈现中，充当**一张纸**的角色，比如你要写字，就需要纸一样

### MyAppBar({required this.title});
表示自定义wdiget的对外属性绑定的内置变量，且声明了是否是必填项

### MaterialApp(home:
表示使用材料App的widget的根路由，表示为**/**，除非**initialRoute**被指定。

### uses-material-design: true
在文件pubspec.yaml中，这允许我们使用预定义的Material Icons

### new MaterialApp
为了继承主题等数据，我们的自定义widget组件需要位于**MaterialApp**内才能正常显示