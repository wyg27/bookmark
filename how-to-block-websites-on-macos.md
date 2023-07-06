# 如何利用 macOS 内置的 pf 程序屏蔽网站或广告

关于 pf 程序的简介：

> 是的，我了解 macOS 中的 `pf`。`pf` 是 macOS（以及其他类似基于 BSD 的操作系统）中的一个内置软件包过滤器，它是一种功能强大的网络包过滤工具。`pf` 的全称是 Packet Filter，它提供了防火墙、网络地址转换（NAT）和网络流量控制的功能。
>
> `pf` 允许你定义规则，以控制网络数据包的流向和处理方式。你可以使用 `pf` 来设置入站和出站的防火墙规则，过滤特定类型的流量，实现端口转发和网络地址转换，以及进行流量限制和负载均衡等功能。
>
> `pf` 使用一个配置文件（通常是 `/etc/pf.conf`），你可以在其中定义规则和选项。配置文件中的规则可以基于源地址、目标地址、端口号、协议类型等进行过滤和匹配。你可以通过编辑配置文件来自定义 `pf` 的行为，并使用命令行工具来加载和激活配置。
>
> 要了解更多关于 `pf` 的信息，你可以在终端中输入 `man pfctl` 查看 `pfctl` 的手册页，或者参考苹果的官方文档。
 
首先，确保在 `/etc` 目录下有文件 `/etc/pf.conf` 和目录 `/etc/pf.anchors`。

有些教程会建议你直接在 `/etc/pf.conf` 中添加规则，但个人建议是不要这样做，原因如下：

* 当 macOS 有新的升级时会覆盖 `/etc/pf.conf`，导致你的规则失效了。
* 如果将 `/etc/pf.conf` 弄乱了，可能会影响正常访问网络。

所以，请遵循以下步骤；在每个步骤中，我会说明为什么要这么做。（注：每一步中的操作都需要用 `root` 账号来完成，也就是使用 `sudo` 命令。）

## 步骤1

在目录 `/etc` 下创建一个新文件，文件名可以随意起，我喜欢的文件名是 `custom_pf.conf`。这个文件用于代替默认的配置文件 `pf.conf`。新文件的内容如下：

```
anchor "org.yiguowang.pf"
load anchor "org.yiguowang.pf" from "/etc/pf.anchors/org.yiguowang.pf.rules"
```
内容中的 `org.yiguowang.pf` 可以换成任何你喜欢的字符串，它的作用只是一个“label“。重要的是 `/etc/pf.anchors/org.yiguowang.pf.rules` 这个文件路径，该文件用来存放我们需要过滤规则。

## 步骤2

在目录 `/etc/pf.anchors` 下创建名为 `org.yiguowang.pf.rules`的文件（见步骤1）。文件的内容是：

```
block drop out quick from any to baidu.com
block drop out quick from any to cctv.com
```
这两条规则屏蔽了两个我最讨厌的网站，请根据你的需要进行修改。

## 步骤3

执行命令 `sudo pfctl -v -n -f /etc/custom_pf.conf`。如果执行的结果是报错，说明 `custom_pf.conf` 或 `/etc/pf.anchors/org.yiguowang.pf.rules` 中的内容格式错误，请检查并修改；直至命令顺利执行。

## 步骤4

最后一步就是让 pf 在每次电脑开机时都自动加载我们自定义的规则。

进入目录 `cd /Library/LaunchDaemons`，创建一个文件 `org.custom-pf-conf.startup.plist`（文件名随意起，后缀是 plist 就行）。文件内容如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple Computer//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>Label</key>             <string>org.custom-pf-conf.startup</string>
    <key>Disabled</key>          <false/>
    <key>RunAtLoad</key>         <true/>
    <key>KeepAlive</key>         <false/>
    <key>LaunchOnlyOnce</key>    <true/>
    <key>ProgramArguments</key>
        <array>
            <string>/sbin/pfctl</string>
            <string>-e</string>
            <string>-f</string>
            <string>/etc/pf.anchors/org.yiguowang.pf.conf</string>
        </array>
</dict>
</plist>
```
上述配置中，你只需要修改 `<string>/etc/pf.anchors/org.yiguowang.pf.conf</string>` 中的路径就行了。

到此，pf 的配置就结束了。目前，我们只是利用 pf 来屏蔽讨厌的网站，其实它的功能远不止这些，如果有兴趣可以自行探索。

## 修改 pf 配置文件后如何让新规则生效？

修改 pf 的配置文件后，只需要执行下列命令即可：

```
sudo pfctl -f /etc/custom_pf.conf
```
这条命令告诉 pfctl（pf 控制工具）读取配置文件 `/etc/custom_pf.conf` 中的规则，并将其应用到当前的 pf 配置中。这样，你就可以更新 pf 的规则并使其生效。

请注意，在重新加载规则之前，确保你已经保存了修改后的配置文件。如果在配置文件中存在语法错误，pfctl 将不会加载新的规则，并显示相应的错误消息。在这种情况下，你需要修复错误，并重新运行上述命令。

## 参考

* https://blog.scottlowe.org/2013/05/15/using-pf-on-os-x-mountain-lion/
* https://blog.chionlab.moe/2016/02/01/use-pf-on-osx/
* https://lvv.me/posts/2022/09/27_pfctl_on_macos/
* https://www.noxxxx.com/pf-on-mac-os-x.html
* https://imagasaikou.cn/blog/12
