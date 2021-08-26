# Flutter之基础组件

## Container
* 一个拥有绘制、定位、调整大小的widget
* 容器首先用padding包围子元素（由属性**decoration**描述的任何边框）
* 然后对填充的范围应用额外的约束（如果width或者height其中一个非空）
* 然后容器被从**margin**描述的额外空白空间包围
* 绘制中
	1. 首先应用给定的**transform**
	2. 然后绘制**decoration**以填充填充范围
	3. 然后绘制子对象
	4. 最后绘制**foregroundDecoration**，同时填充填充范围
* 没有子容器的容器会尽量大，除非传入的约束是无界的，在这种情况下，它们会尽量小。
* 有子容器会根据子容器的大小调整自己的大小，子容器的width、height、约束等参数会在构造时覆盖父容器。
* 默认情况下，容器对所有命中测试都返回false，如果指定了color属性，则命中测试由ColoredBox处理，他始终返回true；如果指定了Decoration或foregroundDecoration属性，则命中测试由Decoration.hitTest处理。

### 布局行为
* 参考：**BoxConstraints**
* 由于Container结合了许多其他小部件，每个小部件都有自己的布局行为，因此布局行为有些复杂
* 简单来说，它会尝试以下顺序：
	1. **alignment**
	2. 根据**child**调整自身大小 
	3. 宽、高和**constraints**
	4. 扩展来适配其parent
	5. 保持尽可能的小

## Row
* 在水平方向上排列子widget的列表。
* 若将子项扩展以填充可用水平空间，需要将子项包裹在**Expanded**中
* 该组件不滚动。如果需要滚动，考虑**ListView**
* 如果只有一个child，考虑使用**Align**或者**Center**来包裹child
* 如果行的非灵活部分（未包含在**Expanded**或者**Flexible**中的内容）加在一起比行本身宽，则称该行已溢出，
* 此时，该行没有任何剩余空间可在其Expanded或Flexible子项之间共享，该行通过在溢出的边缘绘制一个黄色和黑色条纹警告框来报告这一点，
* 如果行外侧有空间，则溢出量以红色字体打印。
* **textDirection**属性控制孩子的渲染方向

### 布局算法
* Row的布局分六个步骤：
	1.  使用无界水平约束和传入的垂直约束为每个孩子布局一个null或者零弹性因子（那些没有Expanded的），如果crossAxisAlignment是**CrossAxisAlignment.stretch**，则使用与传入最大高度匹配的紧密垂直约束
	2. 根据它们的弹性因子，在具有非零弹性因子（例如，那些Expanded）的子级之间划分剩余的水平空间。
	3. 使用与步骤1中相同的垂直约束来布局剩余的每个子项，但不要使用无边界的水平预约，而是根据步骤2中分配的空间量使用水平约束。
	4. 行的高度取决于孩子的最大高度（始终满足传入的垂直约束）
	5. 行的宽度有**mainAxisSize**属性决定。如果该属性是**MainAxisSize.max**，则宽度为传入约束的最大宽度，如果该属性是**MainAxisSize.min**，则宽度为所有子项的宽度的总和（受到传入约束的约束）
	6. 根据**mainAxisAlignment**和**corssAxisAlignment**确定每个孩子的位置。例如，如果 mainAxisAlignment是MainAxisAlignment.spaceBetween，则任何尚未分配给子级的水平空间将被平均划分并放置在子级之间。

## Column
* 在垂直方向上排列子widget的列表。
* 其他特点与Row类似，只是方向问题

## Image
* 一个显示图片的widget
* 为指定图像的各种方式提供了几个构造函数：
	1. **new Image**，用于从**ImageProvider**获取图像
	2. **new Image.asset**，用于使用密钥从**AssetBundle**获取图像
	3. **new Image.network**，用于从URL获取图像
	4. **new Image.file**，用于从**File**获取图像
	5. **new Image.memory**，用于从**Uint8List**获取图像

* 支持以下图片格式：JPEG、PNG、GIF、动画 GIF、WebP、动画 WebP、BMP 和 WBMP
* 底层平台可能支持其他格式。Flutter 会尝试调用平台 API 来解码无法识别的格式，如果平台 API 支持解码图像，Flutter 将能够渲染它。
* 要自动执行像素密​​度感知资产分辨率，请使用AssetImage指定图像并确保MaterialApp、WidgetsApp或MediaQuery小部件存在于小部件树中的Image小部件上方。
* 图像是使用**paintImage**绘制的，它更详细地描述了此类中各个字段的含义。
* Image.asset，Image.network，Image.file和Image.memory 构造允许通过被指定的自定义解码尺寸 cacheWidth和cacheHeight参数。引擎会将图像解码为指定的大小，这主要是为了减少ImageCache的内存使用。
* 在Web平台上使用网络图像的情况下， 由于Web引擎将网络图像的图像解码委托给Web，Web引擎不支持自定义解码大小，因此会忽略cacheWidth和cacheHeight参数。

