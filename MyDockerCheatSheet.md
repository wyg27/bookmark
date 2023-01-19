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
