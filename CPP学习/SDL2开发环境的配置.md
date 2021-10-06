首先去下载sdl2：

[Simple DirectMedia Layer - SDL version 2.0.16 (stable) (libsdl.org)](https://www.libsdl.org/download-2.0.php)

如果不需要看源码的话，只需要下载Development Libraries:

然后解压，放到例如：`C:\dev\SDL2` 文件夹下；

右键项目，选择属性->VC++目录

包含目录：C:\dev\SDL2\include;$(IncludePath)

引用目录： C:\dev\SDL2\lib\$(Platform);$(LibraryPath)

链接器->输入->附加依赖项：

SDL2.lib
SDL2main.lib
SDL2test.lib

再把lib里所有的dll文件复制到生成目录下，和exe放在一起即可。

