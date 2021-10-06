# C#如何调用C++(基础篇)

## **一、创建一个C++类，例如：**

AddOperate.h

```
extern "C" _declspec(dllexport) int Sum(int a, int b);
class AddOperate
{
public :
};
```

AddOperate.cpp

```
#include "AddOperate.h"
#include "iostream"
using namespace std;

int Sum(int a, int b)
{
    if (a - (int)a != 0 || b - (int)b != 0) {
        cout << "请输入整数" << endl;
        return -1;
    }
    return a + b;
}
```

## 2、将C++代码编译成动态库dll

A:项目--属性---配置属性--常规---配置类型---动态库(.dll)

B：项目--属性--配置属性--C/C++---高级---编译为---编译为C++代码（/TP）

项目--生成，就会看到dll了，把这个dll放到c#程序输出目录

## **编写C#代码调用dll**

```
		[DllImport("dll名称.dll", CallingConvention = CallingConvention.Cdecl)]
        extern static int Sum(int a, int b);
        //.....下面是其他代码
        public static void Main(string[] args) {

        }
```

