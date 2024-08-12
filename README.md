rinetd, by Thomas Boutell and Sam Hocevar. Released under the terms
of the GNU General Public License, version 2 or later.

This program is used to efficiently redirect connections from one IP
address/port combination to another. It is useful when operating virtual
servers, firewalls and the like.

To build under Unix, run `./bootstrap` to create the configuration
files, then `./configure` to create the build files, and then type
`make` to build rinetd. To install, type `make install` as root.

For documentation run `make install`, then type `man rinetd` for
details. Or, read `index.html` in your browser.


此程序用于有效地重定向来自一个IP的连接
地址/端口组合到另一个。在操作虚拟机时非常有用
例如服务器、防火墙等。

要在Unix下构建，请运行`/bootstrap创建配置
文件，然后/configure创建构建文件，然后键入
`制造溜冰场。要安装，请以root身份键入“make install”。

对于文档，运行“make install”，然后键入“man rinetd”
细节。或者，在浏览器中阅读`index.html`。

1、安装
yum install gcc
make && make install
EOF
chmod +x rinetd-installer.sh
./rinetd-installer.sh

2、配置
配置端口转发的配置文件在/etc/rinetd.conf

配置文件格式

#
[Source Address] [Source Port] [Destination Address] [Destination Port]

源地址 源端口 目的地址 目的端口
ex:
10.0.1.11 80  192.168.1.11 8080
#
在每一单独的行中指定每个要转发的端口。源地址和目的地址都可以是主机名或IP地址，IP 地址0.0.0.0将rinetd绑定到任何可用的本地IP地址上。例如：0.0.0.0 8080 wuweixiang.cn 80

