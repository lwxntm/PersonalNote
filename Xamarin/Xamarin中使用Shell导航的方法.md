# Xamarin中使用Shell导航的方法

Shell中如果要使用导航，主要是使用路由的方式，使用Shell导航的好处是，可以在ViewModel里定义页面的切换，而不需要完全绑定在Page里。

假如我们有一个叫ItemDetialPage的ContentPage，想要在项目的某个Command里导航过去，则首先注册这个路由：

在MainShell的构造函数里注册路由：

```c#
 public partial class MainShell : Shell
    {
        public MainShell()
        {
            InitializeComponent();
            Routing.RegisterRoute(nameof(ItemDetialPage), typeof(ItemDetialPage));
          
        }
    }
```

在使用时，假如点击一个StackLayout的时候，想要跳转到ItemDetialPage，在该StackLayout里定义手势识别器，

```xaml
<StackLayout.GestureRecognizers>
                                <TapGestureRecognizer NumberOfTapsRequired="1"
                                                      Command="{Binding Source={RelativeSource AncestorType={x:Type viewmodel的命名空间:viewmodel的类名}}, Path=ItemTapped}"
                                                      CommandParameter="{Binding .}"></TapGestureRecognizer>
                            </StackLayout.GestureRecognizers>
```

在该页面对应的ViewModel里定义相应逻辑如下，首先是一个委托命令：

```c#
public DelegateCommand<CiSong> ItemTapped {  get; set; }
```

然后设计跳转的逻辑：

```c#
async void OnItemSelected(Item item)
        {
            if (item == null)
                return;

            // This will push the ItemDetailPage onto the navigation stack
            await Shell.Current.GoToAsync($"{nameof(ItemDetialPage)}");
        }
```

别忘了在VIewModel的构造函数里初始化DelegateCommand，指向该异步函数

```c#
ctor{
 ItemTapped = new DelegateCommand<Item>(OnItemSelected);
}
```

## 带有参数的导航

如果我们想要在导航的时候加上参数，也是可以的，但是参数只能是字符串，我刚开始踩坑的时候想把我的model传过去，结果发现并不可以，因为导航本身是类似于http网址一样的格式。

```c#
//只需要在导航的时候使用以下语句：其中searchCondition就是形式参数，s1里的内容是实际参数
await Shell.Current.GoToAsync($"{nameof(ItemDetialPage)}?SearchCondition={s1}");
```

在目标页面的viewmodel中，加入注解：QueryProperty

```c#
[QueryProperty(nameof(SearchCondition), "SearchCondition")]
    public class ItemDetialViewModel : BindableBase
    {
        //检索条件，作为内部字段和属性
        private string searchCondition;
        
        public string SearchCondition
        {
            get { return searchCondition; }
            set
            {
                searchCondition = value;
                //这里定义了一个内部异步函数，当该属性被赋值时，加载读取逻辑，相当于构造函数的替代
                OnItemLoad();
            }
        }
    }
```

