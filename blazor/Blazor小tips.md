# Blazor小tips

### 在页面初始化的时候执行的函数：

```c#
protected override Task OnInitializedAsync()
    {
        return base.OnInitializedAsync();
    }
```





### 页面导航

```c#
@page "/"

@inject NavigationManager nv

<button @onclick="got">点我跳转</button>

Welcome to your new app.

@code {
    private void got()
    {
        nv.NavigateTo("fetchdata");
    }
}
```





把C#代码和razor分开：

Count.razor和CountBase.cs (继承componentBase)

```
@inherits CountBase
```

