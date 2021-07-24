# Flutter基础之widget生命周期

## widget的生命周期事件
* **createState**
	1. StatefulWidget调用**createState**之后，框架将新的状态对象插入树种，
* **initState**
	1. 然后调用状态对象的**initState**
	2. 子类化可以重写**initState**，已完成仅需要执行一次的工作。
	3. 例如：配置动画，订阅platform services
	4. **initState**的实现中需要调用**super.initState**
* **dispose**
	1. 当一个状态对象不在需要时，框架调用状态对象的**dispose**，可以覆盖该方法来执行清理工作。
	2. 例如：取消定时器、取消订阅platform services。
	3. 典型实现是调用**super.dispose**

## Key
* 使用key控制框架将在widget重建时与哪些其他的widget匹配
* 默认情况下根据它们的**runtimeType**和它们的显示顺序来匹配
* 使用key时，要求两个widget具有相同的key和runtimeType
* 通常用在构建相同类型widget的多个实例时很有用
* 例如ShoppingList构建足够多的ShoppingListItem实例以填充其可见区域：
	1. 如果没有key，当前构建中的第一个条目将始终与前一个构建中的第一个条目同步，即使在语义上，列表中的第一个条目如果滚动出屏幕，那么它将不会在窗口中可见。
	2. 如果给每个条目分配为“语义”key，无限列表可以更加高效，因为框架将会同步条目与匹配的语义key并因此具有相似（或相同）的可视外观。此外，语义上同步条目意味着在有状态的子widget中，保留的状态将附加到相同的语义条目上，而不是附加到相同数字位置上的条目。 

## 全局Key
* 使用全局key来唯一标识子widget