### installation ###
```
apt-get install nginx
pip install supervisor
```

### supervisor configuration ###
```
echo_supervisord_conf > /etc/supervisord.conf
mkdir /etc/supervisord.d/

# add below into /etc/supervisord.conf
[include]
files = /etc/supervisord.d/*.conf

# copy below file to /etc/supervisord.d
```

### test.conf ###
```
[program:test]
command=/path/to/python /path/to/main.py 889%(process_num)01d
process_name=%(program_name)s_%(process_num)01d
redirect_stderr=true
stdout_logfile=/var/log/test.log
numprocs=4
```

### nginx configuration ###
copy below file to /etc/nginx/sites-enabled/

### nginx_test ###
```
upstream test{
 server 127.0.0.1:8890;
 server 127.0.0.1:8891;
 server 127.0.0.1:8892;
 server 127.0.0.1:8893;
 keepalive 1024;
}
server {
 listen 8888;
 server_name localhost;
 location /favicon.ico {
  alias /var/www/favicon.ico;
 }
 location / {
  proxy_pass_header server;
  proxy_set_header host $http_host;
  proxy_set_header x-real-ip $remote_addr;
  proxy_set_header x-scheme $scheme;
  proxy_pass http://test;
  proxy_next_upstream error;
  proxy_read_timeout 7200;
 }
 access_log /var/log/nginx/test.access_log;
 error_log /var/log/nginx/test.error_log;
}
```

### start service ###
```
service nginx start
supervisord -c /etc/supervisord.conf
supervisorctl -c /etc/supervisord.conf
# notice the permissions
```
