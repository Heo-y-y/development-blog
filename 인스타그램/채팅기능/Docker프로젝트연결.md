# Docker - Spring Boot 프로젝트 & MySQL 연결

## Docker 연결

우선 Kafka와 MongoDB, Stomp를 진행하기에 앞서서 도커를 먼저 프로젝트에 연결시키는 작업을 진행하려고 한다.

큰 과정은 아래와 같다.

```bash
DockerFile ->
Docker-compose.yml ->
Docker이미지 ->
Docker Container(compose로 Spring Boot 프로젝트와 Mysql 구성하여 올리기)
```

## Dockerfile

Dockerfile의 위치는 다음과 같다.

![스크린샷1 2024-01-31 오후 5 04 52](https://github.com/Heo-y-y/development-blog/assets/112863029/22d8786a-5adc-4d8b-8807-66b27ed682ac)

### Dockerfile 작성

```bash
FROM openjdk:11-jdk AS builder
COPY . .

FROM openjdk:11-jdk
COPY --from=builder build/libs/*.jar app.jar
EXPOSE 8081
ENTRYPOINT ["java","-jar","/app.jar"]
```

해당 Dockerfile은 두 단계로 구성했다.

Muti-stage 빌드를 활용하여 Spring Boot 애플리케이션을 빌드하고 실행한다.

각 단계의 주요 역할은 다음과 같다.

1. **빌드 단계**
    - `FROM openjdk:11-jdk AS builder`
        - OpenJDK 11 기반을 기반으로 하는 이미지를 builder라는 이름으로 정의한다.
    - `COPY . .`
        - 현재 디렉토리의 모든 파일을 도커 이미지 내부의 작업 디렉토리로 복사한다. 이는 소스 코드 및 빌드 스크립트 등을 이미지로 가져오는 역할을 한다.
2. 실행 단계
    - `FROM openjdk:11-jdk`
        - OpenJDK 11 기반을 기반의 이미지를 사용한다. 이번에는 빌드 단계에서 생성된 builder 이미지를 사용하지 않는다.
    - `COPY --from=builder build/libs/*.jar app.jar`
        - 앞에서 정의한 builder 이미지에서 생성된 JAR 파일들을 현재 이미지의 작업 디렉토리에 복사한다. build/libs/*.jar app.jar 는 빌드 단계에서 생성된 JAR 파일들을 가리킨다.
    - `EXPOSE 8081`
        - 컨테이너가 사용할 포트를 노출한다. 여기서는 8081로 설정했다.
    - `ENTRYPOINT ["java","-jar","/app.jar"]`
        - 컨테이너가 시작될 때 실행될 명령을 설정한다. 여기서는 "java","-jar","/app.jar" 명령어를 실행하여 Spring Boot 애플리케이션을 실행한다.

이렇게 Muti-stage 빌드를 통해 사용하면 애플리케이션을 빌드하는 동안 불필요한 의존성이나 파일을 최종 이미지에 포함시키지 않고, 실행에 필요한 최소한의 구성만을 가진 이미지를 만든다.

## Docker-compose.yml

그러면 이제 여러 도커 서비스 및 설정을 정의하기 위해 `docker-compose.yml`을 만들자.

`docker-compose.yml` 의 위치는 다음과 같다.

![스크린샷2 2024-02-01 오후 9 27 03](https://github.com/Heo-y-y/development-blog/assets/112863029/5db34c29-d89f-4b7e-b9b8-126f334af5e5)

### Docker-compose.yml 작성

```yaml
version: '3'

services:
  database-instagram:
    container_name: database-mysql-instagram
    image: mysql/mysql-server:8.0

    environment:
      MYSQL_ROOT_PASSWORD: '1234'
      MYSQL_ROOT_HOST: '%'
      TZ: Asia/Seoul

    networks:
      - spring-network
    volumes:
      - ./mysql-init.d:/docker-entrypoint-initdb.d

    ports:
      - '13306:3306'

    command:
      - '--character-set-server=utf8mb4'
      - '--collation-server=utf8mb4_unicode_ci'

    healthcheck:
      test: ['CMD-SHELL', 'mysqladmin ping -h 127.0.0.1 -u root --password=$$MYSQL_ROOT_PASSWORD']
      interval: 10s
      timeout: 2s
      retries: 100

  application-instagram:
    container_name: server-instagram
    build:
      context: .
      dockerfile: ./Dockerfile
    restart: unless-stopped
    ports:
      - "8081:8080"
    depends_on:
      database-instagram:
        condition: service_healthy
    networks:
      - spring-network

networks:
  spring-network:
```

1. **서비스 정의**
    - `services` 섹션은 애플리케이션을 구성하는 각 서비스를 정의한다. 여기서는 `database-instagram`과 `application-instagram` 두 개의 서비스를 정의한다.
2. **컨테이너 이름 및 이미지 지정**
    - `container_name` 속성은 각 서비스의 컨테이너 이름을 설정한다. 이를 통해 컨테이너를 쉽게 식별할 수 있다.
    - `imge` 속성은 각 서비스의 사용할 도커 이미지를 지정한다. 즉, `database-instagram` 서비스는 MySQL 8.0 이미지를 사용하고, `application-instagram` 서비스는 빌드한 도커 이미지를 사용한다.
3. **환경 변수 및 볼륨 설정**
    - `environment` 속성은 컨테이너 내부의 환경 변수를 설정한다. 여기서는 MySQL 서비스의 루트 비밀번호와 타임존을 설정하고 있다.
    - `volumes` 속성은 호스트 시스템과 컨테이너 간에 데이터를 공유하기 위한 볼륨을 설정한다. `./mysql-init.d` 디렉토리를 컨테이너의 `/docker-entrypoint-initdb.d` 디렉토리에 마운트한다.
4. **포트 포워딩**
    - `ports` 속성은 호스트와 컨테이너 간의 포트를 매핑한다. `database-instagram` 서비스는 호스트의 13306 포트를 컨테이너의 3306 포트로 매핑한다.
    - `application-instagram` 서비스는 호스트의 8081 포트를 컨테이너의 8080 포트로 매핑한다.
5. **컨테이너 의존성과 네트워크**
    - `depends_on` 속성은 `application-instagram` 서비스가 시작되기 전에 `database-instagram` 서비스가 먼저 시작되도록 하는 의존성을 설정한 것이다.
    - `networks` 속성은 각 서비스가 사용할 네트워크를 정의한다. 여기서는 `spring-network`를 사용한다.
6. **Health Check 설정**
    - `healthcheck` 속성은 해당 서비스의 상태를 체크하는 방법을 정의하는데, MySQL 서비스의 경우 `mysqladmin ping` 명령어를 사용하여 정기적으로 상태를 체크한다.

### 정리

**database-instagram 서비스**

- `mysql/mysql-server:8.0` 이미지를 사용하여 MySQL 서비스를 실행한다.
- `MYSQL_ROOT_PASSWORD`와 `MYSQL_ROOT_HOST` 등의 환경 변수를 설정하여 MySQL 루트 사용자의 비밀번호와 호스트를 지정한다.
- `./mysql-init.d` 디렉토리를 MySQL 컨테이너의 `/docker-entrypoint-initdb.d` 디렉토리에 마운트하여 초기화 스크립트를 실행한다.
- 컨테이너의 3306 포트를 호스트의 13306 포트에 매핑한다.

**application-instagram 서비스**

- 현재 디렉토리에서 `Dockerfile`을 사용하여 애플리케이션 이미지를 빌드한다.
- 컨테이너의 8080 포트를 호스트의 8081 포트에 매핑한다.
- `depends_on` 속성을 사용하여 `database-instagram` 서비스의 준비 상태를 확인하고, `networks` 속성을 통해 `spring-network` 네트워크를 사용한다.

## Docker-compose를 통한 MySQL과 Spring Boot 연결

### init.sql

```sql
// 해당 사용자를 localhost에서만 접근 가능하도록 생성하고 비밀번호 설정
CREATE USER '아이디'@'localhost' IDENTIFIED BY '비밀번호';
// 해당 사용자를 어떤 호스트에서든 접근 가능하도록 생성
CREATE USER '아이디'@'%' IDENTIFIED BY '비밀번호';

// 해당 사용자에게 localhost에서 모든 데이터베이스에 대한 모든 권한 부여
GRANT ALL PRIVILEGES ON *.* TO '아이디'@'localhost';
// 해당 사용자에게 어떤 호스트에서든 모든 데이터베이스에 대한 권한 부여
GRANT ALL PRIVILEGES ON *.* TO '아이디'@'%';

// instagram이라는 데이터베이스를 생성하고, 문자 세트와 콜레이션을 설정
CREATE DATABASE instagram DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

### application.yml

```yaml
spring:
  datasource:
    url: jdbc:mysql://database-mysql-instagram:3306/instagram?useSSL=false&allowPublicKeyRetrieval=true&serverTimezone=UTC&useLegacyDatetimeCode=false
    username: 아이디
    password: 비밀번호
    driver-class-name: com.mysql.cj.jdbc.Driver
```

`url`에는 `docker-compose.yml`에서 설정한 `database-mysql-instagram` 서비스 이름을 사용하여 도커 네트워크 내의 MySQL 서버에 연결한다.

그리고 `init.sql`에서 만든 사용자 아이디와 비밀번호를 `usename`과 `password`에 넣어준다.

## Docker Compose

이제 설정이 끝났다.

compose 명령어를 통해 구성하고, 실행시켜 보자.

```bash
docker-compose up
```

![스크린샷3 2024-02-01 오후 8 48 44](https://github.com/Heo-y-y/development-blog/assets/112863029/b05c7748-6b90-4427-8d5f-b3ecb02850ef)

![스크린샷 2024-02-01 오후 8.47.46.png](https://github.com/Heo-y-y/development-blog/assets/112863029/f2ca8fbd-79da-4c9d-b6d7-988e2f25eaab)

정상적으로 테이블이 만들어지며 실행되고, 기본 도메인이 없어서 Whitelabel Error Page는 뜨지만 서버가 정상적으로 연결된 것을 확인할 수 있다.
