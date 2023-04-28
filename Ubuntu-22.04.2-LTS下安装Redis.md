## 背景

* 操作系统：Ubuntu 22.04.2 LTS
* PHP 版本：v8.1.2
* Apache 版本：Apache/2.4.52 (Ubuntu)

## 步骤

首先，下载最新或特定版本的 Redis 源码到磁盘上的任意目录下。

```shell
$ wget https://github.com/phpredis/phpredis/archive/3.1.4.tar.gz
```

然后解压。

```shell
$ tar zxvf 3.1.4.tar.gz
```

进入目录。

```shell
$ cd phpredis-5.3.7
```

依次执行下列命令。

```shell
$ ./configure
$ sudo make
$ sudo make install
```

如果一切顺利，会看到类似下面的提示（节选）。

```text
----------------------------------------------------------------------
Libraries have been installed in:
   /home/ubuntu/phpredis-5.3.7/modules

If you ever happen to want to link against installed libraries
in a given directory, LIBDIR, you must either use libtool, and
specify the full pathname of the library, or use the '-LLIBDIR'
flag during linking and do at least one of the following:
   - add LIBDIR to the 'LD_LIBRARY_PATH' environment variable
     during execution
   - add LIBDIR to the 'LD_RUN_PATH' environment variable
     during linking
   - use the '-Wl,-rpath -Wl,LIBDIR' linker flag
   - have your system administrator add LIBDIR to '/etc/ld.so.conf'

See any operating system documentation about shared libraries for
more information, such as the ld(1) and ld.so(8) manual pages.
----------------------------------------------------------------------

Build complete.
Don't forget to run 'make test'.

Installing shared extensions:     /usr/lib/php/20210902/
```

注意提示信息末尾的 `Installing shared extensions:     /usr/lib/php/20210902/`，在这个路径中就能找到已经编译成功的 `redis.so` 文件。

然后编辑 Apache 所加载的 php.ini 文件（比如 `/etc/php/8.1/apache2/php.ini`）在里面增加一行配置来指明 `redis.so` 所在的绝对路径：

```
; - Many DLL files are located in the extensions/ (PHP 4) or ext/ (PHP 5+)
;   extension folders as well as the separate PECL DLL download (PHP 5+).
;   Be sure to appropriately set the extension_dir directive.
;
extension=/usr/lib/php/20210902/redis.so
;extension=bz2
```

然后重启 apache 服务：

```shell
$ sudo systemctl restart apache2
```

之后，可以在一个 PHP 页面中调用 `phpinfo()`，然后看看在输出的结果中能否看见“redis”这个区块。如果能看见就表示安装成功了。
