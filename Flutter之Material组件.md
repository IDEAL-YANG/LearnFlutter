# Flutter之Material组件
* 实现了Material Design 指南的视觉、效果、motion-rich的widget。

## App结构和导航

### Scaffold
布局结构的基本实现部件，此类提供用于显示drawer、snackbar和底部sheet的api

## AppBar
应用程序栏，由工具栏和其他可能的widget（如TabBar和FlexibleSpaceBar组件）

### BottomNavigationBar
底部导航条，可以很容易地在tap之间切换和浏览顶级视图。

* 显示在底部的材质小部件，用于在少量视图中进行选择，通常3～5个
* 通常由文本标签、图标或两者形式的多个项目组成，排列在一块材料的顶部，提供应用程序顶级视图之间的快速导航，对于较大的屏幕，侧面导航可能更合适。
* 通用于Scaffold结合使用，作为**Scaffold.bottomNavigationBar**参数提供
* **type**属性会影响它的**items**的展示，默认的，如果少于4项，它的值是**BottomNavigationBarType.fixed**，大于等于4项，它的值是**BottomNavigationBarType.shifting**
* **items**的数量一定至少是2个，且每一项的icon和title/label不能是null
* 少于4个与大于等于4个的类型不同！！！

### TabBar
一个显示水平选项卡的Material Design widget。

* 通常创建为AppBar的**AppBar.bottom**，并与TabBarView结合使用
* 若**TabController**未提供，则一个**DefaultTabController**祖先必须被提供
* 这个TabController的**length**必须等于**tabs**列表的长度和**childre**的长度
* 它的祖先中至少有一个是材料部件
* 使用当前上下文中的**TabBarTheme**，来设置样式
* AnimationControllers can be created with `vsync: this` because of TickerProviderStateMixin 
* **late**关键字，类似于稍后赋值，保证一定有值。

### TabBarView

显示与当前选中的选项卡相对应的页面视图。通常和TabBar一起使用。

### MaterialApp
一个方便的widget，它封装了应用程序实现Material Design所需要的一些widget。

* 通过添加特定于材料设计的功能（如：AnimatedTheme和GridPaper）构建于WidgetsApp之上。
* 它配置了顶级导航**Navigator**来按以下顺序搜索路由：
	1. 对于“/”路由，对应**home**属性，如果非空，就会被使用
	2. 否则，**routes**表被使用，如果有一个路由入口点
	3. 否则，如果提供的话，**onGenerateRoute**被调用，它应该为**home**和**routes**路由，返回一个非空值的可用路由
	4. 否则，最终会调用**onUnknownRoute**

* 如果**Navigator**被创建，那么这些选项中至少有一个必须要处理“/”路由，因为在启动时指定了无效的**initialRoute**时会使用它（例如，由另一个意图在Android上启动该路由的App，可以参考：dart.ui.PlatformDispatcher.defaultRouteName）
* 这个小部件还配置顶级**Navigator**（如果有）的观察者来执行**hero**动画。
* 如果**home**、**routes**、**onGenerateRoute**和**onUnknownRoute**都为null，并且**builder**不为null，则不会创建**Navigator**。
* **WidgetsApp**，它定义了基本的应用程序元素，但不依赖于材料库。

### WidgetsApp
一个方便的类，它封装了应用程序通常需要的一些widget。

* 提供的主要角色之一是将系统后台按钮绑定到pop导航器或退出app
* **MaterialApp**和**CupertinoApp**都是使用它来实现应用程序的基本功能
* MediaQuery，它建立一个子树，其中媒体查询解析为MediaQueryData。

### Drawer
从Scaffold边缘水平滑动以显示应用程序中导航链接的Material Design面板。

* 通常与**Scaffold.drawer**属性一起使用，其child通常是一个**ListView**，它的第一个孩子是一个**DrawerHeader**，实现当前用户的状态信息，其他的抽屉孩子往往与构建**ListTile**，经常有结束的**AboutListTile**。
* 当Scaffold的drawer可用时，AppBar会自动显示适当的IconButton以显示drawer，该脚手架自动处理边缘滑动手势以展示抽屉
* Navigator.pop(context); // close the drawer
* Scaffold.of，获取当前的ScaffoldState，管理抽屉的显示和动画。
* ScaffoldState.openDrawer，显示它的Drawer，如果有的话。

## 按钮

