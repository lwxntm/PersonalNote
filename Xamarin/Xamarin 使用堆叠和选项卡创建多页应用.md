# Xamarin 使用堆叠和选项卡创建多页应用

## 1：NavigationPage

```c#
public partial class App : Application
    {
        //..........
        public App()
        {
            InitializeComponent();
            
            MainPage = new NavigationPage(new MainPage());
        }
   }

public partial class MainPage : ContentPage
    {
//创建新的页面
 private async void MaterialButton_Clicked(object sender, EventArgs e)
        {
            await this.Navigation.PushAsync(new Page1());
            //或者下面这种，Modal页面，可以限制用户主动返回，
            await this.Navigation.PushModalAsync(new Page1());
        }
//关闭并返回刚才的页面
    private async void Button_Clicked(object sender, EventArgs e)
        {
            await this.Navigation.PopModalAsync();
        }
}
```

## 2： TabbedPage

TabbedPage是一个需要实例化的对象，实例化时在构造函数中创建子页面，子页面一般是ContentPage：

```c#
 [XamlCompilation(XamlCompilationOptions.Compile)]
    public partial class TabbedPage1 : TabbedPage
    {
        public TabbedPage1 ()
        {
            InitializeComponent();
            this.Children.Add(new Page1("第一页"));
            this.Children.Add(new Page1("第2页"));
            this.Children.Add(new Page1("第叄页"));
        }
    }
```

然后每一个单独的ContentPage里可以设置属性：`Title`：标题，`Icon`：图标，图标文件存放在：`项目名称.Android\Resources\drawable`

