# access.log
Go to `nginx.conf`  in `/etc/nginx/nginx.conf`.
```cmd
log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';
```
There is a log format in `nginx.conf`. main is the name of the format.
Then, there are variables.
`remote_addr` : IP address of client trying to connect.

`remote_user`: http basic authentication's field. If it doesn't prevent, then only `-` is present.

`time_local` : time at which request has come.

`request`: means request method+URI+HTTP protocol version

`status`: means the response code.

`body_bytes_sent` size of response payload.

`http_referer`:  empty if user directly opened the link from browser.

`http_user_agent` browser and OS sometimes that client is accessing the page from.

`http_x_forwarded_for` is something which is useful when there is a proxy in between.
You can either remove or add new variables and it'll reflect in your `access.log`.

# Configuring Custom Access Logs
```cmd
log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

access_log  /var/log/nginx/access.log  main;
```
`access_log` will use "main" log format and store logs in access.log.
So, main means the format of the log.
If we've multiple logs format section, we can actually mention the sections over here.
Create a new log format just below there
```cmd
log_format  master  '$remote_addr - $remote_user ;
access_log  /var/log/nginx/access.log  master;
```
One of the downsides of mentioning the format over here is that it is a global directive. So, if you've multiple websites, all the websites will start to store the logs according to the master log format.

## So, what if we want the website1 should store the logs in the main format and website2 should actually store the logs based on master format?
nginx is flexible and allows us to do this.
Go to 
`/etc/nginx/conf.d/web.conf`
```cmd
server{
    server_name mywebsitename;
    access_log /var/log/nginx/example.log main;
    listen 80;
    location /{
        root /usr/share/nginx/html;
    }
}
```
As seen above, we can specify custom location for the `access.log` as well as a format that we want.
# error.log
The various levels of `error.log`. They are
- emerg(emergency)
- alert
- crit
- error
- warn
- notice
- info
- debug
nginx doesn't allow us to have the same flexibility in error logs that we had in access logs.
The format of error log is predefined.
The only thing you can change is the level of logging and the file in which the error log will be stored.
```cmd
2023/09/07 13:29:47 [notice] 1996#1996: signal process started
2023/09/07 13:30:17 [error] 1997#1997: *1 open() "/usr/share/nginx/html/favicon.ico" failed (2: No such file or directory), client: 192.168.1.68, server: mywebsitename, request: "GET /favicon.ico HTTP/1.1", host: "192.168.1.75", referrer: "http://192.168.1.75/"
```
emergency logs come when nginx configuration is wrong.
debug logs aren't enabled by default on nginx, you've to compile nginx manually to enable it. debug  logs are very useful and helpful.
