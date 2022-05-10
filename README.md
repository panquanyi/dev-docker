# dev-docker

在一台或多台主机启动多个容器模拟清洁的云主机环境。
不同的系统环境：如centos,debian,ubuntu。
不同的软件环境：如openjdk,oraclejdk等。
仅含CentOS7+OpenJDK8示例。
用于验证应用的分布式部署，不影响旧版应用的同时体验新版。
在多台主机启动多个容器需处理网络问题，可增加端口映射实现。

## 编译
```bash
docker build -f example/Dockerfile -t dev:centos7-openjdk8 --no-cache .
```

## 运行容器
### 运行单个容器
例：
```bash
docker run -d -h host000 --name dev000 --privileged=true -p 2200:22 -v /sys/fs/cgroup:/sys/fs/cgroup dev:centos7-openjdk8
```

### 单台机运行多个容器
例：
```bash
docker run -d -h host001 --name dev001 --privileged=true -p 2201:22 -v /sys/fs/cgroup:/sys/fs/cgroup dev:centos7-openjdk8
docker run -d -h host002 --name dev002 --privileged=true -p 2202:22 -v /sys/fs/cgroup:/sys/fs/cgroup dev:centos7-openjdk8
docker run -d -h host003 --name dev003 --privileged=true -p 2203:22 -v /sys/fs/cgroup:/sys/fs/cgroup dev:centos7-openjdk8
docker run -d -h host004 --name dev004 --privileged=true -p 2204:22 -v /sys/fs/cgroup:/sys/fs/cgroup dev:centos7-openjdk8
docker run -d -h host005 --name dev005 --privileged=true -p 2205:22 -v /sys/fs/cgroup:/sys/fs/cgroup dev:centos7-openjdk8
```

### 多台机运行多个容器
例：
```bash
docker run -d -h ${HOSTNAME} --name ${DOCKER_NAME} --privileged=true -p {SSH_PORT}:22 -p {OTHER_PORT_BINDING} \
-v /sys/fs/cgroup:/sys/fs/cgroup -v {OTHER_VOLUME_BINDING} dev:centos7-openjdk8
```

### 进入容器修改密码
```bash
docker exec -it dev000 /bin/bash
passwd
# 输入密码
```

## 使用
ssh -p ${HOST_PORT} root@${HOST_IP}

## MySQL example

```bash
wget https://repo.mysql.com/mysql80-community-release-el7-5.noarch.rpm -O mysql80-community-release-el7-5.noarch.rpm
yum localinstall -y mysql80-community-release-el7-5.noarch.rpm
yum-config-manager --disable mysql80-community
yum-config-manager --enable mysql57-community
yum install -y mysql-community-server
systemctl start mysqld
PASSWORD=`cat /var/log/mysqld.log | grep 'temporary password' | grep -o -P '(?<=: ).*$'`
echo $PASSWORD
mysql -uroot -p
```

```SQL
ALTER USER 'root'@'localhost' IDENTIFIED BY 'Admin@123';
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'Admin@123' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```