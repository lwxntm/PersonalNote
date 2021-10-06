# Xamarin 设置应用程序的标题和图标

如果需要本地化的标题，可以在`\项目名称.Android\Resources\values\String.xml`里定义：

```xml
<?xml version="1.0" encoding="utf-8" ?>
<resources>
	<string name="app_name">LeiTool</string>
	<string name="taskadd">Add Task</string>
	<string name="taskname">Name</string>
	<string name="tasknotes">Notes</string>
	<string name="taskdone">Done</string>
	<string name="taskcancel">Cancel</string>
</resources>
```

把图标放在drawable文件夹，然后在MainActivity里设置：

```c#
[Activity(Label = "@string/app_name", Icon = "@drawable/toolbox"
```

还有一步：右键安卓项目，选择属性，然后在安卓清单里，修改应用程序名称