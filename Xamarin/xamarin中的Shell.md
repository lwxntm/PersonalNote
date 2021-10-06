# xamarin中的Shell

主类继承Shell

第一优先级是FlyoutItem，是从左边飞出的页面

里面包Tab，是下方选择的页面

再包ShellContent，是上方选择的页面



## 如何设置TabBar的背景颜色等信息？

```xaml
<Shell.Resources>
        <ResourceDictionary>
            <Style x:Key="BaseStyle" TargetType="Element">
                <Setter Property="Shell.BackgroundColor" Value="{StaticResource Primary}" />
                <Setter Property="Shell.ForegroundColor" Value="White" />
                <Setter Property="Shell.TitleColor" Value="White" />
                <Setter Property="Shell.DisabledColor" Value="#B4FFFFFF" />
                <Setter Property="Shell.UnselectedColor" Value="#95FFFFFF" />
                <Setter Property="Shell.TabBarBackgroundColor" Value="{StaticResource Primary}" />
                <Setter Property="Shell.TabBarForegroundColor" Value="White"/>
                <Setter Property="Shell.TabBarUnselectedColor" Value="#95FFFFFF"/>
                <Setter Property="Shell.TabBarTitleColor" Value="White"/>
            </Style>
            <!--下面这一行把TabBar的style绑定到上面-->
            <Style TargetType="TabBar" BasedOn="{StaticResource BaseStyle}" />
            <Style TargetType="FlyoutItem" BasedOn="{StaticResource BaseStyle}" />
        </ResourceDictionary>
    </Shell.Resources>
```



如何在Toolbar上添加按钮？

```xaml
<ContentPage.ToolbarItems>
        <ToolbarItem Text="Add" Command="{Binding AddItemCommand}" />
    </ContentPage.ToolbarItems>
```

