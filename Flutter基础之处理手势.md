# 我来体验手势
* **GestureDetector**作为一个widget，并不具有显示效果
* 检测由用户做出的手势
* 需要child属性，指定其显示的widget
* 当用户点击其child时，可以检测出各种输入手势，包括点击、拖动和缩放
* 许多的widget都会使用一个**GestureDetector**来为其他widget提供可选的回调。例如：**IconButton**、**RaisedButton**、**FloatingActionButton**，它们都有一个onPressed回调。

## 根据输入改变widget

### 无状态 VS 有状态
* 无状态的widget从它们的父widget接收参数，它们被存储在final型的成员变量中，widget被要求构建时，它使用这些存储的值作为参数来构建。
* 有状态的widget，知道如何生成state对象，然后用它来保持状态。

### StatefulWidget VS State
* 单独的对象
* 具有不同的生命周期
* Widget是临时对象，用于构建当前状态下的应用程序
* State对象在多次调用**build()**之间保持不变，允许他们记住信息（状态）

### 事件流 VS 状态流
* 事件流是**向上👆**传递的
* 状态流是**向下👇**传递的
* 类似于React/Vue中父子组件通信的方式
* 子widget到父widget是通过**事件**通信
* 父widget到子widget是通过**状态**通信
* 重定向这一流程的共同父元素是**State**
