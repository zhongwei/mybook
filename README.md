# This is my handbook

## Linux book

### Linux Admin

#### Network Admin

- 两块网卡时，修改路由表优先级

```shell
    sudo ip route add default gw 192.168.99.1 dev enp08s proto static metric 101
```

  