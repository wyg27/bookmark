# 我的 Docker 快速参考

### Auto delete a container after running it

```shell
docker run --rm php echo Hello World
```

### 用 container 中的 PHP 执行本地文件

思路：将本地文件所在的目录映射的 container 内。

```shell
docker run -it --rm -v "$PWD":/tmp -w /tmp [image name] php hostfile.php
```

### 将 container 中的某个文件 dump 出来

来自 Apache 官方 https://hub.docker.com/_/httpd

```shell
$ docker run --rm httpd:2.4 cat /usr/local/apache2/conf/httpd.conf > my-httpd.conf
```

### 将 container 中的文件 copy 出来

```shell
$ docker cp [container name]:/etc/apache2/sites-available/000-default.conf .
```

### Container 的共享内存设置

创建 docker 的时候默认 shm 大小为64M，如果应用使用了比较大的shm，则很可能会崩溃。此时解决有2个办法：

1. 挂载宿主机的 shm，完美。

```
services：
  app:
    image: kelvinblood/app
	volumes:
  	- /dev/shm:/dev/shm
```

2. 运行时参数

`docker run -it --shm-size="1g" ubuntu`

## 参考

* How to Run PostgreSQL and pgAdmin Using Docker https://towardsdatascience.com/how-to-run-postgresql-and-pgadmin-using-docker-3a6a8ae918b5
