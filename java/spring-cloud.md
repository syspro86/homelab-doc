# SpringCloud

Eureka 서버 세팅 시

https://stackoverflow.com/questions/46131196/com-netflix-discovery-shared-transport-transportexception-cannot-execute-reques

```java
server.port=8761

eureka:
  client:
    register-with-eureka: false

```

유레카가 자기 자신의 등록 시도를 막는다. (유레카가 유레카 접속 실패 로그가 자꾸 나옴)

```java
eureka:
  client:
    registerWithEureka: true
    fetchRegistry: true
    serviceUrl:
      defaultZone: http://localhost:8084/eureka/
  instance:
    hostname: localhost

```

클라이언트는 별도 세팅 없으면 http://localhost:8761 로 연결됨