## Text
* 单一格式的文本
* **style**属性是可选的，省略时，文本将使用最接近**DefaultTextStyle**的样式。
* 如果给定样式的**TextSysle.inherit**属性为true（默认值），则给定样式与最近的封闭**DefaultTextStyle**合并，这种合并行为很有用，如果，在使用默认字体系列和大小时，使文本加粗
* 使用Text.rich构造函数，借助**TextSpan**可以实现不同文字的不同风格。
* 要是**Text**对触摸事件做出反应，请将其包装在GestureDetector部件中，借助**GestrueDetector.onTap**处理程序
* 在材料设计App中，考虑使用**TextButton**代替，如果不合适，至少可以使用**InkWell**而不是**GestureDetector**。

## Icon
* A Material Design icon.
* 来自于所述的字体的字形绘制**IconData**
* 图标不是交互式的，对于交互式图标，请考虑材质的**IconButton**
* 使用**Icon**时必须有一个环境**Directionality**组件，通常都是由**MaterialApp**或**WidgetsApp**自动引入的。
* 此小部件假定呈现的图标时方形的，非方形可能无法正确呈现。

## RaisedButton（弃用，使用ElevatedButton）
* Material Design中的高架按钮
* 使用其为原本大部分为平面的布局添加纬度，例如在长而繁忙的内容列表中，或者在宽阔的空间中。应避免在对话框或者卡片等已经提升的内容上使用该按钮
* 显示在**Material**小部件上的标签**child**，当按下按钮时，其**Material.elevation**会增加，标签的**Text**和**Icon**小部件显示在**style**的**ButtonStyle.foregroundColor中，按钮的填充背景是**ButtonStyle.backgroundColor**
* 其按钮的默认样式由**defaultStyleOf**定义，可以使用它的**style**参数覆盖。可以用**ElevatedButtonTheme**覆盖子树中所有提升按钮的样式，并且可以使用**Theme**的ThemeData.elevatedButtonTheme**属性覆盖App中所有提升按钮的样式。
* 静态**styleFrom**方法是一种从简单值创建提升按钮**ButtonStyle**的便捷方法。
* 如果**onPressed**和**onLongPress**回调为空，则按钮将被禁用。

## Scaffold（脚手架）
* Material Design布局结构的基本实现。此类提供了用于显示drawer、snackbar和底部sheet的API。
* Scaffold 被设计为MaterialApp的顶级容器，这意味着向 Material 应用程序上的每个路由添加 Scaffold 将为应用程序提供 Material 的基本视觉布局结构。
* 通常不需要嵌套 Scaffolds
* 尽管在某些用例中，例如显示嵌入式 Flutter 内容的演示应用程序，嵌套脚手架是合适的，但最好避免嵌套脚手架。
* 其属性，提供了一个普通app需要的所有部分

## Appbar
* 一个Material Design应用程序栏，由工具栏和其他可能的widget（如TabBar和FlexibleSpaceBar）组成。
* 由一个工具栏和其他可见的小部件组成，例如**TabBar**和**FlexibleSpaceBar**。
* 通常使用**IconButton**公开一个或多个常见的操作，这些操作可选的后跟一个**PopupMenuButton**用于不太常见的操作（有时称为“溢出菜单”）
* 通常用于**Scaffold.appBar**属性，作为固定高度的小部件放在屏幕顶部，对于可滚动的应用条，请参考**SliverAppBar**，它将AppBar嵌入到sliver中以在**CustomScrollView**中使用。
* 将AppBar包裹在MediaQuery小部件中，并调整其填充以使动画流畅。
* 如果省略了**leading**小部件，但是AppBar是处在Scaffold中带有**Drawer**的话，则会插入一个按钮来打开抽屉。如果不是的话，会看**Navigator**前方是否有路由，有则补充一个**BackButton**。这种行为，可以通过设置**automaticallyImplyLeading**为false来关闭，此时空的leading，会导致中间/title小部件拉伸到开始位置。
* **SnackBar**是一个底部的toast条
* **ScaffoldMessenger**是脚手架消息中心，负责展示消息。
* **Navigator.push**导航跳转，负责push新的页面
* **MaterialPageRoute(builder**构建新的页面路由

## FlutterLogo
* Flutter logo, 以widget形式. 这个widget遵从IconTheme。
* ImageIcon，用于显示来自AssetImage或其他ImageProvider的图标。

## Placeholder
* 一个绘制了一个盒子的的widget，代表日后有widget将会被添加到该盒子中
* 默认情况下，占位符的大小适合其容器。如果占位符位于无界空间中，它将根据给定的fallbackWidth和fallbackHeight 调整自身大小。

