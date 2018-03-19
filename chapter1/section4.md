# C#磁盘遍历——递归

```
static void Main(string[] args)
{
    //创建秒表，记录查询的总时间
    Stopwatch timer = new Stopwatch();
    timer.Start();
    //传入本地磁盘路径，遍历当前路径下的所有文件
    LoadDirectory(@"G:\传智播客.Net培训—就业班精品");
    timer.Stop();
    Console.WriteLine("运行时间：" + timer.Elapsed.Seconds);
    Console.WriteLine("文件个数：" + count);
    
    Console.ReadKey();

}
//记录总文件数
static int count = 0;

/// <summary>
/// 遍历指定路径中的文件
/// </summary>
/// <param name="path"></param>
static void LoadDirectory(string path) {
    
    //获取指定路径下的文件夹
    DirectoryInfo[] dirs = dirInfo.GetDirectories();
    foreach (DirectoryInfo item in dirs)
    {
        count++;
        Console.WriteLine(item.Name);
        //递归调用
        LoadDirectory(item.FullName);
    }
    
    //获取指定路径下的文件
    FileInfo[] files = dirInfo.GetFiles();
    if (files.Length > 0)
    {
        foreach (FileInfo item in files)
        {
            count++;
            Console.WriteLine("-" + item.Name);
        }
    }
}
```