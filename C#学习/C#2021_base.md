## Linq

```c#
public static void LinqTest()
        {
            var numbers = new int[7] { 2, 3, 4, 5, 6, 7, 8 };
            var querynum = from num in numbers where num % 2 == 0 select num;
            var res = querynum.ToList();
            foreach (var item in res)
            {
                Console.WriteLine(item);
            }
            // anotherquery
            var anotherquery = numbers.Where(num => num % 2 == 0).ToList();
            foreach(var item in anotherquery)
            {
                Console.WriteLine(item);
            }
        }
```



## 集合框架

ArrayList， 不支持泛型，尽量少用，可以List<T>代替，若待处理数据为不同的类型，可以用ArrayList处理



### 打包dll文件到exe

```
<IncludeNativeLibrariesForSelfExtract>True</IncludeNativeLibrariesForSelfExtract>
```



### System.Collections.Generic.Stack<T>

​	栈类数据结构：后进先出。

- [Push](https://docs.microsoft.com/zh-cn/dotnet/api/system.collections.generic.stack-1.push?view=net-6.0) 在顶部插入一个元素 [Stack](https://docs.microsoft.com/zh-cn/dotnet/api/system.collections.stack?view=net-6.0) 。
- [Pop](https://docs.microsoft.com/zh-cn/dotnet/api/system.collections.generic.stack-1.pop?view=net-6.0) 从顶部移除一个元素 [Stack](https://docs.microsoft.com/zh-cn/dotnet/api/system.collections.generic.stack-1?view=net-6.0) 。
- [Peek](https://docs.microsoft.com/zh-cn/dotnet/api/system.collections.generic.stack-1.peek?view=net-6.0) 返回位于顶部的元素，但不将 其 从 [Stack](https://docs.microsoft.com/zh-cn/dotnet/api/system.collections.generic.stack-1?view=net-6.0)中移除。
- Clear， 清除所有元素。
- Count，属性，报告栈内有多少元素。



### System.Collections.Generic.Queue<T>

队列，先进先出，用于资源竞争类问题。

- [Enqueue](https://docs.microsoft.com/zh-cn/dotnet/api/system.collections.generic.queue-1.enqueue?view=net-6.0) 将一个元素添加到的末尾 [Queue](https://docs.microsoft.com/zh-cn/dotnet/api/system.collections.generic.queue-1?view=net-6.0) 。
- [Dequeue](https://docs.microsoft.com/zh-cn/dotnet/api/system.collections.generic.queue-1.dequeue?view=net-6.0) 从的开头移除最旧的元素 [Queue](https://docs.microsoft.com/zh-cn/dotnet/api/system.collections.generic.queue-1?view=net-6.0) 。
- [Peek](https://docs.microsoft.com/zh-cn/dotnet/api/system.collections.generic.queue-1.peek?view=net-6.0) 返回位于开始处的最早的元素，但不将其从Queue中移除 。



### System.Collections.Generic.Dictionary<TKey,TValue>

字典，键值对的集合：

### System.Collections.Generic.List<T>

泛型列表：

```c#
public static void ListTest()
        {
            List<int> list = new List<int>();
            list.Add(1);
            list.Add(2);
            list.Add(3);
            list.ForEach(l => Console.WriteLine(l));  //用Foreach来实现遍历列表
        } 
```





### SQLSERVER

安装软件：

* SqlServer Express 2019
* Sql Server mangement Studio (SSMS)

解决方案安装依赖：

* Microsoft.EntityFrameworkCore
* Microsoft.EntityFrameworkCore.Analyzers
* Microsoft.EntityFrameworkCore.Design
* Microsoft.EntityFrameworkCore.SqlServer
* Microsoft.EntityFrameworkCore.Tools

获得连接字符串：

```
Provider=SQLNCLIRDA11;Data Source=LAPTOP-0EP1O0UN\SQLEXPRESS;Integrated Security=SSPI;Initial Catalog=Northwind;
```

Model First ：在项目所在目录用powershell执行以下命令创捷Models:

```shell
dotnet tool install --global dotnet-ef  // run once time 
dotnet ef dbcontext scaffold "Data Source=LAPTOP-0EP1O0UN\SQLEXPRESS;Integrated Security=SSPI;Initial Catalog=Northwind;" Microsoft.EntityFrameworkCore.SqlServer -o Models
```



Q：出现“未能找到源类型 Dbset＜a＞的查询模式的实现”？

```c#
using System.Linq；
```



查找&添加：

```c#
static void Main(string[] args)
        {
            using(var db=new NorthwindContext())
            {
                var q = from c in db.Customers where c.Country == "USA" select c;
                q.ToList<Customer>().ForEach(c => Console.WriteLine(c));
                //查找国家是“USA”的所有成员。
                db.Customers.Add(new Customer
                {
                    CustomerId = "74237",
                    CompanyName = "Wuxi Apptec",
                    ContactName = "Xiaotian Lei",
                    ContactTitle = "researcher",
                    Address = "NT1-2241",
                    City = "qidong",
                    Region = "jiangsu",
                    PostalCode = "000000",
                    Country = "China",
                    Phone = "17775206935",
                    Fax = "00000"
                });
                db.SaveChanges();
                //添加一个成员
            }
        }
```



### Json

使用ServiceStack.Text包处理Json数据：

```c#
using ServiceStack;
using ServiceStack.Text;

class Person
    {
        public string Id { get; set; }
        public string Name { get; set; }
        public int Age { get; set; }

        public Person(string name, int age)
        {
            Id=System.Guid.NewGuid().ToString();
            Name=name;
            Age=age;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello World!");

            Person wang=new Person("xiaotian", 25);
            var leiStr = wang.ToJson();
            Console.WriteLine(leiStr);
            Person lei=leiStr.FromJson<Person>();
            Console.WriteLine(lei.Id);
            var obj = JsonObject.Parse(leiStr);
            var n1 = obj.Get<string>("Name");
            Console.WriteLine(n1);
            Console.WriteLine("============");
            var dynObj = DynamicJson.Deserialize(leiStr);
            var dynName=dynObj.Name;
            Console.WriteLine(dynName);
        }
    }
```

