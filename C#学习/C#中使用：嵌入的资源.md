# C#中使用：嵌入的资源

在C#中使用资源有很多种方式，如果想把资源直接打包到二进制文件中，可以使用：【嵌入的资源】

下文以一个类库为例，假如我们新建一个`ShijingProducer`的`netstandard2.0`类库，用于生产诗经数据的对象列表。数据源是“`shijing.json`”文件，把该文件复制到项目中后，右键：属性：生成操作：嵌入的资源。

```c#
public static List<Shijing> GetAllShijing()
        {
            //获取当前程序集的资源
            Assembly assembly = Assembly.GetExecutingAssembly();
            //从资源里读取信息到流里，ShijingProducer是项目名称，jsons是文件夹名称，shijing.json是文件名称
            var fileStream = assembly.GetManifestResourceStream("ShijingProducer.jsons.shijing.json");
            var sr = new StreamReader(fileStream);
            var sc = sr.ReadToEnd();
            var list = JsonNet.Deserialize<List<Shijing>>(sc);
            return list;
        }
```

