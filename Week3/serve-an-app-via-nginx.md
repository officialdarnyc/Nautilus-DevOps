# Deploy an app via nginx and install SSL

Deployed a node.js application called Rocket.Chat using docker compose. Rocket.Chat is an open-source fully customizable communications platform.
On a Linux VM hosted in Azure, installed docker and docker compose. Created a docker-compose.yaml file and added the below:

```
version: '2'

services:
  rocketchat:
    image: registry.rocket.chat/rocketchat/rocket.chat:latest
    command: >
      bash -c
        "for i in `seq 1 30`; do
          node main.js &&
          s=$$? && break || s=$$?;
          echo \"Tried $$i times. Waiting 5 secs...\";
          sleep 5;
        done; (exit $$s)"
    restart: unless-stopped
    volumes:
      - ./uploads:/app/uploads
    environment:
      - PORT=3000
      - ROOT_URL=http://chat.blutech.tk:3000
      - USE_NATIVE_OPLOG=true
      - MONGO_URL=mongodb://mongo:27017/rocketchat
      - MONGO_OPLOG_URL=mongodb://mongo:27017/local
      - MAIL_URL=smtp://smtp.email
#       - HTTP_PROXY=http://proxy.domain.com
#       - HTTPS_PROXY=http://proxy.domain.com
    depends_on:
      - mongo
    ports:
      - 3000:3000
    labels:
      - "traefik.backend=rocketchat"
      - "traefik.frontend.rule=Host: your.domain.tld"

  mongo:
    image: mongo:4.4
    restart: unless-stopped
    volumes:
     - ./data/db:/data/db
     #- ./data/dump:/dump
    command: mongod --smallfiles --oplogSize 128 --replSet rs0 --storageEngine=wiredTiger
    labels:
      - "traefik.enable=false"

  # this container's job is just run the command to initialize the replica set.
  # it will run the command and remove himself (it will not stay running)
  mongo-init-replica:
    image: mongo:4.4
    command: >
      bash -c
        "for i in `seq 1 30`; do
          mongo mongo/rocketchat --eval \"
            rs.initiate({
              _id: 'rs0',
              members: [ { _id: 0, host: 'localhost:27017' } ]})\" &&
          s=$$? && break || s=$$?;
          echo \"Tried $$i times. Waiting 5 secs...\";
          sleep 5;
        done; (exit $$s)"
    depends_on:
      - mongo

  # hubot, the popular chatbot (add the bot user first and change the password before starting this image)
  hubot:
    image: rocketchat/hubot-rocketchat:latest
    restart: unless-stopped
    environment:
      - ROCKETCHAT_URL=chat.blutech.tk:3000
      - ROCKETCHAT_ROOM=GENERAL
      - ROCKETCHAT_USER=bot
      - ROCKETCHAT_PASSWORD=botpassword
      - BOT_NAME=bot
  # you can add more scripts as you'd like here, they need to be installable by npm
      - EXTERNAL_SCRIPTS=hubot-help,hubot-seen,hubot-links,hubot-diagnostics
    depends_on:
      - rocketchat
    labels:
      - "traefik.enable=false"
    volumes:
      - ./scripts:/home/hubot/scripts
  # this is used to expose the hubot port for notifications on the host on port 3001, e.g. for hubot-jenkins-notifier
    ports:
      - 3001:8080

  #traefik:
  #  image: traefik:latest
  #  restart: unless-stopped
  #  command: >
  #    traefik
  #     --docker
  #     --acme=true
  #     --acme.domains='your.domain.tld'
  #     --acme.email='your@email.tld'
  #     --acme.entrypoint=https
  #     --acme.storagefile=acme.json
  #     --defaultentrypoints=http
  #     --defaultentrypoints=https
  #     --entryPoints='Name:http Address::80 Redirect.EntryPoint:https'
  #     --entryPoints='Name:https Address::443 TLS.Certificates:'
  #  ports:
  #    - 80:80
  #    - 443:443
  #  volumes:
  #    - /var/run/docker.sock:/var/run/docker.sock
```
Started the stack of services by running:

```
docker-compose up -d
```
This started:
- a MongoDB service named `mongo`.
- a service named `mongo-init-replica` that waited for mongo to be ready, connected to it, initialized it and terminated itself (mongo kept running).
- a service `rocketchat`, that also waited for `mongo` to be ready, the `mongo-init-replica` to initialize, and then ran the Rocket.Chat app.

Pointed my subdomain `chat.blutech.tk` to the public IP address of my Linux VM by creating an A record in my DNS records.

Installed nginx web server and confirmed the service was running with the following commands:

```
sudo apt-get install nginx
systemctl status nginx.service
```
To begin setting up nginx as my reverse proxy server in order to serve HTTPS, ran the below commands that installed certbot tool so as to use Let's Encrypt SSL:

```
sudo add-apt-repository ppa:certbot/certbot
sudo apt install python3-certbot-nginx
```
Using the certbot tool, obtained SSL certificate for my subdomain using the command:

```
sudo certbot --nginx -d chat.blutech.tk
```
Copied over my domain's certificate key obtained from let's encrypt to my nginx folder with the command:

```
sudo cp /etc/letsencrypt/live/chat.blutech.tk/privkey.pem /etc/nginx/certificate.key
```

Set the permission lock for the certificate key file:

```
sudo chmod 400 /etc/nginx/certificate.key
```

Copied over my domain's full certificate chain to the same nginx folder:

```
sudo cp /etc/letsencrypt/live/chat.blutech.tk/fullchain.pem /etc/nginx/certificate.crt
```
Edited the default file for the nginx configuration to provide a secure reverse proxy for my Rocket.Chat server app with the following config:

```
# Upstream definition
upstream backend {
    server 127.0.0.1:3000;
}
# http to https redirection
server {
  listen 80;
  server_name chat.blutech.tk;
  rewrite ^ https://$server_name$request_uri? permanent;
}
# HTTPS Server
server {
    listen 443;
    server_name chat.blutech.tk;
    error_log /var/log/nginx/rocketchat.access.log;
    ssl on;
    ssl_certificate /etc/nginx/certificate.crt;
    ssl_certificate_key /etc/nginx/certificate.key;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2; # don’t use SSLv3 ref: POODLE
    location / {
        proxy_pass http://backend/;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $http_host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forward-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forward-Proto http;
        proxy_set_header X-Nginx-Proxy true;
        proxy_redirect off;
    }
}
```
Restarted the nginx service and then, confirmed it was running fine.

```
sudo service nginx restart
sudo service nginx status
```

Confirmed that the server app was reachable on the domain [chat.blutech.tk](https://chat.blutech.tk/), and the SSL was correctly and successfully installed.