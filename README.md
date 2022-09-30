
# Logs Aggregator Service

A self hosted aggregator service using Grafana as the visualization tool and Grafana Loki as the logs collector. 

## Installation

Install using Docker

```bash
  sh start.sh
```
    
## Deployment

To publicly deploy the docker containers, I configured a reverse-proxy (nginx preferrable)
 which mapped a public domain to the docker containers address and port.



## Usage/Examples

To capture logs from my docker containers using loki and stashed for visualization on grafana. 
My loki service was deployed and mapped using nginx to 'https://lola-loki.baseafriquedev.com/'. 

Reference: https://docs.docker.com/compose/compose-file/compose-file-v3/#logging

```docker
version: '3.8'

services:
  api:
    build: .
    image: &lolasqr lolasqr-api
    container_name: lolasqr-api
    command: yarn start
    restart: unless-stopped
    ports:
      - "127.0.0.1:5000:5000"
    env_file:
      - ./.env
    depends_on:
      - cache
    links:
      - cache
    networks:
      - lolasqr-net
    logging:
      driver: loki
      options:
        loki-url: "https://lola-loki.baseafriquedev.com/loki/api/v1/push"
        max-size: "200m"

```


## Authors

- [@codemask](https://www.github.com/DeeMATT)

