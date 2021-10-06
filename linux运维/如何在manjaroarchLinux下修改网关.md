# 如何在manjaro/archLinux下修改网关

我本地的环境用的是旁路路由的形式，默认网关是纯路由器，最近安装的cutefishOS这个系统，用起来挺好看的，但是没有自带修改网关的功能，所以研究了一下如何修改：

首先`ip route show`

```
default via 192.168.1.1 dev ens33
default via 192.168.1.1 dev ens33 proto dhcp metric 100
```

可以看到默认网关是192.168.1.1，这里添加一下自己的旁路网关

```
sudo ip route add default via 192.168.1.248 dev ens33   
```

然后再删除原本的：

```
sudo ip route del default via 192.168.1.1 dev ens33  
```

此时即可访问Google试试吧？