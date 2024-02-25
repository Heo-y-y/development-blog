# Docker - Redis & Kafka & MongoDB 연결해보기**

## Redis & Kafka & Zookeeper 설치 및 설정

먼저 Redis, Kafka, Zookeeper를 이용해서 이미지를 pull 받고 띄워줬다.

**docker-compose.yml**

```java
version: '3'
services:
  redis:
    image: redis:alpine
    command: redis-server --port 6379
    container_name: redis-instagram
    hostname: redis-instagram
    labels:
      - "name-redis"
      - "mode=standalone"
    ports:
      - "6379:6379"
    restart: always

  zookeeper:
    image: wurstmeister/zookeeper
    container_name: zookeeper
    ports:
      - "2181:2181"

  kafka:
    image: wurstmeister/kafka
    container_name: kafka
    ports:
      - "9092:9092"
    environment:
      KAFKA_ADVERTISED_HOST_NAME: localhost
      KAFKA_CREATE_TOPICS: "Topic:1:1"
      KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181
    volumes:
      - /var/run/docker.sock
    depends_on:
      - zookeeper
```

`docker-compose.yml` 에 설정을 하고, 다음과 같이 명령어를 입력한다.

```bash
docker-compose up - d
```

그리고 Kafka와 Zookeeper 사용을 위해 기본적인 설정을 진행했다.

### Zookeeper 설정

1. **Zookeeper Bash로 이동**
    
    ```bash
    docker exec -i -t zookeeper bash
    ```
    
2. **Zookeeper 서버 실행**
    
    ```bash
    bash bin/zkServer.sh start /opt/zookeeper-3.4.13/conf/zoo.cfg -demon
    ```
    

![스크린샷1 2024-02-07 오후 9 55 58](https://github.com/Heo-y-y/development-blog/assets/112863029/b14006b1-2059-440d-bbbc-4156de43961f)

Zookeeper 서버의 2181 포트가 LISTEN 상태로 변경되는 걸 확인할 수 있다.

### Kafka 설정

1. **Kafka Bash로 이동**
    
    ```bash
    docker exec -i -t kafka bash
    ```
    
2. **Kafka 서버 실행**
    
    ```bash
    kafka-server-start.sh -daemon
    ```
    

![스크린샷2 2024-02-07 오후 9 58 29](https://github.com/Heo-y-y/development-blog/assets/112863029/a8cb8837-5398-40e6-92ef-668d6e5b4fce)

이렇게 Zookeeper와 Kafka의 설정을 완료했다.

Kafka는 Topic을 생성해서 해당 주제(Topic)을 구독해야 pub/sub을 통한 메시지의 생산과 소비가 가능하다. 따라서 기능을 개발할 때 사용할 Topic을 생성해야 했다.

- **Kafka Bash로 이동 후 Topic 생성**
    
    ```bash
    kafka-topics.sh --create --zookeeper zookeeper:2181 --topic { Topic 이름 } --partitions 1 --replication-factor 1
    ```
    
- **Topic 생성 확인**
    
    ```bash
    kafka-topics.sh --list --zookeeper zookeeper
    ```
    

![스크린샷3 2024-02-07 오후 10 04 08](https://github.com/Heo-y-y/development-blog/assets/112863029/06858e3d-39a1-4cab-8864-f9d222a79679)

이렇게 Kafka에서 사용할 Topic을 생성하고, Zookeeper, Kafka, Redis까지 등록했다.

## MongoDB 설치 및 설정

MongoDB도 마찬가지로 docker-compose를 사용했다.

**docker-compose.yml**

```bash
version: '3'
services:
  mongodb:
    image: mongo
    command: --replSet replDb --bind_ip_all --port 27017
    container_name: mongodb-instagram
    ports:
      - "27017:27017"
    restart: always
```

`docker-compose.yml` 에 설정을 하고, 다음과 같이 명령어를 입력한다.

```bash
docker-compose up - d
```

그러면 채팅을 구현하기 위해 필요한 것들은 모두 완료했다.

![스크린샷4 2024-02-07 오후 10 04 08](https://github.com/Heo-y-y/development-blog/assets/112863029/afca4ef9-0913-47b7-a45d-2ba781db52e0)

하지만 위 사진을 보면, zookeeper에 AMD 경고문이 뜨는 것을 볼 수 있다. 필자는 Mac M1을 사용하기 때문에 그런것인데 위 문제는 아래 설정을 통해 해결할 수 있다.

- 기존

```bash
  zookeeper:
    image: wurstmeister/zookeeper
```

- 변경

```bash
  zookeeper:
    image: zookeeper
```
![스크린샷 2024-02-25 오후 4 10 37](https://github.com/Heo-y-y/development-blog/assets/112863029/45664886-915b-4946-ae50-d0d4bdec8822)

