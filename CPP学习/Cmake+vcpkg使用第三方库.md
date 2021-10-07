# Cmake+vcpkg使用第三方库

### 为啥要这么麻烦？nuget不香吗？

还真不香，不知道为啥，我在visual studio里用nuget安装的第三方包之后，在#include的时候死活找不到，见了鬼了。

### 那咋办？

首先要安装vcpkg，参考([microsoft/vcpkg: C++ Library Manager for Windows, Linux, and MacOS (github.com)](https://github.com/Microsoft/vcpkg))给出的办法：

新建文件夹：`C:\dev`，在此处clone程序包，并运行脚本程序：

```shell
git clone https://github.com/microsoft/cpprestsdk.git
.\vcpkg\bootstrap-vcpkg.bat
#在管理员权限下运行下方代码（visual studio 用）
.\vcpkg\vcpkg integrate install
```

此处以cpprestsdk这个包为例子，举例如何使用：首先在vcpkg里安装

```powershell
PS C:\dev\vcpkg\vcpkg.exe install cpprestsdk cpprestsdk:x64-windows
```

安装完成之后，创建一个cmake解决方案：使用Clion打开：

在Clion-> Settings -> Build, xxxx   -> CMake设置里，有一个CMake options 添加一行：并保存

```
-DCMAKE_TOOLCHAIN_FILE=C:/dev/vcpkg/scripts/buildsystems/vcpkg.cmake"
```

这样，CMAKE就可以和vcpkg联合使用起来了。

我们编辑项目的CMakeLists.txt，现有的都不管主要添加一下两行，这样就可以了。

```cmake
cmake_minimum_required(VERSION 3.0)
project(clionDemo1)

set(CMAKE_CXX_STANDARD 14)
find_package(cpprestsdk REQUIRED)   #添加
add_executable(${PROJECT_NAME} main.cpp)
target_link_libraries(${PROJECT_NAME} PRIVATE cpprestsdk::cpprest)  #添加

```

在main.cpp里试一下吧！

```c++
#include <iostream>
#include <cpprest/http_client.h>
int main()
{
    web::http::client::http_client client(U("https://postman-echo.com/get?a=b"));
    auto rsp = client.request(web::http::methods::GET).get();
    auto body = rsp.extract_string().get();
    std::wcout << rsp.status_code() << "\n" << body << std::endl;
}
```

运行，万事大吉！

### 在Qt Creator中使用Boost

```
-DCMAKE_TOOLCHAIN_FILE=C:/dev/vcpkg/scripts/buildsystems/vcpkg.cmake
```

上面这一行加入到CMake初始化参数里。

然后在cmakeLists里添加：

```
find_package(Boost REQUIRED)
target_link_libraries(${PROJECT_NAME} Qt${QT_VERSION_MAJOR}::Core Boost::boost)
```

这两行都是要单独的，不能分开 如果用其他的库，使用下面这种形式：

```
find_package(Boost REQUIRED [COMPONENTS <libs>...])
target_link_libraries(main PRIVATE Boost::boost Boost::<lib1> Boost::<lib2> ...)
```

