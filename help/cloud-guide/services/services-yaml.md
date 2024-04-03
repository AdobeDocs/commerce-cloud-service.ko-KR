---
title: 서비스 구성
description: 클라우드 인프라에서 Adobe Commerce에서 사용하는 서비스를 구성하는 방법에 대해 알아봅니다.
feature: Cloud, Configuration, Services
exl-id: 48091c10-c53f-4aad-afbe-b4516653bda1
source-git-commit: 4d790bff2ba5d02ef10de5c36a2f0d140e31a407
workflow-type: tm+mt
source-wordcount: '1007'
ht-degree: 0%

---

# 서비스 구성

다음 `services.yaml` file은 MySQL, Redis 및 OpenSearch와 같은 클라우드 인프라에서 Adobe CommerceElasticsearch 가 지원하고 사용하는 서비스를 정의합니다. 외부 서비스 공급자에 가입할 필요가 없습니다. 이 파일은 다음에 있습니다. `.magento` 프로젝트의 디렉터리입니다.

배포 스크립트는 `.magento` 구성된 서비스로 환경을 프로비저닝할 디렉토리입니다. 서비스가에 포함된 경우 애플리케이션에서 사용할 수 있게 됩니다. [`relationships`](../application/properties.md#relationships) 의 속성 `.magento.app.yaml` 파일. 다음 `services.yaml` 파일에 _유형_ 및 _디스크_ 값. 서비스 유형은 서비스를 정의합니다. _이름_ 및 _버전_.

서비스 구성을 변경하면 배포에서 업데이트된 서비스로 환경을 프로비저닝하게 되며, 이는 다음 환경에 영향을 줍니다.

- 프로덕션을 포함한 모든 스타터 환경 `master`
- Pro 통합 환경

{{pro-update-service}}

## 기본 및 지원 서비스

클라우드 인프라는 다음 서비스를 지원하고 배포합니다.

- [MySQL](mysql.md)
- [레디스](redis.md)
- [RabbitMQ](rabbitmq.md)
- [Elasticsearch](elasticsearch.md)
- [OpenSearch](opensearch.md)

현재 버전에서의 기본 버전 및 디스크 값을 볼 수 있습니다. [기본값 `services.yaml` 파일](https://github.com/magento/magento-cloud/blob/master/.magento/services.yaml). 다음 샘플은 `mysql`, `redis`, `opensearch` 또는 `elasticsearch`, 및 `rabbitmq` 에 정의된 서비스 `services.yaml` 구성 파일:

```yaml
mysql:
    type: mysql:10.4
    disk: 5120

redis:
    type: redis:6.2

opensearch:
    type: opensearch:2  # minor version not required; uses latest
    disk: 1024

rabbitmq:
    type: rabbitmq:3.9
    disk: 1024
```

## 서비스 값

서비스 ID 및 서비스 유형 구성을 제공해야 합니다. `type: <name>:<version>`. 서비스가 영구 스토리지를 사용하는 경우 디스크 값을 제공해야 합니다.

다음 형식을 사용하십시오.

```yaml
<service-id>:
    type: <name>:<version>
    disk: <value-MB>
```

### `service-id`

다음 `service-id` 값은 프로젝트에서 서비스를 식별합니다. 소문자 영숫자만 사용할 수 있습니다. `a` 끝 `z` 및 `0` 끝 `9`, 예: `redis`.

이 _service-id_ 값은에서 사용됩니다. [`relationships`](../application/properties.md#relationships) 의 속성 `.magento.app.yaml` 구성 파일:

```yaml
relationships:
    redis: "<name>:redis"
```

각 서비스 유형의 여러 인스턴스에 이름을 지정할 수 있습니다. 예를 들어 세션 인스턴스와 캐시 인스턴스 인스턴스를 여러 개 사용할 수 있습니다.

```yaml
redis:
    type: redis:<version>

redis2:
    type: redis:<version>
```

에서 서비스 이름 바꾸기 `services.yaml` 파일 **영구적으로 제거** 다음:

- 지정한 새 이름으로 서비스를 만들기 전의 기존 서비스입니다.
- 서비스에 대한 기존 데이터가 모두 제거됩니다. Adobe은 다음을 강력히 권장합니다. [스타터 환경 백업](../storage/snapshots.md) 기존 서비스의 이름을 변경하기 전에

### `type`

다음 `type` 값은 서비스 이름과 버전을 지정합니다. For example:

```yaml
mysql:
    type: mysql:10.4
```

### `disk`

다음 `disk` 값은 서비스에 할당할 영구 디스크 저장소의 크기(MB)를 지정합니다. MySQL과 같은 영구 저장소를 사용하는 서비스는 디스크 값을 제공해야 합니다. Redis와 같이 영구 저장소 대신 메모리를 사용하는 서비스에는 디스크 값이 필요하지 않습니다.

```yaml
mysql:
    type: mysql:10.4
    disk: 5120
```

프로젝트당 현재 기본 스토리지 용량은 5GB 또는 512 0MB입니다. 이 금액을 애플리케이션과 각 서비스 간에 분배할 수 있습니다.

## 서비스 관계

Adobe Commerce에서 클라우드 인프라 프로젝트, 서비스 [관계](../application/properties.md#relationships) 에서 구성됨 `.magento.app.yaml` 파일은 애플리케이션에서 사용할 수 있는 서비스를 결정합니다.

에서 모든 서비스 관계에 대한 구성 데이터를 검색할 수 있습니다. [`$MAGENTO_CLOUD_RELATIONSHIPS`](../environment/variables-cloud.md) 환경 변수입니다. 구성 데이터에는 포트 번호 및 로그인 자격 증명과 같은 필수 연결 세부 정보와 함께 서비스 이름, 유형 및 버전이 포함됩니다.

**로컬 환경에서 관계를 확인하려면**:

1. 로컬 환경에서 활성 환경의 관계를 표시합니다.

   ```bash
   magento-cloud relationships
   ```

1. 확인 `service` 및 `type` 응답. 이 응답은 IP 주소 및 포트 번호와 같은 연결 정보를 제공합니다.

   >축약된 샘플 응답

   ```terminal
   redis:
       -
   ...
           type: 'redis:7.0'
           port: 6379
   elasticsearch:
       -
   ...
           type: 'opensearch:2'
           port: 9200
   database:
       -
   ...
           type: 'mysql:10.6'
           port: 3306
   ```

**원격 환경에서 관계를 확인하려면**:

1. SSH를 사용하여 원격 환경에 로그인합니다.

1. 환경에 구성된 모든 서비스의 관계 구성 데이터를 나열합니다.

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   또는 다음을 사용하십시오 `ece-tools` 관계를 보는 명령:

   ```bash
   php ./vendor/bin/ece-tools env:config:show services
   ```

1. 확인 `service` 및 `type` 응답. 이 응답은 IP 주소 및 포트 번호와 필요한 사용자 이름 및 암호 자격 증명과 같은 연결 정보를 제공합니다.

## 서비스 버전

클라우드 인프라에서 Adobe Commerce에 대한 서비스 버전 및 호환성 지원은 클라우드 인프라에서 배포되고 테스트된 버전에 따라 결정되며 Adobe Commerce 온프레미스 배포에서 지원하는 버전과 다른 경우가 있습니다. 다음을 참조하십시오 [시스템 요구 사항](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html) 다음에서 _설치_ Adobe이 특정 Adobe Commerce 및 Magento Open Source 릴리스에서 테스트한 타사 소프트웨어 종속성 목록을 안내합니다.

### 소프트웨어 EOL 확인

배포 프로세스 중에 `ece-tools` 설치된 서비스 버전을 각 서비스의 EOL(서비스 종료) 날짜에 대해 확인합니다.

- 서비스 버전이 EOL 날짜로부터 3개월 이내인 경우 배포 로그에 알림이 표시됩니다.
- EOL 날짜가 과거인 경우 경고 알림이 표시됩니다.

저장소 보안을 유지하려면 설치된 소프트웨어 버전이 EOL에 도달하기 전에 업데이트하십시오. 다음 문서에서 EOL 날짜를 검토할 수 있습니다. [ece-tools `eol.yaml` 파일](https://github.com/magento/ece-tools/blob/develop/config/eol.yaml).

### OpenSearch로 마이그레이션

{{elasticsearch-support}}

Adobe Commerce 버전 2.4.4 이상의 경우 [OpenSearch 서비스 설정](opensearch.md).

## 서비스 버전 변경

설치된 서비스 버전을 업그레이드하여 클라우드 환경에 배포된 Adobe Commerce 버전과의 호환성을 유지할 수 있습니다.

설치된 서비스의 서비스 버전을 직접 다운그레이드할 수 없습니다. 그러나 필요한 버전으로 서비스를 만들 수는 있습니다. 다음을 참조하십시오 [서비스 버전 다운그레이드](#downgrade-version).

### 설치된 서비스 버전 업그레이드

에서 서비스 구성을 업데이트하여 설치된 서비스 버전을 업그레이드할 수 있습니다. `services.yaml` 파일.

1. 변경 [`type`](#type) 에 있는 서비스에 대한 값 `.magento/services.yaml` 파일:

   > 원래 서비스 정의

   ```yaml
   mysql:
       type: mysql:10.3
       disk: 2048
   ```

   > 업데이트된 서비스 정의

   ```yaml
   mysql:
       type: mysql:10.4
       disk: 5120
   ```

1. 코드 변경 사항을 추가, 커밋 및 푸시합니다.

   ```bash
   git add .magento/services.yaml
   ```

   ```bash
   git commit -m "Upgrade MySQL from MariaDB 10.3 to 10.4."
   ```

   ```bash
   git push origin <branch-name>
   ```

### 다운그레이드 버전

설치된 서비스를 직접 다운그레이드할 수는 없습니다. 다음 두 가지 옵션이 있습니다.

1. 기존 서비스의 이름을 새 버전으로 바꾸십시오. 새 버전으로 바꾸면 기존 서비스와 데이터가 제거되고 새 서비스가 추가됩니다.

1. 서비스를 만들고 기존 서비스의 데이터를 저장합니다.

서비스 버전을 변경할 때에서 서비스 구성을 업데이트해야 합니다 `services.yaml` 을(를) 만들고 의 관계를 업데이트합니다. `.magento.app.yaml` 파일.

**기존 서비스의 이름을 변경하여 서비스 버전을 다운그레이드하려면**:

1. 에서 기존 서비스 이름 바꾸기 `.magento/services.yaml` 파일을 만들고 버전을 변경합니다.

   >[!WARNING]
   >
   >기존 서비스의 이름을 바꾸면 기존 서비스가 바뀌고 모든 데이터가 삭제됩니다. 데이터를 유지해야 하는 경우 기존 서비스의 이름을 바꾸는 대신 서비스를 만듭니다.

   예를 들어 의 MariaDB 버전을 다운그레이드하려면 _mysql_ 서비스 버전 10.4에서 10.3으로, 기존 버전 변경 _service-id_ 및 _유형_ 구성.

   > 원본 `services.yaml` 정의

   ```yaml
   mysql:
       type: mysql:10.4
       disk: 5120
   ```

   > 신규 `services.yaml` 정의

   ```yaml
   mysql2:
        type: mysql:10.3
        disk: 5120
   ```

1. 에서 관계 업데이트 `.magento.app.yaml` 파일.

   > 원본 `.magento.app.yaml` 구성

   ```yaml
   relationships:
       database: "mysql:mysql"
   ```

   > 업데이트됨 `.magento.app.yaml` 구성

   ```yaml
   relationships:
       database: "mysql2:mysql"
   ```

1. 코드 변경 사항을 추가, 커밋 및 푸시합니다.

**서비스를 생성하여 서비스를 다운그레이드하려면**:

1. 에 서비스 정의 추가 `services.yaml` 다운그레이드된 버전 사양이 있는 프로젝트용 파일입니다. 다음을 참조하십시오 _mysql2_ 다음 예제에서는

   > services.yaml

   ```yaml
   mysql:
       type: mysql:10.4
       disk: 5120
   mysql2:
       type: mysql:10.3
       disk: 5120
   ```

1. 에서 관계 구성 변경 `.magento.app.yaml` 새 서비스를 사용할 파일입니다.

   > 원본 `.magento.app.yaml` 구성

   ```yaml
   relationships:
       database: "mysql:mysql"
   ```

   > 신규 `.magento.app.yaml` 구성

   ```yaml
   relationships:
       database: "mysql2:mysql"
   ```

1. 코드 변경 사항을 추가, 커밋 및 푸시합니다.
