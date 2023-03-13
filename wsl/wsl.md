
### WSL开启支持systemd

1. 在 `/etc/wsl.conf`中添加如下配置
``` conf
[boot]
systemd=true
```
2. `wsl --shutdown`