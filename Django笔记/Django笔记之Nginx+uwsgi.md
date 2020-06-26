# Django笔记之Nginx对接uwsgi

- 安装 nginx (ubuntu)

```bash
# 安装先决条件：
sudo apt install curl gnupg2 ca-certificates lsb-release
# 要为稳定的nginx软件包设置apt仓库，请运行以下命令：
echo "deb http://nginx.org/packages/ubuntu `lsb_release -cs` nginx" \
    | sudo tee /etc/apt/sources.list.d/nginx.list
# 如果要使用主线nginx软件包，请改为运行以下命令：
echo "deb http://nginx.org/packages/mainline/ubuntu `lsb_release -cs` nginx" \
    | sudo tee /etc/apt/sources.list.d/nginx.list
# 接下来，导入一个官方的nginx签名密钥，以便apt可以验证软件包的真实性：
curl -fsSL https://nginx.org/keys/nginx_signing.key | sudo apt-key add -
# 验证您现在是否具有正确的密钥：
sudo apt-key fingerprint ABF5BD827BD9BF62
# 输出应包含完整的指纹 573B FD6B 3D8F BC64 1079 A6AB ABF5 BD82 7BD9 BF62 ，如下所示：
pub   rsa2048 2011-08-19 [SC] [expires: 2024-06-14]
      573B FD6B 3D8F BC64 1079  A6AB ABF5 BD82 7BD9 BF62
uid   [ unknown] nginx signing key <signing-key@nginx.com>
# 要安装nginx，请运行以下命令：
sudo apt update
sudo apt install nginx
```

- 安装 uwsgi

```bash
pip install uwsgi
```

- 配置项目 nginxconfig.conf 文件

```conf

user  nginx;
worker_processes  1;

error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;

events {
    worker_connections  1024;
}

http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout  65;

    #gzip  on;

    server {
        listen       80;
        server_name  localhost;

        root   /home/lala/PycharmProjects/Django-Demo;

        # 上传文件的目录
        location /media/ {
            alias /home/lala/PycharmProjects/Django-Demo/media/uploads/;
        }

        # 静态资源的目录
        location /static/ {
            alias /home/lala/PycharmProjects/Django-Demo/static;
        }

        location / {
            include /etc/nginx/uwsgi_params;
            uwsgi_pass 127.0.0.1:8080;
        }

        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   /usr/share/nginx/html;
        }
    }
}

```

- 配置 uwsgi.ini 文件

```ini
[uwsgi]
# 使用 nginx 连接时使用
socket = 127.0.0.1:8080
# 直接作为 web 服务器使用
# http = 127.0.0.1:8080
# 配置工程目录
chdir = /home/lala/PycharmProjects/Django-Mall
# 配置项目的 wsgi 目录 相对于工程目录
wsgi-file = Mall/wsgi.py
# 配置进程·线程信息
processes = 4
threads = 2
enable-threads = True
master = True
# 进程id
pidfile = uwsgi.pid
# 日志文件
daemonize = uwsgi.log
```

- 启动 nginx

```bash
sudo nginx -c 配置文件路径.conf
```

- 启动 uwsgi

```bash
uwsgi --ini 配置文件路径.ini
```

- 检查 nginx 和 uwsgi 进程

```bash
ps -ef | grep nginx
# 输出为
root       73387     963  0 17:53 ?        00:00:00 nginx: master process nginx -c /home/lala/PycharmProjects/Django-Mall/config.conf
nginx      73388   73387  0 17:53 ?        00:00:00 nginx: worker process
user       73423   72521  0 17:54 pts/0    00:00:00 grep --color=auto nginx
# ------
ps -ef | grep uwsgi
# 输出为
user       73853     963  2 18:07 ?        00:00:00 uwsgi --ini uwsgi.ini
user       73856   73853  0 18:07 ?        00:00:00 uwsgi --ini uwsgi.ini
user       73857   73853  0 18:07 ?        00:00:00 uwsgi --ini uwsgi.ini
user       73859   73853  0 18:07 ?        00:00:00 uwsgi --ini uwsgi.ini
user       73861   73853  0 18:07 ?        00:00:00 uwsgi --ini uwsgi.ini
user       73867   72521  0 18:07 pts/0    00:00:00 grep --color=auto uwsgi
```

- 关闭 nginx

```bash
sudo nginx -s stop
```

- 关闭 uwsgi

```bash
uwsgi --stop uwsgi.pid
```

> 出错多看看日志文件

