# Cheat Sheet

## httpd 相关

### 添加登录认证

- 参考链接： https://blog.51cto.com/llk726/121365

1. 第一步：修改httpd配置

    修改Apache的配置文件/etc/httpd/conf/httpd.conf，对需要认证的资源所在的目录进行配置。具体配置如下：

    ```conf
    vim /etc/httpd/conf/httpd.conf

    ＜Directory "/var/www/html"＞
        Options Indexes FollowSymLinks
        Allowoverride AuthConfig
        Order allow,deny
        Allow from all
    ＜/Directory＞
    ```

    其中，Allowoverride AuthConfig /var/www/html目录下的内容进行用户认证

2. 第二步: 在限制访问目录/var/www/html下创建文件.htaccess，其内容如下：

    ```conf
    vim /var/www/html/.htaccess
    AuthName " my share web"
    AuthType Basic
    AuthUserFile /var/www/html/.htpasswd
    require valid-user
    #AuthName 描述，随便写
    #AuthUserFile /var/www/html/.htpasswd
    ```
    密码文件推荐使用.htpasswd,因为apache默认系统对“.ht”开头的文件默认不允许外部读取，安全系数会高一点哦。
    说明：文件.htaccess中常用的配置命令有以下几个：

      - AuthName命令：指定认证区域名称。区域名称是在提示要求认证的对话框中显示给用户的
      - AuthType命令：指定认证类型。在HTTP1.0中，只有一种认证类型：basic。在HTTP1.1中有几种认证类型，如：MD5。
      - AuthUserFile命令：指定一个包含用户名和密码的文本文件，每行一对。
      - AuthGroupFile命令：指定包含用户组清单和这些组的成员清单的文本文件。组的成员之间用空格分开，如：
      managers:user1 user2
      - require命令：指定哪些用户或组才能被授权访问。如：
        1. require user user1 user2(只有用户user1和user2可以访问)
        2. requiresgroupsmanagers (只有组managers中成员可以访问)
        3. require valid-user (在AuthUserFile指定的文件中任何用户都可以访问)


3. 第三步： 创建httpd的验证用户

    第一次添加用户时.htpasswdt文件不存在，需要用-c选项创建文件

    ```bash
    htpasswd -bc /var/www/html/.htpasswd dev dev
    htpasswd -b  /var/www/html/.htpasswd test test
    ```

4. 第四步：重启httpd

    ```bash
    systemctl restart httpd
    ```


    ```bash
    curl -u username:password [url]
    wget --http-user=username --http-passwd=password  [url]
    ```
