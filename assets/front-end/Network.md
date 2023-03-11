# 网络

## OSI 七层模型

Open System Interconnection，从上到下依次为：

- 应用层：协议有：HTTP FTP TFTP SMTP SNMP DNS TELNET HTTPS POP3 DHCP
- 表示层：数据的表示、安全、压缩。格式有，JPEG、ASCll、DECOIC、加密格式等
- 会话层：建立、管理、终止会话。对应主机进程，指本地主机与远程主机正在进行的会话
- 传输层：定义传输数据的**协议端口号**，以及流控和差错校验。协议有：TCP UDP，数据包一旦离开网卡即进入网络传输层
- 网络层：进行**逻辑地址（如 IP 地址）**寻址，实现不同网络之间的路径选择。协议有：ICMP IGMP IP（IPV4 IPV6） ARP RARP
- 数据链路层：建立逻辑连接、进行硬件地址寻址、差错校验等功能。将比特组合成字节进而组合成帧，用**MAC 地址**访问介质，错误发现但不能纠正。
- 物理层：建立、维护、断开物理连接。

![OSI_1](.\images\OSI_1.png)

![OSI_2](.\images\OSI_2.png)

![OSI_3](.\images\OSI_3.png)