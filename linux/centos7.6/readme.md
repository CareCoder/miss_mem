```
  开启虚拟机的网卡

  ip addr#查看ens33网卡
  vi /etc/sysconfig/network-scripts/ifcfg-ens33
  修改ONBOOT=yes
  service network restart

  追加修改hosts
  echo "172.26.143.209  k8s-slb.biaoguoworks.com" >> /etc/hosts
```
