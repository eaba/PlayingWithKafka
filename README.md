﻿# Playing with Kafka

## Setting up Kafka with Docker

### Prerequisites

* Docker for Windows - https://store.docker.com/editions/community/docker-ce-desktop-windows
* Windows Subsystem for Linux - https://docs.microsoft.com/en-us/windows/wsl/install-win10
* Docker on Windows Subsystem for Linux (Ubuntu) - https://medium.com/@sebagomez/installing-the-docker-client-on-ubuntus-windows-subsystem-for-linux-612b392a44c4

### Setting up Kafka

* https://hub.docker.com/r/confluent/kafka/

#### Stopping and removing containers

```bash
docker stop zookeeper | xargs docker rm
docker stop kafka | xargs docker rm
docker stop schema-registry | xargs docker rm
docker stop rest-proxy | xargs docker rm
```

#### Start Zookeeper and expose port 2181 for use by the host machine

```bash
docker run -d --name zookeeper -p 2181:2181 confluent/zookeeper
```

#### Start Kafka and expose port 9092 for use by the host machine

*Also configure the broker to use the docker machine's IP address*

```bash
docker run -d --name kafka -p 9092:9092 --link zookeeper:zookeeper --env KAFKA_ADVERTISED_HOST_NAME=[DOCKER_HOST_IP] confluent/kafka
```

#### Start Schema Registry and expose port 8081 for use by the host machine

```bash
docker run -d --name schema-registry -p 8081:8081 --link zookeeper:zookeeper --link kafka:kafka confluent/schema-registry
```

#### Start REST Proxy and expose port 8082 for use by the host machine

```bash
docker run -d --name rest-proxy -p 8082:8082 --link zookeeper:zookeeper --link kafka:kafka --link schema-registry:schema-registry confluent/rest-proxy
```

### Generate C# classes from AVRO schemas

#### Avrogen

https://github.com/confluentinc/avro/releases/download/v1.7.7.4/avrogen.zip

```bash
C:\PROJECTDATA\PLAYGROUND\PlayingWithKafka\Kafka (master)
λ dotnet ..\Avrogen\avrogen.dll -s Organisation.asvc .
```