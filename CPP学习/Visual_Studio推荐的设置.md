在解决方案资源管理器中， 保持【显示所有文件】按钮处于按下状态，这样就不需要使用自带的过滤器了（那个真没啥用）。

右键项目，选择属性，配置选所有配置，平台选所有平台，然后修改下面的配置项目：

输出目录：`$(SolutionDir)\bin\$(Platform)\$(Configuration)\`

中间目录：`$(SolutionDir)\bin\intermeidates\$(Platform)\$(Configuration)\`

