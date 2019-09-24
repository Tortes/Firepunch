## Build Server on your VPS
### Scripts
The script of Teddysun is prefered to set up a server for the begginers. The script is [here](https://github.com/teddysun/shadowsocks_install). You could get the script with the `git` tools.

### Manually configure
#### [Ssserver](https://github.com/Tortes/shadowsocks)
- `yum install python-setuptools && easy_install pip`
- `yum install python34-pip git`
- 添加pip3.4的PATH到.bashrc
- `pip3.4 install  git+https://github.com/shadowsocks/shadowsocks.git@master`
- 创建配置文件`mkdir /etc/shadowsocks`, `cd /etc/shadowsocks`, `vi config.json`
- Here the `11234` is the port number and `Elpsycongroo` is the password.
```
{
    "server":"0.0.0.0",
    "local_address":"127.0.0.1",
    "local_port":1080,
    "port_password":{
        "11234":"Elpsycongroo"
   },
    "timeout":300,
    "method":"aes-256-gcm",
    "fast_open":true
}
``` 
#### 开启防火墙端口
- You should open the tcp and udp of the port in `/etc/shadowsocks/config.json`
```
firewall-cmd --permanent --zone=public --add-port=11234/tcp
firewall-cmd --permanent --zone=public --add-port=11234/udp
firewall-cmd --reload   #reload
firewall-cmd --list-all #check
```
#### 优化吞吐量
`vi /etc/sysctl.d/local.conf`
```
# max open files
fs.file-max = 51200
# max read buffer
net.core.rmem_max = 67108864
# max write buffer
net.core.wmem_max = 67108864
# default read buffer
net.core.rmem_default = 65536
# default write buffer
net.core.wmem_default = 65536
# max processor input queue
net.core.netdev_max_backlog = 4096
# max backlog
net.core.somaxconn = 4096

# resist SYN flood attacks
net.ipv4.tcp_syncookies = 1
# reuse timewait sockets when safe
net.ipv4.tcp_tw_reuse = 1
# turn off fast timewait sockets recycling
net.ipv4.tcp_tw_recycle = 0
# short FIN timeout
net.ipv4.tcp_fin_timeout = 30
# short keepalive time
net.ipv4.tcp_keepalive_time = 1200
# outbound port range
net.ipv4.ip_local_port_range = 10000 65000
# max SYN backlog
net.ipv4.tcp_max_syn_backlog = 4096
# max timewait sockets held by system simultaneously
net.ipv4.tcp_max_tw_buckets = 5000
# turn on TCP Fast Open on both client and server side
net.ipv4.tcp_fastopen = 3
# TCP receive buffer
net.ipv4.tcp_rmem = 4096 87380 67108864
# TCP write buffer
net.ipv4.tcp_wmem = 4096 65536 67108864
# turn on path MTU discovery
net.ipv4.tcp_mtu_probing = 1

net.ipv4.tcp_congestion_control = bbr
```
- `sysctl --system`
- `systemctl daemon-reload`
- `systemctl restart shadowsocks-server`

#### 开机启动
- `vi /etc/init.d/shadowsocks`配置init.d启动脚本
```
#!/usr/bin/env bash
#chkconfig: 2345 90 10
#description:auto_run
nohup ssserver -c /etc/shadowsocks/config.json > /dev/null 2>&1 &
```
- `chkconfig --add shadowsocks`

#### [Libsodium](https://www.jb51.net/os/RedHat/541431.html): Support for chacha20
```
yum install m2crypto gcc -y
wget -N --no-check-certificate https://download.libsodium.org/libsodium/releases/libsodium-1.0.18-stable.tar.gz
tar -zxvf libsodium-1.0.18-stable.tar.gz 
cd libsodium-stable
./configure
make && make install
echo "include ld.so.conf.d/*.conf" > /etc/ld.so.conf
echo "/lib" >> /etc/ld.so.conf
echo "/usr/lib64" >> /etc/ld.so.conf
echo "/usr/local/lib" >> /etc/ld.so.conf
ldconfig
```