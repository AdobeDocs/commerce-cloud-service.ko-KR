---
title: MySQL 서비스 설정
description: 클라우드 인프라의 Adobe Commerce을 사용하여 영구 데이터 스토리지에 대한 MySQL 서비스를 관리하는 방법을 알아봅니다.
feature: Cloud, Services, Storage
exl-id: 70820d00-8b82-4b60-87e4-ea98fd7ffcb2
source-git-commit: 9be8d1e062ab3dba86bc4d9c9ee8b8ece33d5b75
workflow-type: tm+mt
source-wordcount: '773'
ht-degree: 1%

---

# MySQL 서비스 설정

다음 `mysql` 이 서비스는 다음을 기반으로 영구 데이터 스토리지 제공 [마리아DB](https://mariadb.com/) 버전 10.2에서 10.4로, [XtraDB](https://docs.percona.com/percona-xtradb-cluster/8.0/index.html) MySQL 5.6 및 5.7에서 스토리지 엔진 및 재구현된 기능

MariaDB 10.4에 대한 리인덱싱은 다른 MariaDB 또는 MySQL 버전에 비해 시간이 더 걸립니다. 다음을 참조하십시오 [인덱서](https://experienceleague.adobe.com/docs/commerce-operations/performance-best-practices/configuration.html#indexers) 다음에서 _성능 모범 사례_ 가이드.

>[!WARNING]
>
>MariaDB를 버전 10.1에서 10.2로 업그레이드할 때 주의하십시오. MariaDB 10.1은 지원되는 마지막 버전입니다. _XtraDB_ 스토리지 엔진으로 사용됩니다. MariaDB 10.2는 _InnoDB_ 스토리지 엔진용 10.1에서 10.2로 업그레이드한 후에는 변경 사항을 롤백할 수 없습니다. Adobe Commerce은 두 스토리지 엔진을 모두 지원합니다. 그러나 프로젝트에서 사용하는 확장 및 기타 시스템이 MariaDB 10.2와 호환되는지 확인해야 합니다. 다음을 참조하십시오 [10.1과 10.2 사이의 호환되지 않는 변경 사항](https://mariadb.com/kb/en/upgrading-from-mariadb-101-to-mariadb-102/#incompatible-changes-between-101-and-102).

{{service-instruction}}

**MySQL을 사용하려면**:

1. 필요한 이름, 유형 및 디스크 값(MB)을 `.magento/services.yaml` 파일.

   ```yaml
   mysql:
       type: mysql:<version>
       disk: 5120
   ```

   >[!TIP]
   >
   >다음과 같은 MySQL 오류 `PDO Exception: MySQL server has gone away`디스크 공간이 부족하면 가 발생할 수 있습니다. 의 서비스에 충분한 디스크 공간을 할당했는지 확인합니다. [`.magento/services.yaml`](services-yaml.md#disk) 파일.

1. 에서 관계 구성 `.magento.app.yaml` 파일.

   ```yaml
   relationships:
       database: "mysql:mysql"
   ```

1. 코드 변경 사항을 추가, 커밋 및 푸시합니다.

   ```bash
   git add .magento/services.yaml .magento.app.yaml && git commit -m "Enable mysql service" && git push origin <branch-name>
   ```

1. [서비스 관계 확인](services-yaml.md#service-relationships).

{{service-change-tip}}

## MySQL 데이터베이스 구성

MySQL 데이터베이스를 구성할 때 다음과 같은 옵션이 있습니다.

- **`schemas`**—스키마가 데이터베이스를 정의합니다. 기본 스키마는 `main` 데이터베이스.
- **`endpoints`**- 각 끝점은 특정 권한이 있는 자격 증명을 나타냅니다. 기본 끝점은 입니다. `mysql`, , `admin` 액세스 권한: `main` 데이터베이스.
- **`properties`**- 속성을 사용하여 추가 데이터베이스 구성을 정의합니다.

다음은 의 기본 예제 구성입니다. `.magento/services.yaml` 파일:

```yaml
mysql:
    type: mysql:10.4
    disk: 5120
    configuration:
        properties:
            optimizer_switch: "rowid_filter=off"
            optimizer_use_condition_selectivity: 1
```

다음 `properties` 위의 예에서 기본값이 수정됩니다. `optimizer` 다음으로 설정 [성능 모범 사례 안내서에서 권장됨](https://experienceleague.adobe.com/docs/commerce-operations/performance-best-practices/configuration.html#indexers).

**MariaDB 구성 옵션**:

| 옵션 | 설명 | 기본값 |
| -------------------- | --------------------------------------------------- | ------------------ |
| `default_charset` | 기본 문자 집합입니다. | utf8mb4 |
| `default_collation` | 기본 데이터 정렬입니다. | utf8mb4_unicode_ci |
| `max_allowed_packet` | 패킷의 최대 크기(MB)입니다. 범위 `1` 끝 `100`. | 16 |
| `optimizer_switch` | 쿼리 최적화 프로그램에 대한 값을 설정합니다. 다음을 참조하십시오 [MariaDB 설명서](https://mariadb.com/kb/en/server-system-variables/#optimizer_switch). | |
| `optimizer_use_condition_selectivity` | 최적기에서 사용하는 통계를 선택합니다. 범위 `1` 끝 `5`. 다음을 참조하십시오 [MariaDB 설명서](https://mariadb.com/kb/en/server-system-variables/#optimizer_use_condition_selectivity). | 10.4 이상용 4 |

### 여러 데이터베이스 사용자 설정

선택적으로, 다음에 액세스할 수 있는 다양한 권한을 가진 여러 사용자를 설정할 수 있습니다. `main` 데이터베이스.

기본적으로 이라는 하나의 엔드포인트가 있습니다. `mysql` 데이터베이스에 대한 관리자 액세스 권한이 있습니다. 여러 데이터베이스 사용자를 설정하려면 `services.yaml` 에서 관계를 파일화하고선언합니다. `.magento.app.yaml` 파일. Pro 스테이징 및 프로덕션 환경의 경우 [Adobe Commerce 지원 티켓 제출](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) 추가 사용자를 요청합니다.

중첩된 배열을 사용하여 특정 사용자 액세스에 대한 끝점을 정의합니다. 각 끝점은 하나 이상의 스키마(데이터베이스)에 대한 액세스와 각 항목에 대한 서로 다른 권한 수준을 지정할 수 있습니다.

유효한 권한 수준은 다음과 같습니다.

- `ro`: SELECT 쿼리만 허용됩니다.
- `rw`: SELECT 쿼리와 INSERT, UPDATE 및 DELETE 쿼리가 허용됩니다.
- `admin`: DDL 쿼리(CREATE TABLE, DROP TABLE 등)를 포함한 모든 쿼리가 허용됩니다.

For example:

```yaml
mysql:
    type: mysql:10.4
    disk: 5120
    configuration:
        schemas:
            - main
        endpoints:
            admin:
                default_schema: main
                privileges:
                    main: admin
            reporter:
                privileges:
                    main: ro
            importer:
                privileges:
                    main: rw
        properties:
            optimizer_switch: "rowid_filter=off"
            optimizer_use_condition_selectivity: 1
```

앞의 예제에서는 `admin` 끝점은 관리 수준 액세스 권한을 제공합니다. `main` 데이터베이스, `reporter` endpoint는 읽기 전용 액세스를 제공하며 `importer` endpoint는 읽기-쓰기 액세스를 제공하며, 이는 다음을 의미합니다.

- 다음 `admin` 사용자는 데이터베이스에 대한 모든 권한을 갖습니다.
- 다음 `reporter` 사용자는 SELECT 권한만 있습니다.
- 다음 `importer` 사용자는 SELECT, INSERT, UPDATE 및 DELETE 권한이 있습니다.

위의 예에서 정의한 끝점을 `relationships` 의 속성 `.magento.app.yaml` 파일. For example:

```yaml
relationships:
    database: "mysql:admin"
    databasereporter: "mysql:reporter"
    databaseimporter: "mysql:importer"
```

>[!NOTE]
>
>MySQL 사용자 하나를 구성하는 경우 [`DEFINER`](https://dev.mysql.com/doc/refman/8.0/en/show-grants.html) 저장 프로시저 및 뷰에 대한 액세스 제어 메커니즘.

## 데이터베이스에 연결

MariaDB 데이터베이스에 직접 액세스하려면 SSH를 사용하여 원격 클라우드 환경에 로그인하고 데이터베이스에 연결해야 합니다.

1. SSH를 사용하여 원격 환경에 로그인합니다.

   ```bash
   magento-cloud ssh
   ```

1. 에서 MySQL 로그인 자격 증명 검색 `database` 및 `type` 의 속성 [$MAGENTO_클라우드_관계](../application/properties.md#relationships) 변수를 채우는 방법에 따라 페이지를 순서대로 표시합니다.

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   또는

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"])));'
   ```

   응답에서 MySQL 정보를 찾습니다. For example:

   ```json
   "database" : [
      {
         "password" : "",
         "rel" : "mysql",
         "hostname" : "nnnnnnnn.mysql.service._.magentosite.cloud",
         "service" : "mysql",
         "host" : "database.internal",
         "ip" : "###.###.###.###",
         "port" : 3306,
         "path" : "main",
         "cluster" : "projectid-integration-id",
         "query" : {
            "is_master" : true
         },
         "type" : "mysql:10.3",
         "username" : "user",
         "scheme" : "mysql"
      }
   ],
   ```

1. 데이터베이스에 연결합니다.

   - Starter의 경우 다음 명령을 사용합니다.

     ```bash
     mysql -h database.internal -u <username>
     ```

   - Pro의 경우 다음 명령을 사용할 때 호스트 이름, 포트 번호, 사용자 이름 및 암호는 `$MAGENTO_CLOUD_RELATIONSHIPS` 변수를 채우는 방법에 따라 페이지를 순서대로 표시합니다.

     ```bash
     mysql -h <hostname> -P <number> -u <username> -p'<password>'
     ```

>[!TIP]
>
>다음을 사용할 수 있습니다. `magento-cloud db:sql` 원격 데이터베이스에 연결하고 SQL 명령을 실행하는 명령

## 보조 데이터베이스에 연결

>[!IMPORTANT]
>
>이 기능은 프로프로덕션 및 스테이징 클러스터에서만 사용할 수 있습니다.

데이터베이스 성능을 향상시키거나 데이터베이스 잠금 문제를 해결하기 위해 보조 데이터베이스에 연결해야 하는 경우가 있습니다. 이 구성이 필요한 경우 다음을 사용합니다. `"port" : 3304` 연결을 설정합니다. 다음을 참조하십시오. [MySQL 슬레이브 연결을 구성하는 우수 사례](https://experienceleague.adobe.com/docs/commerce-operations/implementation-playbook/best-practices/planning/mysql-configuration.html) 의 주제 _구현 우수 사례_ 가이드.

## 문제 해결

MySQL 문제 해결에 대한 도움말은 다음 Adobe Commerce 지원 문서를 참조하십시오.

- [느린 쿼리 및 프로세스 MySQL 확인](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/database/checking-slow-queries-and-processes-mysql.html)
- [클라우드에서 데이터베이스 덤프 만들기](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/create-database-dump-on-cloud.html)
- [데이터 마이그레이션 도구 문제 해결](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/data-migration-tool-troubleshooting.html)
- [Adobe Commerce 업그레이드: 컴팩트에서 다이내믹 테이블 2.2.x, 2.3.x에서 2.4.x로](https://experienceleague.adobe.com/docs/commerce-operations/implementation-playbook/best-practices/maintenance/commerce-235-upgrade-prerequisites-mariadb.html)
