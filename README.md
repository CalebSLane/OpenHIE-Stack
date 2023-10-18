# OpenHIE-Stack

## Setup Docker (Ubuntu)

### Install Docker

```sudo apt update```

```sudo apt install apt-transport-https ca-certificates curl software-properties-common```

```curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -```

```sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"```

```sudo apt install docker-ce```

```sudo systemctl status docker```


### Activate Docker Swarm

```sudo docker swarm init``` or ```sudo docker swarm init --advertise-addr <<fill-in-address-here>>```


## Install Instant v2

```curl -L https://github.com/openhie/instant-v2/releases/latest/download/instant-linux -o instant```

```chmod +x ./instant```

```sudo mv instant /usr/bin/instant```

```sudo chown root:root /usr/bin/instant```


## Download project

```git clone https://github.com/CalebSLane/OpenHIE-Stack.git```

```cd OpenHIE-Stack```


## Run project unsecured

``` sudo instant project init && sudo instant project up --env-var INSECURE_PORTS=3100:3100```


# Connecting Docker logging from remote machines

## [Install Grafana-Loki log driver in Docker]( https://grafana.com/docs/loki/latest/send-data/docker-driver/)

```docker plugin install grafana/loki-docker-driver:2.9.1 --alias loki --grant-all-permissions```

```systemctl restart docker```

## [Specify log driver](https://grafana.com/docs/loki/latest/send-data/docker-driver/configuration/)

### Specifying in command line

```
docker run --log-driver=loki 
    --log-opt loki-url="http://<user_id>:<password>@<central-logging-server-address>:3100/loki/api/v1/push" \
    <image>
```


### Specifying in compose

- in your docker-compose.yml
```
    logging:
      driver: loki
      options:
        loki-url: "http://<user_id>:<password>@<central-logging-server-address>:3100/loki/api/v1/push"
```


### Specifying in docker daemon

- in your daemon.json
  ```
  {
    "debug" : true,
    "log-driver": "loki",
    "log-opts": {
        "loki-url": "http://<user_id>:<password>@<central-logging-server-address>:3100/loki/api/v1/push",
    }
  }
  ```
