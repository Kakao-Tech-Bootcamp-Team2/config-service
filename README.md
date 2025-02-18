# Config Service

## 개요 🚀

Config Service는 **마이크로서비스들의 설정을 중앙에서 관리**하는 서비스입니다. Spring Cloud Config와 Spring Cloud Bus를 활용하여 **설정을 쉽게 업데이트하고 적용**할 수 있습니다.

애플리케이션을 중단하지 않고 **Actuator의 엔드포인트**를 통해 설정을 업데이트할 수 있으며, **Vault를 이용한 시크릿 관리**를 통해 보안성을 극대화하였습니다.

<br />

## 주요 기능 🔥

- **중앙 집중형 설정 관리**: 모든 마이크로서비스의 설정을 단일 Config Service에서 관리
- **Spring Cloud Config**를 활용하여 설정 변경 사항을 동적으로 반영
- **Spring Cloud Bus**를 이용한 설정 변경 자동 전파
- **Actuator 엔드포인트 활용**: 애플리케이션을 재시작하지 않고 설정 변경 적용
- **Vault 기반 보안 관리**: 데이터베이스, API 키 등의 민감한 정보 안전하게 저장 및 관리
- **Git 연동**: 외부 Git 저장소에서 설정 파일을 관리하여 버전 이력 추적 가능

<br />

## 아키텍처 개요 🏗️

1. 애플리케이션이 기동될 때 Config Service에서 설정 값을 가져옵니다.
2. 설정이 변경되면 Spring Cloud Bus를 이용해 변경 사항이 모든 서비스에 즉시 반영됩니다.
3. 민감한 정보(시크릿)는 Vault에서 안전하게 관리됩니다.
4. Actuator 엔드포인트를 통해 서비스의 재시작 없이 설정을 갱신할 수 있습니다.

```
(Client) -> [Config Service] -> [Spring Cloud Bus] -> [Microservices]
                         ↘
                          [Vault]
```

<br />

## 기술 스택 🛠️

- **Java 17**
- **Spring Boot 3**
- **Spring Cloud Config**
- **Spring Cloud Bus**
- **RabbitMQ** (Spring Cloud Bus 메시지 브로커)
- **Vault** (시크릿 관리)
- **Actuator** (설정 변경 API)
- **Git** (설정 파일 버전 관리)
- **Docker**

<br />

## 설치 및 실행 방법 💻

### 1. 로컬 실행

```sh
./gradlew build && ./gradlew bootRun
```

### 2. Docker 이미지 빌드

```sh
./gradlew bootBuildImage
```

### 3. Docker 실행

Docker 실행 방법 및 관련 설정에 대한 자세한 내용은 아래 저장소의 README를 참고하세요.

📌 저장소 링크: [Zipbob Deployment Repository](https://github.com/Kakao-Tech-Bootcamp-Team2/zipbob-deployment)

<br />

## Actuator 엔드포인트 활용 🛠️

Config Service는 Actuator 엔드포인트를 활용하여 **애플리케이션을 재시작하지 않고 설정을 변경**할 수 있습니다.

### 설정 변경 적용 방법:

```sh
curl -X POST http://localhost:8080/actuator/busrefresh
```

이 명령을 실행하면 Spring Cloud Bus를 통해 변경된 설정이 모든 마이크로서비스에 전파됩니다.

<br />

## 시크릿 관리(Vault) 🔒

Config Service는 **Vault**를 이용하여 시크릿을 안전하게 관리합니다.

### Vault에 시크릿 저장 예제:

```sh
vault kv put secret/application db.username=admin db.password=testexample
```

마이크로서비스는 설정을 가져올 때 Vault에서 해당 값을 안전하게 조회할 수 있습니다.

