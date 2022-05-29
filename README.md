# laravel-sse-example
## Prerequisites
- Docker
- docker-compose

## How to use
Run this program as follows:
```
$ git clone -b http2_server_side_events https://github.com/lechatthecat/laravel-soketi-broadcast-example-by-websocket
```
Please note that the server is using http2, so you need "server.crt" and "server.key" in "{this project's root path}/docker/nginx/cert" for SSL connection.

Also, you need a DB. Please access "mydb" container. The password of root is "root". Then create a database named as "laravel".

After creating the certificate, please run these commands:
```
$ cd laravel-soketi-broadcast-example-by-websocket
$ docker-compose up -d --build
$ docker-compose exec laravel ash
# In the laravel container:
$ sh -x /laravel_build.sh
$ php artisan migrate
$ npm run watch
```
Then see https://localhost:8081/  
You can see laravel is working on docker there.  
   
Now you can broadcast a message by accessing this url from your browser:  
https://localhost:8081/api/store_message  
It is using server side events to broadcast the event.  
But please note that number of concurrent connections toward the "stream" API (that is a SSE endpoint) is limited to only 10 because of how php-fpm works.  
  
You can see your browser hangs if you open 11th tab of https://localhost:8081/  
The number of concurrent persistent connections can be increased by changing number of "pm.max_children = 10", but this number cannot be increased to a big number, or your server will crash. php-fpm conf file is here: {this project's root path}/docker/laravel/php-fpm.conf
  
