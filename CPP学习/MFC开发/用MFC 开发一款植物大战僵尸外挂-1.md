# 用MFC 开发一款植物大战僵尸外挂-1：MFC的Hello， world

## MFC是啥？

**微软基础类库**（英语：**M**icrosoft **F**oundation **C**lasses，简称**MFC**）是一个[微软公司](https://zh.wikipedia.org/wiki/微软公司)提供的类库（class libraries），以[C++](https://zh.wikipedia.org/wiki/C%2B%2B)类的形式封装了[Windows API](https://zh.wikipedia.org/wiki/Windows_API)，并且包含一个（也是微软产品的唯一一个）应用程序框架，以减少应用程序开发人员的工作量。其中包含的类包含大量Windows[句柄](https://zh.wikipedia.org/wiki/句柄)封装类和很多Windows的内建控件和组件的封装类。

## 为啥要用这老掉牙的玩意？

主要是写外挂需要用到windows API，其实不用MFC也是可以的，比如说用QT，不过本文正好记录记录一下MFC开发Hello World级应用程序的方法。知道原理之后，用QT重新实现一下也就几分钟的事情。

## 环境配置

首先安装最新的稳定版visual studio,安装的时候记得勾选：**适用于最新v xxx 生成工具的 C++ MFC (x86和x64)；**

打开VS，创建一个新的MFC应用。

应用程序类型，我们选择基于对话框，这是最简单的MFC应用类型，适用于单页小功能app。可选静态/动态链接库。

进入VS后首先把目标程序设置为64位，不然创建线程时会踩坑。

## 界面绘制

打开资源文件->以.rc结尾的文件，打开之后，在资源视图里，Dialog里可以看到界面的设计，我们可以直接把里面预先放好的小部件删除。

选中主窗口，在属性里可以修改：边框：现在是Resizing，可以改成对话框外框，这样就修改不了窗口大小了。

然后点击工具箱，拖一个Button进去实验一下：

选择Button，在属性栏里可以调整小部件的属性，比如说描述文字：可以改成：打开bing，保存之后效果就可以看到了。

然后也可以修改控件的ID，最好保持格式不变，可以把ID改成 IDC_OPENBING.

## 点击事件处理

要让MFC控件的事件被正确的处理，我们需要创建一个对应的函数和点击事件相绑定。

首先我们打开Resource.h，这里可以找到一行：

```c++
#define IDC_OPENBING                    1000
```

这就是我们在创建控件的时候，MFC自动为我们生成的对应控件的ID。

然后我们在窗口类里创建一个成员函数，用于被点击时执行。

在对话框类的头文件里，添加一个成员函数的声明，例如：

```c++
class CPVZCheckerRDlg : public CDialogEx
{
......省略
protected:
	afx_msg void OnBtnOpenBingClicked();
```

前面的afx_meg是一个宏，没有实际作用，只是指示我们这是一个消息处理函数。

然后在cpp文件里放进去具体的实现，这里先不写逻辑，把壳搭建好：

```c++
void CPVZCheckerRDlg::OnBtnOpenBingClicked()
{
}
```

然后在宏：`BEGIN_MESSAGE_MAP(CPVZCheckerRDlg, CDialogEx)` 里加入按键绑定（该宏一般在`DoDataExchange`函数下面）

```c++
BEGIN_MESSAGE_MAP(CPVZCheckerRDlg, CDialogEx)
	......
	ON_BN_CLICKED(IDC_OPENBING, OnBtnOpenBingClicked)
END_MESSAGE_MAP()
```

这样，一个完整的事件处理过程就搭建好了，可以在函数内部打一个断点，然后调试执行试试能不能进入断点即可。

如果想要有一个Mssagebox的效果的话，还不是特别的简单，这里我提供一个宏，用来实现调试输出：

```c++
#define log(fmt, ...)                     \
  CString cstr;                           \
  cstr.Format(CString(fmt), __VA_ARGS__); \
  AfxMessageBox(cstr)

```

把这个宏放到某个头文件里即可，然后使用`log()`即可调试输出。

```c++
	CString name("MFC开发");
	log("hello, %s", name);
```

当然了，这个按钮说了要打开Bing嘛，那肯定不能只是打印log就算了，打开某个网页的话，使用windows api提供的一个函数即可：

```c++
::ShellExecute(nullptr,
	               CString("open"),
	               CString("https://www.bing.com/?mkt=zh-CN"),
	               nullptr,
	               nullptr,
	               SW_SHOWNORMAL
	);
```

说了这么多，基本都是在介绍MFC的使用，下文我将把内容转到如何修改游戏上来。