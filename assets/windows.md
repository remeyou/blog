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
