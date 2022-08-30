# WSO2 API Manager Docker

## Getting started

### Prerequisites

- [Docker](https://docs.docker.com/engine/install/)
- [Docker Compose](https://docs.docker.com/compose/install/)

### Configuration

Set api manager hostname

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

Start the api manager

```
docker-compose up -d
```

Stop the api manager

```
docker-compose down
```
