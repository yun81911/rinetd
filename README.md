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

rinetd.conf配置文件格式

#
[Source Address] [Source Port] [Destination Address] [Destination Port]

源地址 源端口 目的地址 目的端口
ex:
10.0.1.11 80  192.168.1.11 8080

在每一单独的行中指定每个要转发的端口。源地址和目的地址都可以是主机名或IP地址，IP 地址0.0.0.0将rinetd绑定到任何可用的本地IP地址上。例如：0.0.0.0 8080 wuweixiang.cn 80

'''
rm -f /etc/rinetd.conf

cat >> /etc/rinetd.conf <<EOF

*# 设置允许访问的ip地址信息

*# allow 192.168.2.*

*# 设置拒绝访问的ip地址信息

*# deny 192.168.1.*

*# 设置日志文件路径

logfile /var/log/rinetd.log

*# 例子: 将本机 8080 端口重定向至 188.131.152.100 的 8080 端口

*# 0.0.0.0 8090 188.131.152.100 8080

EOF
'''

3、创建启动脚本
'''
cat >> /etc/init.d/rinetd <<'EOF'
#!/bin/bash

EXEC=/usr/sbin/rinetd
CONF=/etc/rinetd.conf
PID_FILE=/var/run/rinetd.pid
NAME=Rinetd
DESC="Rinetd Server"

case "$1" in
    start)
        if [ -x "$PID_FILE" ]; then
            echo "$NAME is running ..."
            exit 0
        fi

        $EXEC -c $CONF

        echo -e "\e[1;32m$NAME is running\e[0m"
    ;;
    stop)
        if [ -f "$PID_FILE" ]; then
            kill `cat $PID_FILE`

            while [ -x "$PID_FILE" ]
            do
                echo "Waiting for $NAME to shutdown..."  
                sleep 1
            done

            rm -f $PID_FILE
        fi

        echo -e "\e[1;31m$NAME stopped.\e[0m"
    ;;
    restart)
        $0 stop
        $0 start
    ;;
    status)
        if [ -f $PID_FILE ]; then
            echo "$NAME is running ..."
        else
            echo "$NAME stopped."
        fi
    ;;
    *)
        echo $"Usage: $0 {start|stop|restart|status}"
        exit 2
    ;;
esac

exit 0
EOF
'''

4、启动服务
'''
/etc/init.d/rinetd start
'''
5、开机启动 
 
在/etc/rc.local 文件中，添加/usr/sbin/rinetd 或者 /usr/sbin/rinetd -c /etc/rinetd.conf 启动命令即可。
 
6、需要注意

rinetd.conf中绑定的本机端口必须没有被其它程序占用
 



