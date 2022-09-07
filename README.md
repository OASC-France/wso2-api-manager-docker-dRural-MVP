# WSO2 API Manager Docker for dRural

## Getting started

### Prerequisites

- [Docker](https://docs.docker.com/engine/install/)
- [Docker Compose](https://docs.docker.com/compose/install/)

### Configuration

> Make sure you are in the same directory as the docker-compose.yml file before running these commands.

Pull the api manager image

```
sudo docker pull wso2/wso2am:4.1.0
```

Run a container of the api manager

```
sudo docker run -d wso2/wso2am:4.1.0
```

> Take note of the container identifier.

> Wait 5 minutes for the api manager to be fully started.

Copy the api manager directory

```
sudo docker cp <container-id>:/home/wso2carbon/wso2am-4.1.0 ./
```

Stop the container

```
sudo docker stop <container-id>
```

Define the api manager user as owner of the entire wso2am-4.1.0 directory

```
sudo chown 802:802 -R ./wso2am-4.1.0
```

#### Set the hostname and configure HTTPS (using Apache as reverse proxy)

##### Set up Apache as reverse proxy and configure HTTPS

> WSO2 API Manager does not seem to be designed to easily choose which ports to use, and it is not clear how to import your own HTTPS certificates. That's why in this example we will use a reverse proxy and the default ports. In order not to create a conflict with Docker ports, we increment each port transferred from the Docker container by 10. Example: You call https://example.com:9443/publisher, the reverse proxy redirects you to https://localhost:9453/publisher.

Install Apache

```
sudo apt install apache2
```

Disable default virtual hosts

```
sudo a2dissite 000-default
sudo a2dissite default-ssl
```

Create virtual host

/etc/apache2/sites-available/example.com.conf

```
<VirtualHost *:80>
    ServerName example.com

    ProxyPreserveHost On

    SSLProxyEngine on
    SSLProxyVerify none
    SSLProxyCheckPeerCN off
    SSLProxyCheckPeerName off
    SSLProxyCheckPeerExpire off

    ProxyPass / https://localhost:9453/
    ProxyPassReverse / https://localhost:9453/
</VirtualHost>
```

Enable virtual host

```
sudo a2ensite example.com
```

Listen for port 9443

/etc/apache2/ports.conf

```
Listen 9443
```

Enable required modules

```
sudo a2enmod ssl
sudo a2enmod proxy
sudo a2enmod proxy_http
sudo a2enmod proxy_ajp
sudo a2enmod rewrite
sudo a2enmod deflate
sudo a2enmod headers
sudo a2enmod proxy_balancer
sudo a2enmod proxy_connect
sudo a2enmod proxy_html
```

Restart Apache

```
sudo service apache2 restart
```

Install [Certbot](https://certbot.eff.org/instructions?ws=apache&os=debianbuster)

Run Certbot

```
sudo certbot --apache
```

Disable HTTP virtual host

```
sudo a2dissite example.com
```

Change HTTPS port from 443 to 9443

> Please note the "-le-ssl" file name suffix

/etc/apache2/sites-available/example.com-le-ssl.conf

```
…
<VirtualHost *:9443>
…
```

Restart Apache

```
sudo service apache2 restart
```

> This operation can be repeated for the gateway.

##### Set the hostname

wso2am-4.1.0/repository/conf/deployment.toml

```
[server]
hostname = "example.com"
…
[[apim.gateway.environment]]
…
service_url = "https://example.com:${mgt.transport.https.port}/services/"
…
http_endpoint = "http://example.com:${http.nio.port}"
https_endpoint = "https://example.com:${https.nio.port}"
…
[apim.devportal]
url = "https://example.com:${mgt.transport.https.port}/devportal"
```

#### Customization of the developer portal logo

wso2am-4.1.0/repository/deployment/server/jaggeryapps/devportal/site/public/theme/userTheme.js

```js
const Configurations = {
    custom: {
        appBar: {
            logo: 'https://example.com/logo.png',
        },
    },
};
```

### Usage

#### Start the api manager

```
sudo docker compose up -d
```

#### Stop the api manager

```
sudo docker compose down
```

### Endpoints

> The ports must be open on the machine to be able to access it from outside.

#### Gateway

https://example.com:8243

#### Publisher

https://example.com:9443/publisher

#### Developer Portal

https://example.com:9443/devportal

> If you have not defined the certificates for the HTTPS protocol and you want to try an api via the developer portal, you will first have to manually access the gateway endpoint via your web browser and acknowledge the warning message.

#### Management Console

https://example.com:9443/carbon
