```
  开启虚拟机的网卡

  ip addr#查看ens33网卡
  vi /etc/sysconfig/network-scripts/ifcfg-ens33
  修改ONBOOT=yes
  service network restart
```
