# Flutter基础之综合实例

## 购物车🛒Demo

### ShoppingListItem
* widget的参数声明，有两种方式
	1. 一个是this.变量名
	2. 一个是正常测参数声明，在冒号：后指定具体的变量 = 参数变量	
* 返回值类型可以声明为：类型？，表示可为空
* 列表widget：**ListTile**
	1. onTap: () => onCartChanged(product, !inCart)
	2. onTap: () {onCartChanged(product, !inCart);}
	3. 以上两种等价，一个是箭头函数，一个是正常的block
       	
* 点击列表项，widget不会直接修改其**inCart**值，而是会调用其父widget给它的回调函数，此模式可以让你在widget层次结构中存储更高的状态，从而使状态持续更长的时间
* 极端情况下，存储传给**runApp**应用程序的widget的状态将存在整个生命周期中持续存在
* 父项收到回调时，更新其内部状态，触发父项使用新的**inCart**值重建**ShoppingListItem**新实例。
* 虽然父项在重建时创建了一个新的实例，但该操作开销很小，因为Flutter框架会将新构建的widget与先前构建的widget进行比较，并仅将差异部分应用于底层**RenderObject**

### ShoppingList
* 当**ShoppingList**首次插入到树中时，框架会调用其**createState**函数创建一个新的**_ShoppingListState**实例来与该树中的相应位置关联（注意，我们通常命名State子类时带一个下划线，表示其私有）
* 当这个widget的父级重建时，父级将创建一个新的**ShoppingList**实例，但是Flutter框架将重用已经在树中的**_ShoppingListState**实例，而不是再次调用**createState**创建一个新的
* 要访问当前**ShoppingList**的属性，**_ShoppingListState**可以使用它的**widget**属性
* 如果父级重建并创建一个新的ShoppingList，那么**_ShopingListState**也将用新的**widget**值重建（注意：_ShopingListState不会重建，而其**widget**属性会更新为新构建的widget）
* 如果希望在**widget**属性更改时收到通知，则可以覆盖**didUpdateWidget**函数，以便将旧的**oldWidget**与当前**widget**进行比较。
* 调用**setState**来通知框架它改变了它的内部状态，将该widget标记为“dirty（脏的）”，并且计划在下次应用程序需要更新屏幕时重新构建它。
* 如果在修改widget的内部状态后忘记调用**setState**，框架将不知道您的widget是“dirty（脏的）”，并且可能不会调用widget的**build**方法，意味着用户界面可能不会更新以展示新的状态。
* 通过这种方式管理状态，你不需要写用于创建和更新子widget的单独代码，相反，你只需实现可以处理这两种情况的build函数。