# C#获取奇怪的编码的HTTP content

近日在使用新浪财经提供的API接口时，获取到的 HTTP content 编码方式是 GB18030，要解析这种编码，需要使用以下方法：

nuget 安装包： `System.Text.Encoding.CodePages`



```c#

System.Text.Encoding.RegisterProvider(
    System.Text.CodePagesEncodingProvider.Instance);

HttpClient HttpClient = new HttpClient();
var contentStream = await HttpClient
    .GetStreamAsync("http://hq.sinajs.cn/list=sz002307");
var encode = System.Text.Encoding.GetEncoding("GB18030");
string s1 = string.Empty;
using (var myStreamReader = new StreamReader(contentStream, encode))
{
    s1 = myStreamReader.ReadToEnd();
}
//下面是解析API结果
var ss1 = s1.Split('=')[1].Split(',');
for (int i = 0; i < ss1.Length; i++)
{
    Console.WriteLine(i + " " + ss1[i]);
}
```