### RaisedButton
Material Design中的button， 一个凸起的材质矩形按钮
* 弃用，推荐使用：ElevatedButton
* 高架按钮
* styleFrom方法是一种从简单值创建提升按钮ButtonStyle的便捷方法。

### FloatingActionButton
一个圆形图标按钮，它悬停在内容之上，以展示应用程序中的主要动作。FloatingActionButton通常用于Scaffold.floatingActionButton字段。

* 每个屏幕最多使用一个
* 应用于积极的操作，例如“创建”、“共享”、“导航”（如果在一个Route中使用了多个浮动操作按钮，那么请确保每个按钮都有唯一的heroTag，否则会抛出异常）
* 如果**onPressed**回调为空，则按钮将被禁用并且不会对触摸做出反应，强烈建议不要禁用浮动操作按钮，因为不会向用户指示该按钮已被禁用
* 如果禁用浮动操作按钮，请考虑更改其**backgroundColor** 

### FlatButton
一个扁平的Material按钮

* 弃动，推荐使用：TextButton
* 扁平按钮、文本按钮

### IconButton
一个Material图标按钮，点击时会有水波动画

* 要求其祖先之一是Material小部件
* 如果可能，图标按钮的点击区域的大小至少为 kMinInteractiveDimension 像素，而不管实际的 iconSize 是多少，以满足 Material Design 规范中的触摸目标大小要求。**alignment**控制图标本身如何定位命中区域内。
* 图标按钮不支持指定背景颜色或其他背景装饰，因为通常图标只是显示在父小部件的背景之上
* 使用Ink小部件创建带有填充背景的图标按钮非常容易。该油墨控件呈现底层上的装饰材料与飞溅和亮点沿着 InkResponse贡献的后裔部件。

### PopupMenuButton
当菜单隐藏式，点击或调用onSelected时显示一个弹出式菜单列表

* 按下时显示菜单，并在菜单因选择项目而关闭时调用onSelected。传递给onSelected的值是所选菜单项的值。
* 其中一个孩子或图标可以提供，但不能同时使用。如果提供了图标，则PopupMenuButton 的行为类似于IconButton。
* 如果两者都为空，则创建一个标准的溢出图标（取决于平台）。

### ButtonBar
水平排列的按钮组

* 一行末端对齐的按钮，如果没有足够的水平空间，则排列成一列。
* 根据buttonPadding水平放置按钮。孩子们在布置行与MainAxisAlignment.end。当 Directionality为TextDirection.ltr 时，按钮栏的子项右对齐，最后一个子项成为最右边的子项。当 Directionality TextDirection.rtl 时，子项左对齐，最后一个子项成为最左边的子项。
* 如果按钮栏的宽度超过小部件上的最大宽度约束，它会在列中对齐其按钮。这里的主要区别在于MainAxisAlignment将被视为跨轴/水平对齐。例如，如果按钮溢出并且 ButtonBar.alignment设置为MainAxisAlignment.start，则按钮将与按钮栏的水平起点对齐。
* 的按钮栏可与被配置ButtonBarTheme。对于 ButtonBar 上的任何 null 属性，将改为使用周围 ButtonBarTheme 的属性。如果 ButtonBarTheme 的属性也为 null，则该属性将默认为以下字段文档中描述的值。
* 的孩子们被包裹在一个ButtonTheme即周边ButtonTheme与由上面描述的按钮栏的属性重写按钮属性的副本。这些属性包括 buttonTextTheme、buttonMinWidth、buttonHeight、buttonPadding和buttonAlignedDropdown。
* 由Dialog用于在对话框底部排列操作。

## 输入框和选择框

### TextField
文本输入框

