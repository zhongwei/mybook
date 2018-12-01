# This is my handbook

## Linux book

### Linux Admin

#### Network Admin

- 两块网卡时，修改路由表优先级

```shell
    sudo ip route del default via 192.168.99.1 dev enp0s8
    sudo ip route add default via 192.168.99.1 dev enp0s8 proto static metric 101
```
```yaml
#/etc/netplan/50-cloud-init.yaml
network:
    ethernets:
        enp0s3:
            addresses: []
            dhcp4: true
        enp0s8:
            addresses:
            - 192.168.99.100/24
            dhcp4: false
            nameservers:
                addresses: []
                search: []
            routes:
              - to: 0.0.0.0/0
                via: 192.168.99.1
                metric: 101
    version: 2
#sudo netplan apply
```
  
