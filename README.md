# Rinetd - 高效的IP重定向工具

由Thomas Boutell和Sam Hocevar开发，Rinetd根据[GNU通用公共许可证第二版或更高版本](https://www.gnu.org/licenses/gpl-2.0.html)的条款发布。该程序用于高效地将连接从一个IP地址/端口组合重定向到另一个，特别适用于操作虚拟服务器、防火墙等。

## 构建和安装

在Unix下构建Rinetd，请按照以下步骤操作：

1. 运行`./bootstrap`创建配置文件。
2. 运行`./configure`创建构建文件。
3. 输入`make`构建Rinetd。
4. 输入`make install`以root身份安装Rinetd。

## 文档

安装后，运行`make install`，然后输入`man rinetd`查看详细信息，或者在浏览器中阅读`index.html`。

## 中文说明

此程序用于有效地将来自一个IP地址/端口组合的连接重定向到另一个，在操作虚拟服务器、防火墙等时非常有用。

在Unix下构建，请运行`/bootstrap`创建配置文件，然后`/configure`创建构建文件，然后键入`make`来构建Rinetd。要安装，请以root身份键入`make install`。

对于文档，运行`make install`，然后键入`man rinetd`获取详细信息。或者，在浏览器中阅读`index.html`。

## 安装步骤

1. **安装依赖**
   ```shell
   yum install gcc make
   make && make install
   
2、**设置权限**

 ```shell
chmod +x rinetd-installer.sh
./rinetd-installer.sh

3、**配置**

端口转发的配置文件位于/etc/rinetd.conf。
rinetd.conf文件的格式如下：
[源地址] [源端口] [目的地址] [目的端口]
例如：10.0.1.11 80 192.168.1.11 8080
在每一行中指定每个要转发的端口。源地址和目的地址可以是主机名或IP地址。使用IP地址0.0.0.0将Rinetd绑定到任何可用的本地IP地址。


 ```shell
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

4、**创建启动脚本**

 ```shell
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


5、**启动服务**

 ```shell
/etc/init.d/rinetd start

6、**设置开机启动**
在/etc/rc.local文件中添加/usr/sbin/rinetd 或 /usr/sbin/rinetd -c /etc/rinetd.conf 启动命令。

注意事项
确保rinetd.conf中绑定的本机端口没有被其他程序占用。