* 每当用户更改字段中的文本时，都会调用onChanged回调
* 如果用户指示他们已完成在字段中的输入（例如，通过按下软键盘上的按钮），则文本字段将调用onSubmitted回调
* 要控制文本字段中显示的文本，请使用**Controller**。例如，要设置文本字段的初始值，请使用已经包含一些文本的**Controller**。该控制器还可以控制的选择和组成区域（并观察更改文本，选择和组成区域）。
* 默认情况下，文本字段具有在文本字段下方绘制分隔线的**decoration**，您可以使用装饰属性来控制装饰，例如通过添加标签或图标。如果将decoration 属性设置为null，则装饰将被完全删除，包括装饰引入的额外填充以节省标签空间。
* 如果装饰为非空（这是默认值），则文本字段要求其祖先之一是**Material**小部件。
* 要将TextField与其他FormField小部件集成到**Form**中，请考虑使用TextFormField。
* 记得调用TextEditingController.dispose的的TextEditingController 当不再需要它。这将确保我们丢弃对象使用的任何资源。
* 对于大多数应用程序，onSubmitted回调足以对用户输入做出反应。
* 该onEditingComplete回调也当用户完成编辑运行。它与onSubmitted不同，因为它有一个默认值，用于更新文本控制器并产生键盘焦点。需要不同行为的应用程序可以覆盖默认的onEditingComplete 回调。
* 请记住，您始终可以使用TextEditingController.text从 TextField 的TextEditingController读取当前字符串 。
* 在查找某些用户输入的长度时，请使用 string.characters.length. 不要使用string.length甚至 string.runes.length. 对于复杂的字符“👨‍👩‍👦”，这在用户看来是一个单独的字符，string.characters.length 直观的返回1，反之string.length返回8， string.runes.length返回5！
* 聚焦时保持插入符号可见
聚焦时，此小部件将尝试在以下情况下保持文本区域及其插入符号（即使showCursor为false）可见：
1. 当用户关注此文本字段并且它不是readOnly 时。
2. 当用户更改文本字段的选择时，或者当文本字段不是readOnly时更改文本。
3. 当虚拟键盘弹出时。

### Checkbox

复选框，允许用户从一组中选择多个选项。

* 复选框本身不保持任何状态。相反，当复选框的状态发生变化时，小部件会调用onChanged回调。大多数使用复选框的小部件将侦听onChanged回调并使用新值重建复选框以更新复选框的视觉外观。
* 该复选框可以选择显示三个值 - true、false 和 null - 如果tristate为 true。当值为空时，会显示一个破折号。默认情况下， 三态为 false，复选框的值必须为 true 或 false。

### Radio

单选框，允许用户从一组中选择一个选项。

* 用于在多个互斥值之间进行选择。选择组中的一个单选按钮时，组中的另一个单选按钮可停止。这些值的类型T是Radio 类的类型参数。枚举通常用于此目的。
* 单选按钮本身不保持任何状态。相反，选择收音机会调用onChanged回调，将值作为参数传递。
* 如果 groupValue和value匹配，则将选择此单选。大多数小部件将通过调用State.setState来更新单选按钮的groupValue来响应onChanged。

### Switch

On/off 用于切换一个单一状态

* 一个材料设计开关。
* 用于切换单个设置的开/关状态。
* 开关本身不保持任何状态。相反，当开关的状态改变时，小部件调用onChanged回调。大多数使用开关的小部件将侦听onChanged回调并使用新值重建开关以更新开关的视觉外观。
* 如果onChanged回调为空，则开关将被禁用（它不会响应输入）。默认情况下，禁用开关的拇指和轨道以灰色阴影呈现。禁用开关的默认外观可以用inactiveThumbColor和inactiveTrackColor覆盖。

### Slider

滑块，允许用户通过滑动滑块来从一系列值中选择。

* 用于从一系列值中进行选择。
* 滑块可用于从一组连续或离散的值中进行选择。默认值是使用从min到 max的连续值范围。要使用离散值，请对Divisions使用非空值，它表示离散间隔的数量。例如，如果min为 0.0， max为 50.0 且分度为 5，则滑块可以采用离散值 0.0、10.0、20.0、30.0、40.0 和 50.0。
* 滑块部件的术语是：
	1. “拇指”，这是一个当用户拖动它时水平滑动的形状。
	2. “轨道”，即滑块拇指滑动的线。
	3. “值指示器”，这是一个形状，当用户拖动拇指时会弹出来指示被选择的值。
	4. 滑块的“活动”一侧是拇指和最小值之间的一侧。
	5. 滑块的“非活动”一侧是拇指和最大值之间的一侧。

* 如果onChanged为 null 或min .. max给出的范围 为空（即，如果min等于max），滑块将被禁用。
* 滑块小部件本身不保持任何状态。相反，当滑块的状态发生变化时，小部件会调用onChanged回调。大多数使用滑块的小部件将侦听onChanged回调并使用新值重建滑块以更新滑块的视觉外观。要知道值何时开始更改，或何时更改完成，请设置可选回调onChangeStart和/或onChangeEnd。
* 默认情况下，滑块将尽可能宽，垂直居中。当给定无限约束时，它将尝试使轨道宽度为 144 像素（每边有边距）并垂直收缩包裹。
* 为了确定应当如何显示（例如颜色，拇指形状等），滑块使用SliderThemeData可从任一SliderTheme 插件或ThemeData.sliderTheme一个主题插件它上面在widget树。您还可以使用activeColor 和inactiveColor属性覆盖某些颜色，尽管使用SliderThemeData可以实现更细粒度的外观控制。

