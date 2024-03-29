# Ubuntu 22.04.2 LTS 下配合 Apache/2.4.52 (Ubuntu) 解析 PHP

## 背景

* 操作系统：Ubuntu 22.04.2 LTS
* Apache：Apache/2.4.52 (Ubuntu)

## 修改 Apache 的配置文件

这里假设使用 vim 来编辑文件。

```shell
$ sudo vim /etc/apache2/sites-available/000-default.conf
```

如果你的 Apache/2.4.52 是全新安装的，会在 `000-default.conf` 中看到以下内容。

```
<VirtualHost *:80>
        # The ServerName directive sets the request scheme, hostname and port that
        # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the request's Host: header to
        # match this virtual host. For the default virtual host (this file) this
        # value is not decisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.
        #ServerName www.example.com

        ServerName openapi-sandbox.foodhwy.com
        ServerAdmin webmaster@localhost
        DocumentRoot /var/html/walmart_openapi/public
        <Directory "/var/html/walmart_openapi/public">
            Require all granted
        </Directory>

        # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
        # error, crit, alert, emerg.
        # It is also possible to configure the loglevel for particular
        # modules, e.g.
        #LogLevel info ssl:warn

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

        # For most configuration files from conf-available/, which are
        # enabled or disabled at a global level, it is possible to
        # include a line for only one particular virtual host. For example the
        # following line enables the CGI configuration for this host only
        # after it has been globally disabled with "a2disconf".
        #Include conf-available/serve-cgi-bin.conf 
</VirtualHost>
```
