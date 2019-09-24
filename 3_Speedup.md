# Speed up your shadowsocks
## [BBR](https://teddysun.com/489.html)

- `wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh && chmod +x bbr.sh && ./bbr.sh`
- `uname -r` 查看是否为最新内核  
- `sysctl net.ipv4.tcp_available_congestion_control`检查返回值是否为`net.ipv4.tcp_available_congestion_control = bbr cubic reno`
### 更新header
- `yum remove kernel-headers`
- `yum --enablerepo=elrepo-kernel -y install kernel-ml-headers`
- `yum install gcc gcc-c++`


## [魔改BBR](https://github.com/tcp-nanqinlang/wiki/wiki/manual-centos)
- 无法`yum groupinstall`时，`yum clean all`
- 需要gcc进行编译
- `rpm -qa | grep kernel | grep -v "4.12.10"` 检查内核，删除, `yum remove -y “内核名”`
- 安装新内核
```
wget https://raw.githubusercontent.com/nanqinlang/CentOS-kernel/master/kernel-ml-4.12.10-1.el7.elrepo.x86_64.rpm && yum install -y kernel-ml-4.12.10-1.el7.elrepo.x86_64.rpm
wget https://raw.githubusercontent.com/nanqinlang/CentOS-kernel/master/kernel-ml-devel-4.12.10-1.el7.elrepo.x86_64.rpm && yum install -y kernel-ml-devel-4.12.10-1.el7.elrepo.x86_64.rpm 
wget https://raw.githubusercontent.com/nanqinlang/CentOS-kernel/master/kernel-ml-headers-4.12.10-1.el7.elrepo.x86_64.rpm && yum  install -y kernel-ml-headers-4.12.10-1.el7.elrepo.x86_64.rpm
grub2-mkconfig -o /boot/grub2/grub.cfg && grub2-set-default 0
```
- `reboot` 重启后删除原来的内核
```
yum groupinstall -y "Development Tools"
wget -O Makefile https://raw.githubusercontent.com/tcp-nanqinlang/general/master/Makefile/Makefile-CentOS
wget https://raw.githubusercontent.com/tcp-nanqinlang/general/master/General/CentOS/source/tcp_nanqinlang.c
make && make install
sed -i '/net\.ipv4\.tcp_congestion_control/d' /etc/sysctl.conf
echo -e "\nnet.ipv4.tcp_congestion_control=nanqinlang\c" >> /etc/sysctl.conf && sysctl -p
```
- Check `lsmod | grep nanqinlang`

## [Lotserver](https://github.com/MoeClub/lotServer)