### Date & Time Pickers

日期&时间选择器，显示一个包含 Material Design 日期选择器的对话框。

* 当用户确认对话时，返回的Future解析为用户选择的日期。如果用户取消对话，则返回 null。
* 当日期选择器第一次显示时，它会显示initialDate，initialDate选择的月份 。
* 该firstDate是最早允许的日期。的lastDate是最新的允许的日期。initialDate必须介于这些日期之间，或者等于其中之一。对于这些DateTime参数中的每一个，只考虑它们的日期。它们的时间字段被忽略。它们都必须是非空的。
* 该currentDate代表当前日（即今天）。该日期将在日期网格中突出显示。如果为 null，DateTime.now()则将使用日期 。
* 可选initialEntryMode参数可用于在DatePickerEntryMode.calendar（日历月网格）或DatePickerEntryMode.input（文本输入字段）模式下显示日期选择器。它默认为DatePickerEntryMode.calendar并且必须为非空。
* selectableDayPredicate可以传入一个可选函数，只允许某些天进行选择。如果提供，则只能selectableDayPredicate选择返回 true的天数 。例如，这可用于仅允许选择工作日。如果提供，它必须为 返回 true initialDate。
* 以下可选字符串参数允许您覆盖用于对话框各个部分的默认文本：
	1. helpText, 标签显示在对话框顶部。
	2. cancelText, 取消按钮上的标签。
	3. confirmText, 确定按钮上的标签。
	4. errorFormatText, 当输入文本的日期格式不正确时使用的消息。
	5. errorInvalidText, 当输入文本不是可选日期时使用的消息。
	6. fieldHintText, 用于在字段中未输入文本时提示用户的文本。
	7. fieldLabelText, 日期文本输入字段的标签。

* 可选locale参数可用于设置日期选择器的语言环境。它默认为Localizations提供的环境语言环境。
* 可选textDirection参数可用于设置日期选择器的文本方向（TextDirection.ltr或TextDirection.rtl）。它默认为Directionality提供的环境文本方向。如果 locale和textDirection都非空，则textDirection覆盖为 选择的方向locale。
* 的context，useRootNavigator并且routeSettings参数被传递到 的ShowDialog的文档，其中讨论了如何使用它。context 并且useRootNavigator必须是非空的。
* 该builder参数可用于包装对话框小部件以添加继承的小部件，如Theme。
* 可选initialDatePickerMode参数可用于使日历日期选择器最初出现在DatePickerMode.year或 DatePickerMode.day模式中。它默认为DatePickerMode.day，并且必须为非空。
* 使用此方法不会为日期选择器启用状态恢复。为了为日期选择器启用状态恢复，请使用 Navigator.restorablePush或Navigator.restorablePushNamed和 DatePickerDialog。

## 对话框、Alert、Panel

###SimpleDialog

简单对话框可以显示附加的提示或操作

* 一个简单的材料设计对话框。
* 一个简单的对话框为用户提供了几个选项之间的选择。一个简单的对话框有一个可选的标题，显示在选项上方。
* 选项通常使用SimpleDialogOption小部件表示。如果使用其他小部件，请参阅contentPadding以获取有关获取 Material Design 预期间距的约定的注释。
* 对于通知用户某种情况的对话框，请考虑使用 AlertDialog。
* 通常作为子小部件传递给showDialog，后者显示对话框。

### AlertDialog

一个会中断用户操作的对话款，需要用户确认

* 材料设计警报对话框。
* 警报对话框通知用户需要确认的情况。警报对话框有一个可选的标题和一个可选的操作列表。标题显示在内容上方，操作显示在内容下方。
* 如果内容太大而无法垂直显示在屏幕上，对话框将显示标题和操作并让内容溢出，这是很少需要的。考虑为content使用滚动小部件，例如 SingleChildScrollView，以避免溢出。（但是，请注意，由于 AlertDialog尝试使用其子项的固有尺寸来调整自身大小，因此使用惰性视口的ListView、GridView和CustomScrollView等小部件将无法工作。如果这是一个问题，请考虑直接使用Dialog。）
* 对于为用户提供多个选项之间选择的对话框，请考虑使用SimpleDialog。
* 通常作为子小部件传递给showDialog，后者显示对话框。

