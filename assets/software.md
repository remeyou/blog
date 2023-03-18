# Software

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

> 7z x file.zip -mcp=936
> 簡中 936
> 日文 932
> [對照表](https://www.wikiwand.com/en/Code_page#/DOS_code_pages)

## Powershell 历史记录文件

%userprofile%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt
