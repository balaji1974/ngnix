# NGNIX

## Configuring URL paths to different URL/ports (app servers)
```xml
Routing needed: 
http://localhost -> will be routed to the default ngnix root folder
http://locahost/test1/ -> will be routed to the http://localhost:8081/ or any other app server in an outside machine
http://locahost/test2/ -> will be routed to the http://localhost:8082/ or any other app server in an outside machine

Open the ngix.conf file and change the following: 

server {
        listen       80;
        server_name  localhost;

        
        location / {
            root   html;
            index  index.html index.htm;
            
        }

        location /test1/ {
            proxy_pass http://127.0.0.1:8081/;
            
        }

        location /test2/ {
            proxy_pass http://127.0.0.1:8082/;
            
        }
}

```


## Configuring subdomains to different URL/ports (app servers)
```xml
Routing needed: 
http://localhost -> will be routed to the default ngnix root folder
http://test1.locahost/ -> will be routed to the http://localhost:8081/ or any other app server in an outside machine
http://test2.locahost/ -> will be routed to the http://localhost:8082/ or any other app server in an outside machine

For this to work we need to add the subdomain in the host machine's hosts file. 
Open the host file and add the following:
127.0.0.1	localhost
127.0.0.1   localhost test1.localhost
127.0.0.1   localhost test2.localhost


Next open the ngix.conf file and add the following: 

server {
    listen       80;
    server_name  localhost;
    location / {
        root   html;
        index  index.html index.htm;
        
    }
}

server {
    listen       80;
    server_name test1.localhost;
    location / {
        proxy_pass       http://localhost:8081/;
    }
}

server {
    listen       80;
    server_name test2.localhost;
    location / {
        proxy_pass       http://localhost:8082/;
    }
}

```


### References:
https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy/
