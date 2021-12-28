# Omnione Enterprise API Server

Omnione Enterprise API Server 아래와 같은 기본 기능을 제공합니다. 
- DID 생성, 수정, 삭제, 조회 기능
- VC 생성, 삭제, 검증 기능(지정된 VC만 기능)
- DID Auth
- DataHub 기능

---
## 1. application.properties 설정 가이드


### 1-1. 항목표

| 프러퍼티명 | 설명 | 설정값 |
| --- | ------------- | ------------- | 
| --- Applicaton Setting --- | | |
| app.blockchain-server-domain | 블록체인 서버 주소 | http://192.168.0.85:8888 |
| app.api-server-domain | 해당 API 서버 주소 | http://10.0.0.139:8081 |
| app.keymanager-path | Wallet 파일경로 | classpath:config/apiserver.wallet |
| app.keymanager-password | Wallet 패스워드 | raonfunfun777* |
| app.omnione-ent-account | omnione.ent 블록체인 계정 | omnione.ent |
| app.admin-account | omni.admin  블록체인 계정 | omni.admin |
| app.issuer-account | omni.issuer  블록체인 계정 | omni.issuer |
| app.sp-account | omni.sp  블록체인 계정 | omni.sp |
| app.issuer-did-path | omni.issuer DID 파일 경로 | classpath:config/issuer.did |
| app.sp-did-path | omni.sp DID 파일 경로 | classpath:config/sp.did |
| app.default-vc-type-code | 기본 VC Type Code | driverlicen |
| app.default-service-code | 기본 서비스 코드 | spdriver |
| app.vc-encrypt-type | VC 제출 정보 암호화 여부 | true |
| app.verify-check-blockchain | VC 사본 블록체인에게 직접 검증 | true |
| app.convert-sha-litten-endian |  | false |
| app.lincense-file-path | 라이선스 파일 경로 | omnione_enterprise_server.rsl |
| --- Log Setting --- | | |
| app.sdk-detail-log | SDK 상세로그 출력 | true |
| logging.level.com.raonsecure | 로그 레벨 | debug |
| logging.file.name | 로그 파일명 | ./logs/application.log |
| --- Web Server Setting --- | | |
| server.servlet.context-path | WEB Context 패스 | /omniapi |
| server.port | 서버 포트 | 8081 |
| server.ssl.enabled | SSL 설정 여부 | true |
| server.ssl.key-store | SSL Keystore 경로 | certificate.pfx |
| server.ssl.key-store-type | SSL Keystore 타입 | JKS |
| server.ssl.key-store-password | SSL Keystore 비밀번호 | aa11 |
| spring.application.name | Applicaton 명 | OmniOne-Ent-API-Server |
| spring.profiles.active | 프로파일(기본 product) | dev |
| --- Spring Boot Admin Setting --- | | |
| spring.boot.admin.client.url | boot Admin 설정 | http://localhost:9090 |
| spring.boot.admin.client.instance.service-base-url | boot Admin 설정 | http://localhost:8081 |
| spring.boot.admin.client.username | boot Admin 설정 | omnione |
| spring.boot.admin.client.password | boot Admin 설정 | omnioneFunEnt1 |
| management.endpoints.web.exposure.include | boot Admin 설정 | * |
| management.endpoint.health.show-details | boot Admin 설정 | ALWAYS |
| management.endpoint.shutdown.enabled | boot Admin 설정 | false |
| --- Mariadb config --- | |
| spring.datasource.driver-class-name | JDBC 드라이버 네임 | org.mariadb.jdbc.Driver |
| spring.datasource.url | DB 접속 정보 | jdbc:mariadb://192.168.0.85:3306/omni_datahub?useUnicode |
| spring.jpa.properties.hibernate.default_schema |  | omni_datahub |
| --- Database commom config --- | |
| spring.datasource.password | DB 패스워드 | omnione |
| spring.datasource.username | DB 유저 네임 | root |
| spring.jpa.generate-ddl |  | true |
| spring.jpa.hibernate.ddl-auto |  | update |
| spring.jpa.hibernate.naming.physical-strategy |  | org.hibernate.boot.model.naming.PhysicalNamingStrategyStandardImpl |
| spring.jpa.hibernate.use-new-id-generator-mappings |  | false |
| spring.jpa.show-sql |  | true |
| --- Scheduler --- | |
| spring.task.scheduling.pool.size |  | 2 |
| spring.task.scheduling.thread-name-prefix |  | datahub-scheduler |
| app.data-scheduler-fixed-delay | 만료 데이터 삭제 주기 | 60000 |
| --- WEB config --- | |
| spring.thymeleaf.enabled |  | true |
| spring.resources.add-mappings |  | true |
| spring.thymeleaf.encoding |  | UTF-8 |
| spring.thymeleaf.suffix |  | .html |


### 1-2. 배포 설정 

#### Swagger 표시 제거 
배포된 모듈에서는 기본으로 Swagger UI 및 테스트 Html 페이지는 표시하지 않도록 세팅합니다. 
 * spring.profiles.active = product
 * spring.thymeleaf.enabled = false
 * spring.resources.add-mappings = false




***
## 2. API 문서 생서 가이드
API 문서는 swagger를 통해 생성하고 redoc를 통해 배포할 html를 생성 합니다. 

### 1-1. Swagger UI
- UI 접속 주소: /omniapi/swagger-ui.html
- swagger.json 주소: /omniapi/v2/api-docs


### 1-2. Swagger Editor
Swagger UI가 자동을 생성해준 swagger.json를 Swagger Editor를 내용을 수정합니다.
> Swagger Editor를 반드시 사용할 필요는 없지만, 불필요한 내용이나 수정이 필요한 부분이 있을 때 사용합니다. 

#### Swagger Editor 설치(Docker)
```shell
# docker pull swaggerapi/swagger-editor
# docker run -d -p 30080:8080 swaggerapi/swagger-editor
```

### 1-3. Redoc CLI
Redoc를 통하여 swagger.json를 HTML로 변환 합니다. 

설치
```shell
# npm install -g redoc-cli
```

변환
```shell
# redoc-cli bundle -o index.html swagger.json
```

API HTML 주소 
> * /omniapi/swagger_default_api.html
> * /omniapi/swagger_datahub_api.html


