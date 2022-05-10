# Windows10修改Docker镜像目录参考

停止docker
cmd中查看停止情况：
```shell
wsl --list -v
  NAME                   STATE           VERSION
* docker-desktop         Stopped         2
  docker-desktop-data    Stopped         2
```

```shell
wsl --export docker-desktop-data "D:\docker\docker-desktop-data.tar"
wsl --unregister docker-desktop-data
wsl --import docker-desktop-data "D:\docker" "D:\docker\docker-desktop-data.tar" --version 2

```

