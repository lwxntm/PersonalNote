# WPF-MVVMlight增删改查小软件的笔记



## 1:创建一个.net Framework的WPF项目

可选使用UI框架：HandyControl

## 2：引入nuget包：Prism.Wpf

不推荐使用：Microsoft.Toolkit.Mvvm 

原因是我们自己定义的实体类如果要继承他的`ObservableRecipient`，会莫名奇妙多一个隐形的"isActive"的字段，对数据库进行操作时非常不友善。(俺大晚上的踩坑，在那debug。。。)

## 3： 创建数据模型类： 比如： Models/T.cs

让该类继承 `BindableBase` ，设计类的属性，属性的set方法里添加一个：`RaisePropertyChanged()` 

## 4： 设计数据操作层：

主要实现数据库的连接和ORM，增删改查那一套， ， 添加方法：

* GetAll()
* Edit()   //使用EFCore里的Update
* Add()
* Del()
* GetByName()
* GetById()
* 等等。。。

## 5： 主界面的设计

在`MainWindow`的构造函数中，新建一个实现了 `BindableBase`的 `MainViewModel`的对象，然后把`MainWindow`的`DataContext`指向该对象，代表将主界面的上下文指向`MainViewModel`对象实体，这样我们就可以在主界面的xaml设计时随时将一个数据和命令绑定到`MainViewModel`里的字段。

用 `Grid.RowDefinitions` 设计分行，定义行高，然后每一行中都可以用`StackPanel`设计；

  

设计搜索条件，搜索R，重置R ，新增C，编辑U，删除D等按钮时，按钮的 `Command`属性，绑定到`MainViewModel`里的类型为`DelegateCommand`的对象。



该对象默认为空，在MainViewModel的构造函数里初始化，功能是调用MainViewModel里的一些增删改查方法



DataGrid，内容绑定使用ItemsSource，绑定到ObservableCollection<T>，选中的单个项绑定为SelectedItem；



