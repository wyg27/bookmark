# 我的 Docker 快速参考

### Auto delete a container after running it

```shell
docker run --rm php echo Hello World
```

### 将 container 中的某个文件 dump 出来

来自 Apache 官方 https://hub.docker.com/_/httpd

```shell
$ docker run --rm httpd:2.4 cat /usr/local/apache2/conf/httpd.conf > my-httpd.conf
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
