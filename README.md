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

> Wait 5 minutes for the container to be fully started.

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

#### Set api manager hostname

wso2am-4.1.0/repository/conf/deployment.toml

```
[server]
hostname = "example.com"
…
[[apim.gateway.environment]]
…
service_url = "https://example.com:${mgt.transport.https.port}/services/"
…
ws_endpoint = "ws://example.com:9099"
wss_endpoint = "wss://example.com:8099"
http_endpoint = "http://example.com:${http.nio.port}"
https_endpoint = "https://example.com:${https.nio.port}"
websub_event_receiver_http_endpoint = "http://example.com:9021"
websub_event_receiver_https_endpoint = "https://example.com:8021"
…
[apim.devportal]
url = "https://example.com:${mgt.transport.https.port}/devportal"
```

### Usage

#### Start the api manager

```
docker-compose up -d
```

#### Stop the api manager

```
docker-compose down
```

### Endpoints

#### Gateway

https://example.com:8243

#### Publisher

https://example.com:9443/publisher

#### Developer Portal

https://example.com:9443/devportal

> If you have not defined the certificates for the HTTPS protocol and you want to try an api via the developer portal, you will first have to manually access the gateway endpoint via your web browser and acknowledge the warning message.

#### Management Console

https://example.com:9443/carbon
