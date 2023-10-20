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


## Run project secured with [staging certs](https://letsencrypt.org/docs/staging-environment/)

- Create a basic auth loki user entry 

```
sudo htpasswd -c /loki/.htpasswd loki
```

- Edit .env to have the following entries

```
INSECURE=false
RENEWAL_EMAIL=<email>
DOMAIN_NAME=<server-address>
SUBDOMAINS=grafana.<server-address>,loki.<server-address>
```
- init project
  
``` sudo instant project init --env-file .env ```


## Run project secured with real certs

- Create  loki user entry

```
sudo htpasswd -c /loki/.htpasswd loki
```

- Edit .env to have the following entries

```
INSECURE=false
STAGING=false
RENEWAL_EMAIL=<email>
DOMAIN_NAME=<server-address>
SUBDOMAINS=grafana.<server-address>,loki.<server-address>
```
- init project
  
``` sudo instant project init --env-file .env ```


# Connecting Docker logging from remote machines

## [Install Grafana-Loki log driver in Docker]( https://grafana.com/docs/loki/latest/send-data/docker-driver/)

```sudo docker plugin install grafana/loki-docker-driver:2.9.1 --alias loki --grant-all-permissions```

```sudo systemctl restart docker```

## [Specify log driver](https://grafana.com/docs/loki/latest/send-data/docker-driver/configuration/)

### Specifying in command line

```
docker run --log-driver=loki 
    --log-opt loki-url="https://<user_id>:<password>@<central-logging-server-address>/loki/api/v1/push" \
    <image>
```


### Specifying in compose

- in your docker-compose.yml
```
    logging:
      driver: loki
      options:
        loki-url: "https://<user_id>:<password>@<central-logging-server-address>/loki/api/v1/push"
```


### Specifying in docker daemon

- in your daemon.json
  ```
  {
    "debug" : true,
    "log-driver": "loki",
    "log-opts": {
        "loki-url": "https://<user_id>:<password>@<central-logging-server-address>/loki/api/v1/push",
    }
  }
  ```
