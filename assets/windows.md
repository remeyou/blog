# Windows

## Windows 10 快速访问异常：右键单击闪退

**清除快速访问栏的缓存文件：** 这几天用的 Windows10 系统遇到一个莫名奇妙的问题，就是在快速访问里总是固定了下载文件夹，而且无法取消固定，其它文件夹都正常就是它不能取消固定，始终顽固的显示在快速访问里。即使我在“查看|选项|隐私"里取消对”在‘快速访问’中显示最近使用的文件”和“在‘快速访问’中显示常用文件夹”的勾选，再清除文件资源管理器历史记录最不行。 最后在询问了度娘 N 多关键词后终于在一个解决其它快速访问功能异常的问答中找到了解决我这个问题的答案。 在文件资源管理器地址栏输入：%APPDATA%\Microsoft\Windows\Recent\AutomaticDestinations 打开的文件夹里找到文件：f01b4d95cf55d32a.automaticDestinations-ms 删除它，或者安全起见你可以剪切它到其它地方，再重新打开文件资源管理器，问题就这么简单的解决了：）

ref: [Windows 10 快速访问异常:无法取消固定,的解决方法](https://blog.csdn.net/yin0hao/article/details/88052343)

**闪退根本原因：** 部分软件的右键菜单不兼容引起，尝试分别禁用软件的右键菜单

ref: [【win10】win10 右键快速访问等文件夹导致资源浏览器崩溃的处理方法](https://blog.csdn.net/sunjinshengli/article/details/82977516?utm_medium=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.add_param_isCf&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.add_param_isCf)

删除任务栏里已经卸载的软件图标

1、按“Win+R”组合键，输入“regedit”打开注册表编辑器；
2、然后打开如下键值：HKEY_CLASSES_ROOT\Local Settings\Software\Microsoft\Windows\CurrentVersion\TrayNotify
3、在右边可以看到两个键值：IconStreams 和 PastIconsStream，将它们的值删除。
4、然后调出任务管理器将进程“explorer.exe”终止，再在任务管理器中点击“文件——新建任务”；
5、输入“explorer”，回车，以此重新启动该系统进程 。(或者重新启动计算机）。
现在再来查看一下通知区域的图标，过期的图标已经被成功清理了。

## Windows 11 Internet 选项

```bash
inetcpl.cpl
```

## 解除 localhost:80 占用

```bash
# 管理员
net stop http
```

## Office 2019 Activation

1. Click ”清除激活状态“
2. Click “安装许可证“ & ”应用服务器地址“（before 检测 KMS 可用性）
3. Click ”激活“

> Office 专业增强版 2019
>
> Key: NMMKJ-6RK4F-KMJVX-8D9MJ-6MWKP
>
> KMS server address: kms.v0v.bid

ref: [Windows 系统一句命令激活](https://v0v.bid/)

## 使用 Sandboxie Plus 运行 QQ 桌面版

ref: [一文教您使用 Sandboxie Plus 运行 QQ 桌面版](https://ericclose.github.io/run-QQ-with-Sandboxie-Plus.html)

## QTTabBar 配置

配置 Windows Terminal

```plaintext
# Path: C:\Program Files\WindowsApps\Microsoft.WindowsTerminal_1.10.2383.0_x64__8wekyb3d8bbwe\wt.exe
Arguments: -d %c%
```

## 7-zip

ref: [https://www.cnblogs.com/conorblog/p/14543286.html](https://www.cnblogs.com/conorblog/p/14543286.html)

> 配置
>
> 下载安装 7z：官网 [https://www.7-zip.org/](https://www.7-zip.org/)
>
> 配置环境变量：win 键按下，搜索 env，打开编辑环境变量，选择环境变量，在系统变量下的 path 中添加你的 7zip 安装位置，如 C:\Program Files\7-Zip\，一路 OK 确认，关闭窗口
>
> 检查可用性：打开 cmd，输入 7z 命令，查看是否可用
>
> 压缩
>
> 7z a -t[format] archive_name file_name
>
> 参数 a 表示加进压缩包
>
> -t[format] 表示压缩包格式，自己指定，如 -tzip 为 zip 压缩包
>
> archive_name 压缩包名字
>
> file_name 文件名，带扩展名，可以一个一个罗列出来，也可以用通配符，如
>
> \*.txt 匹配所有 txt 文件
>
> _._ 匹配所有文件
>
> 举例：将当前目录下所有 docx 文件打包压缩，压缩包名为 archive_name.zip
>
> 7z a -tzip archive_name.zip \*.docx
>
> 解压
>
> 参数有两种：一个是 e，一个是 x
>
> 区别：e 解压出来的没有文件夹结构，x 解压出来的有文件夹结构
>
> 一般都用 x
>
> 下面说明 x 的语法
>
> 7z x -o[output_dir] archive_name
>
> -o[output_dir] 输出文件夹，举例：-otest 表示当前目录下的 test 文件夹下，不写就是当前目录
>
> 注意输出 -o 和文件夹名称要连着写，中间没有空格
>
> 举例：解压缩 archive.zip 包到当前目录下的 source_file 文件夹下
>
> 7z x -osource_file archive.zip
>
> 查看文件
>
> -l 表示列出所有文件 (英文字母 L)
>
> 7z l test.zip

ref: [https://tech.gjlmotea.com/2021/07/windows7-zip.html](https://tech.gjlmotea.com/2021/07/windows7-zip.html)
指定编码解压

> 7z x file.zip -mcp=932
> 簡中 936
> 日文 932
> [對照表](https://www.wikiwand.com/en/Code_page#/DOS_code_pages)

## PowerShell 历史记录文件

%userprofile%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt
