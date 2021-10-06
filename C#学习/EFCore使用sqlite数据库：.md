# EFCore使用sqlite数据库：

先用 Navicat 创建一个新的sqlite数据库文件，设计好简单的表，然后假如保存位置为 E:\StockTradingNotes.db
安装依赖：
microsoft.entityframeworkcore
microsoft.entityframeworkcore.analyzers
microsoft.entityframeworkcore.design
microsoft.entityframeworkcore.sqlite
microsoft.entityframeworkcore.tools

保存并关闭后重新打开项目，在ORM解决方案所在目录打开powershell，执行：
dotnet ef dbcontext scaffold "Data Source=E:\StockTradingNotes.db" Microsoft.EntityFrameworkCore.Sqlite -o Models

即可得到上下文对象，操作数据库时使用 var db = new StockTradingNotesContext() 即可