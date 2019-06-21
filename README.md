# Makerobos landing page

If you want to run the Makerobos conversational landing page in you your server, just pull this docker image and pass the bot-id as environment variable with key BOT

## example for docker:
```
docker run -e BOT=<bot-id> -p 127.0.0.1:80:4800/tcp makerobos/landing_page:latest node dist/server
```

## example of docker-compose:
```yml
version: '3'
services:
    landing_page:
        image: makerobos/landing_page:latest
        command: "node dist/server"
        ports:
             - "8000":"4800"
        environment:
             - BOT=<bot-id>
```


then you can proxy pass to you web server
here is how you can do it using nginx:

## nginx server conf
```
server {
    server_name your.server.com;
    location / {
        proxy_pass http://127.0.0.1:8000$request_uri;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        add_header locationischat ank;
        proxy_redirect     off;
        proxy_set_header   Host $host;
        proxy_set_header   X-Real-IP $remote_addr;
        proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header   X-Forwarded-Host $server_name;
    }
}
```
