---
title: 변수 배포
description: Adobe Commerce on cloud infrastructure 배포 단계에서 작업을 제어하는 환경 변수 목록을 참조하십시오.
feature: Cloud, Configuration, Cache, Deploy, SCD, Storage, Search
recommendations: noDisplay, catalog
role: Developer
exl-id: 673880b5-830b-4837-ac0c-5fa5643ae34c
source-git-commit: 7c09a0041f35890f34a00a8eea8f0154dc0f5dcd
workflow-type: tm+mt
source-wordcount: '2209'
ht-degree: 0%

---

# 변수 배포

다음 _배포_ 변수는 배포 단계에서 작업을 제어하며 [전역 변수](variables-global.md)에서 값을 상속하고 재정의할 수 있습니다. `.magento.env.yaml` 파일의 `deploy` 단계에 다음 변수를 삽입합니다.

```yaml
stage:
  deploy:
    DEPLOY_VARIABLE_NAME: value
```

빌드 및 배포 프로세스 사용자 지정에 대한 자세한 내용은 다음을 참조하십시오.

- [배포 구성](configure-env-yaml.md)
- [배포 프로세스](../deploy/process.md)

## `CACHE_CONFIGURATION`

- **기본값**—_설정되지 않음_
- **버전**—Adobe Commerce 2.1.4 이상

Redis 페이지 및 기본 캐싱을 구성합니다. `cm_cache_backend_redis` 매개 변수를 설정할 때는 `server`, `port` 및 `database` 옵션을 지정해야 합니다.

```yaml
stage:
  deploy:
    CACHE_CONFIGURATION:
      frontend:
        default:
          backend: file
        page_cache:
          backend: file
```

{{merge-options}}

다음 예제에서는 새 값을 기존 구성에 병합합니다.

```yaml
stage:
  deploy:
    CACHE_CONFIGURATION:
      _merge: true
      frontend:
        default:
          backend_options:
            database: 10
        page_cache:
          backend_options:
            database: 11
```

다음 예제에서는 _구성 안내서_&#x200B;에 정의된 대로 [Redis 미리 로드 기능](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/redis/redis-pg-cache.html#redis-preload-feature)을 사용합니다.

```yaml
stage:
  deploy:
    CACHE_CONFIGURATION:
      _merge: true
      frontend:
        default:
          id_prefix: '061_'
          backend_options:
            preload_keys:
              - '061_EAV_ENTITY_TYPES:hash'
              - '061_GLOBAL_PLUGIN_LIST:hash'
              - '061_DB_IS_UP_TO_DATE:hash'
              - '061_SYSTEM_DEFAULT:hash'
```

사용자 지정 [REDIS_BACKEND](#redis_backend) 모델(허용 목록에서만)을 사용하려면 `_custom_redis_backend` 옵션을 `true`(으)로 설정하여 다음 예제와 같이 올바른 유효성 검사를 사용하도록 설정하십시오.

```yaml
stage:
  deploy:
    CACHE_CONFIGURATION:
      frontend:
        default:
          _custom_redis_backend: true
          backend: '\CustomRedisModel'
```

## `CLEAN_STATIC_FILES`

- **기본값**—`true`
- **버전**—Adobe Commerce 2.1.4 이상

빌드 또는 배포 단계에서 생성된 [정적 콘텐츠 파일](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment.html)을(를) 정리하거나 사용하지 않도록 설정합니다. 개발에서 기본값 _true_&#x200B;을(를) 사용하는 것이 좋습니다.

- **`true`** - 업데이트된 정적 콘텐츠를 배포하기 전에 기존 정적 콘텐츠를 모두 제거합니다.
- **`false`** - 생성된 콘텐츠에 최신 버전이 포함된 경우 배포는 기존 정적 콘텐츠 파일만 덮어씁니다.

별도의 프로세스를 통해 정적 콘텐츠를 수정하는 경우 값을 _false_(으)로 설정하십시오.

```yaml
stage:
  deploy:
    CLEAN_STATIC_FILES: false
```

배포 전에 정적 보기 파일을 정리하지 않으면 이전 버전을 제거하지 않고 기존 파일에 업데이트를 배포할 경우 문제가 발생할 수 있습니다. [정적 파일 대체](https://developer.adobe.com/commerce/frontend-core/guide/caching/#clean-static-files-cache) 규칙으로 인해 디렉터리에 동일한 파일의 버전이 여러 개 있는 경우 대체 작업에 잘못된 파일이 표시될 수 있습니다.

## `CRON_CONSUMERS_RUNNER`

- **기본값**—`cron_run = false`, `max_messages = 1000`
- **버전**—Adobe Commerce 2.2.0 이상

이 환경 변수를 사용하여 배포 후 메시지 큐가 실행 중인지 확인하십시오.

- `cron_run`—`consumers_runner` cron 작업을 활성화하거나 비활성화하는 부울 값입니다(기본값 = `false`).
- `max_messages`—종료하기 전에 각 소비자가 처리해야 하는 최대 메시지 수를 지정하는 숫자입니다(기본값 = `1000`). 이 값을 `0`(으)로 설정하여 소비자가 종료되는 것을 방지할 수 있습니다.
- `consumers`—실행할 소비자를 지정하는 문자열 배열입니다. 빈 배열은 _모두_ 소비자를 실행합니다.

- `multiple_processes`-각 소비자에 대해 생성할 프로세스 수를 지정하는 숫자입니다. Commerce **2.4.4** 이상에서 지원됩니다.

>[!NOTE]
>
>메시지 큐 `consumers` 목록을 반환하려면 원격 환경에서 `./bin/magento queue:consumers:list` 명령을 실행하십시오.

각 소비자에 대해 생성할 특정 `consumers` 및 `multiple_processes`을(를) 실행하는 예제 배열:

```yaml
stage:
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      max_messages: 1000
      consumers:
        - example_consumer_1
        - example_consumer_2
-     multiple_processes:
        example_consumer_1: 4
        example_consumer_2: 3
```

`consumers`을(를) 모두 실행하는 빈 배열의 예:

```yaml
stage:
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      max_messages: 1000
      consumers: []
```

기본적으로 배포 프로세스는 `env.php` 파일의 모든 설정을 덮어씁니다. 온-프레미스 Adobe Commerce에 대해서는 _Commerce 구성 가이드_&#x200B;의 [메시지 큐 관리](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/message-queues/manage-message-queues.html)를 참조하세요.

## `CONSUMERS_WAIT_FOR_MAX_MESSAGES`

- **기본값**—`false`
- **버전**—Adobe Commerce 2.2.0 이상

다음 옵션 중 하나를 선택하여 `consumers`에서 메시지 큐의 메시지를 처리하는 방법을 구성하십시오.

- `false`—`Consumers`이(가) 큐에서 사용 가능한 메시지를 처리하고 TCP 연결을 닫고 종료합니다. `Consumers`은(는) 처리된 메시지 수가 `CRON_CONSUMERS_RUNNER` 배포 변수에 지정된 `max_messages` 값보다 작더라도 추가 메시지가 대기열에 들어갈 때까지 기다리지 않습니다.

- `true`—`Consumers`은(는) TCP 연결을 닫고 소비자 프로세스를 종료하기 전에 `CRON_CONSUMERS_RUNNER` 배포 변수에 지정된 최대 메시지 수(`max_messages`)에 도달할 때까지 메시지 큐에서 메시지를 계속 처리합니다. `max_messages`에 도달하기 전에 큐를 비우면 소비자는 더 많은 메시지가 도착할 때까지 기다립니다.

>[!WARNING]
>
>크론 작업을 사용하는 대신 작업자를 사용하여 `consumers`을(를) 실행하는 경우 이 변수를 true로 설정하십시오.

```yaml
stage:
  deploy:
    CONSUMERS_WAIT_FOR_MAX_MESSAGES: false
```

## `CRYPT_KEY`

- **기본값**—_설정되지 않음_
- **버전**—Adobe Commerce 2.1.4 이상

>[!WARNING]
>
>`.magento.env.yaml` 파일 대신 [!DNL Cloud Console]을(를) 통해 `CRYPT_KEY` 값을 설정하여 환경에 대한 소스 코드 리포지토리의 키가 노출되지 않도록 하십시오. [환경 및 프로젝트 변수 설정](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/project/overview.html#configure-environment)을 참조하십시오.

설치 프로세스 없이 데이터베이스를 한 환경에서 다른 환경으로 이동할 때는 해당 암호화 정보가 필요합니다. Adobe Commerce에서는 [!DNL Cloud Console]에 설정된 암호화 키 값을 `env.php` 파일의 `crypt/key` 값으로 사용합니다.

## `DATABASE_CONFIGURATION`

- **기본값**—_설정되지 않음_
- **버전**—Adobe Commerce 2.1.4 이상

`.magento.app.yaml` 파일의 [관계 속성](../application/properties.md#relationships)에서 데이터베이스를 정의한 경우 배포를 위해 데이터베이스 연결을 사용자 지정할 수 있습니다.

```yaml
stage:
  deploy:
    DATABASE_CONFIGURATION:
      some_config: 'some_value'
```

{{merge-options}}

다음 예제에서는 새 값을 기존 구성에 병합합니다.

```yaml
stage:
  deploy:
    DATABASE_CONFIGURATION:
      some_config: 'some_new_value'
      _merge: true
```

또한 테이블 접두사를 구성할 수 있습니다.

>[!WARNING]
>
>테이블 접두사와 함께 병합 옵션을 사용하지 않는 경우 기본 연결 설정을 제공해야 합니다. 그렇지 않으면 배포가 유효성 검사에 실패합니다.

다음 예제에서는 `_merge` 옵션 대신 기본 연결 설정과 함께 `ece_` 테이블 접두사를 사용합니다.

```yaml
stage:
  deploy:
    DATABASE_CONFIGURATION:
      connection:
        default:
          username: user
          host: host
          dbname: magento
          password: password
      table_prefix: 'ece_'
```

샘플 출력:

```
MariaDB [main]> SHOW TABLES;
+-------------------------------------+
| Tables_in_main                      |
+-------------------------------------+
| ece_admin_passwords                 |
| ece_admin_system_messages           |
| ece_admin_user                      |
| ece_admin_user_session              |
| ece_adminnotification_inbox         |
| ece_amazon_customer                 |
| ece_authorization_rule              |
| ece_cache                           |
| ece_cache_tag                       |
| ece_captcha_log                     |
...
```

## `ELASTICSUITE_CONFIGURATION`

- **기본값**—_설정되지 않음_
- **버전**—Adobe Commerce 2.2.0 이상

배포 간에 사용자 지정된 [!DNL Elastic Suite] 서비스 설정을 유지하고 기본 [!DNL Elastic Suite] 구성의 &#39;system/default/smile_elasticsuite_core_base_settings&#39; 섹션에서 사용합니다. [!DNL Elastic Suite] 작성기 패키지가 설치되어 있으면 자동으로 구성됩니다.

```yaml
stage:
  deploy:
    ELASTICSUITE_CONFIGURATION:
      es_client:
        servers: 'remote-host:9200'
      indices_settings:
        number_of_shards: 1
        number_of_replicas: 0
```

>[!NOTE]
>
>세 개의 노드(또는 [크기 조정된 아키텍처](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/architecture/scaled-architecture#service-tier)에 세 개의 서비스 노드)가 있는 Pro Staging/Production 클러스터에서는 `indices_settings`을(를) 다음과 같이 설정해야 합니다.
>
>```yaml
>           indices_settings:
>               number_of_shards: 3
>               number_of_replicas: 2
>```

{{merge-options}}

다음 예제에서는 새 값을 기존 구성에 병합합니다.

```yaml
stage:
  deploy:
    ELASTICSUITE_CONFIGURATION:
      indices_settings:
        number_of_shards: 3
        number_of_replicas: 2
      _merge: true
```

**알려진 제한 사항**:

- 검색 엔진을 `elasticsuite`이(가) 아닌 다른 형식으로 변경하면 적절한 유효성 검사 오류가 발생하는 배포 오류가 발생합니다
- Elasticsearch 서비스를 제거하면 적절한 유효성 검사 오류가 발생하는 배포 오류가 발생합니다

>[!NOTE]
>
>Adobe Commerce에서 [!DNL Elastic Suite] 플러그인을 사용하거나 문제를 해결하는 방법에 대한 자세한 내용은 [[!DNL Elastic Suite] 설명서](https://github.com/Smile-SA/elasticsuite)를 참조하십시오.

## `ENABLE_GOOGLE_ANALYTICS`

- **기본값**—`false`
- **버전**—Adobe Commerce 2.1.4 이상

스테이징 및 통합 환경에 배포할 때 Google Analytics을 활성화하거나 비활성화합니다. 기본적으로 Google Analytics은 프로덕션 환경에만 true입니다. 스테이징 및 통합 환경에서 Google Analytics을 사용하려면 이 값을 `true`(으)로 설정하십시오.

- **`true`**—스테이징 및 통합 환경에서 Google Analytics을 사용하도록 설정합니다.
- **`false`**—스테이징 및 통합 환경에서 Google Analytics을 사용하지 않도록 설정합니다.

`ENABLE_GOOGLE_ANALYTICS` 환경 변수를 `.magento.env.yaml` 파일의 `deploy` 단계에 추가합니다.

```yaml
stage:
  deploy:
    ENABLE_GOOGLE_ANALYTICS: true
```

>[!NOTE]
>
>배포 프로세스를 통해 항상 프로덕션 환경에서 Google Analytics을 사용할 수 있습니다.

## `FORCE_UPDATE_URLS`

- **기본값**—`true`
- **버전**—Adobe Commerce 2.1.4 이상

Pro 또는 스타터 스테이징 및 프로덕션 환경에 배포할 때 이 변수는 데이터베이스의 Adobe Commerce 기본 URL을 [`MAGENTO_CLOUD_ROUTES`](variables-cloud.md) 변수에 지정된 프로젝트 URL로 바꿉니다. 스테이징 또는 프로덕션 환경에 배포할 때 무시되는 [UPDATE_URLS](#update_urls) 배포 변수의 기본 동작을 무시하려면 이 설정을 사용합니다.

```yaml
stage:
  deploy:
    FORCE_UPDATE_URLS: true
```

## `LOCK_PROVIDER`

- **기본값**—`file`
- **버전**—Adobe Commerce 2.2.5 이상

잠금 공급자는 중복 크론 작업 및 크론 그룹의 시작을 방지합니다. 프로덕션 환경에서 `file` 잠금 공급자를 사용합니다. Starter 환경 및 Pro 통합 환경에서는 [MAGENTO_CLOUD_LOCKS_DIR](variables-cloud.md) 변수를 사용하지 않으므로 `ece-tools`이(가) `db` 잠금 공급자를 자동으로 적용합니다.

```yaml
stage:
  deploy:
    LOCK_PROVIDER: "db"
```

_설치 가이드_&#x200B;에서 [잠금 구성](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/tutorials/lock-provider.html)을 참조하십시오.

## `MYSQL_USE_SLAVE_CONNECTION`

- **기본값**—`false`
- **버전**—Adobe Commerce 2.1.4 이상

>[!TIP]
>
>`MYSQL_USE_SLAVE_CONNECTION` 변수는 클라우드 인프라 스테이징 및 Production Pro 클러스터 환경의 Adobe Commerce에서만 지원되며 시작 프로젝트에서는 지원되지 않습니다.

Adobe Commerce은 여러 데이터베이스를 비동기식으로 읽을 수 있습니다. 데이터베이스에 대한 _읽기 전용_ 연결을 자동으로 사용하여 마스터가 아닌 노드에서 읽기 전용 트래픽을 받도록 `true`(으)로 설정하십시오. 이 연결은 한 노드만 읽기/쓰기 트래픽을 처리하기 때문에 로드 밸런싱을 통해 성능을 향상시킵니다. `env.php` 파일에서 기존 읽기 전용 연결 배열을 제거하려면 `false`(으)로 설정하십시오.

```yaml
stage:
  deploy:
    MYSQL_USE_SLAVE_CONNECTION: true
```

`MYSQL_USE_SLAVE_CONNECTION` 변수가 `true`(으)로 설정되면 Pro 스테이징 및 프로덕션 환경의 `env.php` 파일에서 `synchronous_replication` 매개 변수는 기본적으로 `true`(으)로 설정됩니다. `MYSQL_USE_SLAVE_CONNECTION`을(를) `false`(으)로 설정하면 `synchronous_replication` 매개 변수가 구성되지 않습니다.

## `QUEUE_CONFIGURATION`

- **기본값**—_설정되지 않음_
- **버전**—Adobe Commerce 2.1.4 이상

이 환경 변수를 사용하여 배포 간에 사용자 지정된 AMQP 서비스 설정을 유지합니다. 예를 들어, 클라우드 인프라에 의존하지 않고 기존 메시지 큐 서비스를 사용하여 자동으로 만들고자 하는 경우 `QUEUE_CONFIGURATION` 환경 변수를 사용하여 사이트에 연결하십시오.

```yaml
stage:
  deploy:
    QUEUE_CONFIGURATION:
      amqp:
        host: test.host
        port: 1234
      amqp2:
        host: test.host2
        port: 12345
      mq:
        host: mq.host
        port: 1234
```

{{merge-options}}

다음 예제에서는 새 값을 기존 구성에 병합합니다.

```yaml
stage:
  deploy:
    QUEUE_CONFIGURATION:
      _merge: true
      amqp:
        host: changed1.host
        port: 5672
      amqp2:
        host: changed2.host2
        port: 12345
      mq:
        host: changedmq.host
        port: 1234
```

## `REDIS_BACKEND`

- **기본값**—`Cm_Cache_Backend_Redis`
- **버전**—Adobe Commerce 2.3.0 이상

Redis 캐시에 대한 백엔드 모델 구성을 지정합니다.

Adobe Commerce 버전 2.3.0 이상에는 다음 백엔드 모델이 포함되어 있습니다.

- `Cm_Cache_Backend_Redis`
- `\Magento\Framework\Cache\Backend\Redis`
- `\Magento\Framework\Cache\Backend\RemoteSynchronizedCache`

`REDIS_BACKEND`을(를) 설정하는 방법의 예

```yaml
stage:
  deploy:
    REDIS_BACKEND: '\Magento\Framework\Cache\Backend\RemoteSynchronizedCache'
```

>[!NOTE]
>
>[L2 캐시](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/level-two-cache.html)를 사용하도록 Redis 백 엔드 모델로 `\Magento\Framework\Cache\Backend\RemoteSynchronizedCache`을(를) 지정하면 `ece-tools`에서 캐시 구성을 자동으로 생성합니다. _Adobe Commerce 구성 가이드_&#x200B;에서 예제 [구성 파일](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/level-two-cache.html#configuration-example)을(를) 참조하십시오. 생성된 캐시 구성을 재정의하려면 [CACHE_CONFIGURATION](#cache_configuration) 배포 변수를 사용하십시오.

## `REDIS_USE_SLAVE_CONNECTION`

- **기본값**—`false`
- **버전**—Adobe Commerce 2.1.16 이상

>[!WARNING]
>
>[크기 조정된 아키텍처](../architecture/scaled-architecture.md) 프로젝트에서 이 변수를 _사용하지 마십시오_. 이로 인해 Redis 연결 오류가 발생합니다. Redis 노예는 여전히 활동적이지만 Redis 읽기에는 사용되지 않습니다. Adobe 또는 Adobe Commerce 2.3.5 이상을 사용하고, 새로운 Redis 백엔드 구성을 구현하고, Redis에 대한 L2 캐싱을 구현하는 것이 좋습니다.

>[!TIP]
>
>`REDIS_USE_SLAVE_CONNECTION` 변수는 클라우드 인프라 스테이징 및 Production Pro 클러스터 환경의 Adobe Commerce에서만 지원되며 시작 프로젝트에서는 지원되지 않습니다.

Adobe Commerce은 여러 Redis 인스턴스를 비동기식으로 읽을 수 있습니다. Redis 인스턴스에 대한 _읽기 전용_ 연결을 자동으로 사용하여 마스터가 아닌 노드에서 읽기 전용 트래픽을 받도록 하려면 `true`(으)로 설정하십시오. 이 연결은 한 노드만 읽기/쓰기 트래픽을 처리하기 때문에 로드 밸런싱을 통해 성능을 향상시킵니다. `env.php` 파일에서 기존 읽기 전용 연결 배열을 제거하려면 `false`(으)로 설정하십시오.

```yaml
stage:
  deploy:
    REDIS_USE_SLAVE_CONNECTION: true
```

`.magento.app.yaml` 파일 및 `services.yaml` 파일에 Redis 서비스가 구성되어 있어야 합니다.

[ECE-Tools 버전 2002.0.18](../release-notes/cloud-release-archive.md#v2002018) 이상에서는 내결함성 설정을 더 많이 사용합니다. Adobe Commerce에서 Redis _슬레이브_ 인스턴스에서 데이터를 읽을 수 없는 경우 Redis _마스터_ 인스턴스에서 데이터를 읽습니다.

통합 환경에서 또는 [`CACHE_CONFIGURATION` 변수](#cache_configuration)을(를) 사용하는 경우 읽기 전용 연결을 사용할 수 없습니다.

## `RESOURCE_CONFIGURATION`

- **기본값**—설정되지 않음
- **버전**—Adobe Commerce 2.1.4 이상

리소스 이름을 데이터베이스 연결에 매핑합니다. 이 구성은 `env.php` 파일의 `resource` 섹션에 해당합니다.

{{merge-options}}

다음 예제에서는 새 값을 기존 구성에 병합합니다.

```yaml
stage:
  deploy:
    RESOURCE_CONFIGURATION:
      _merge: true
      default_setup:
        connection: default
```

## `SCD_COMPRESSION_LEVEL`

- **기본값**—`4`
- **버전**—Adobe Commerce 2.1.4 이상

정적 콘텐츠를 압축할 때 사용할 [gzip](https://www.gnu.org/software/gzip) 압축 수준(`0` ~ `9`)을 지정합니다. `0`은(는) 압축을 사용하지 않도록 설정합니다.

```yaml
stage:
  deploy:
    SCD_COMPRESSION_LEVEL: 5
```

## `SCD_COMPRESSION_TIMEOUT`

- **기본값**—`600`
- **버전**—Adobe Commerce 2.1.4 이상

정적 자산을 압축하는 데 걸리는 시간이 압축 시간 제한 을 초과하면 배포 프로세스를 중단합니다. 정적 콘텐츠 압축 명령의 최대 실행 시간(초)을 설정합니다.

```yaml
stage:
  deploy:
    SCD_COMPRESSION_TIMEOUT: 800
```

## `SCD_MATRIX`

- **기본값**—_설정되지 않음_
- **버전**—Adobe Commerce 2.1.4 이상

테마당 여러 로케일을 구성할 수 있습니다. 이 맞춤화는 불필요한 테마 파일의 수를 줄여 배포 프로세스를 가속화합니다. 예를 들어 _magento/backend_ 테마는 영어로, 사용자 지정 테마는 다른 언어로 배포할 수 있습니다.

다음 예제에서는 세 개의 로케일로 `Magento/backend` 테마를 배포합니다.

```yaml
stage:
  deploy:
    SCD_MATRIX:
      "magento/backend":
        language:
          - en_US
          - fr_FR
          - af_ZA
```

또한 테마를 _배포하지 않도록_ 선택할 수 있습니다.

```yaml
stage:
  deploy:
    SCD_MATRIX:
      "magento/backend": [ ]
```

## `SCD_MAX_EXECUTION_TIME`

- **기본값**—_설정되지 않음_
- **버전**—Adobe Commerce 2.2.0 이상

정적 콘텐츠 배포에 대한 최대 예상 실행 시간을 늘릴 수 있습니다.

기본적으로 Adobe Commerce은 최대 예상 실행을 900초로 설정하지만, 일부 시나리오에서는 클라우드 프로젝트에 대한 정적 콘텐츠 배포를 완료하는 데 더 많은 시간이 필요할 수 있습니다.

```yaml
stage:
  deploy:
    SCD_MAX_EXECUTION_TIME: 3600
```

{{scd-timing-warning}}

## `SCD_NO_PARENT`

- **기본값**—`false`
- **버전**—Adobe Commerce 2.4.2 이상

배포 단계에서 부모 테마에 대한 정적 콘텐츠 생성이 배포 단계 동안 발생하지 않도록 `SCD_NO_PARENT: true`을(를) 설정하십시오. 이 설정은 배포 시간을 최소화하고, 배포 중에 정적 콘텐츠 빌드가 실패할 경우 발생할 수 있는 사이트 가동 중단을 방지합니다. [정적 콘텐츠 배포](../deploy/static-content.md)를 참조하세요.

```yaml
stage:
  deploy:
    SCD_NO_PARENT: true
```

## `SCD_STRATEGY`

- **기본값**—`quick`
- **버전**—Adobe Commerce 2.2.0 이상

정적 콘텐츠에 대한 [배포 전략](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-strategy.html)을(를) 사용자 지정할 수 있습니다. [정적 보기 파일 배포](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment.html)를 참조하세요.

로케일이 두 개 이상인 경우 _only_ 옵션을 사용합니다.

- `standard` - 모든 패키지에 대한 모든 정적 보기 파일을 배포합니다.
- `quick`—(_default_)은(는) 배포 시간을 최소화합니다.
- `compact` - 서버의 디스크 공간을 절약합니다. Adobe Commerce 버전 2.2.4 및 이전 버전에서 이 설정은 값이 `1`인 `scd_threads`의 값을 재정의합니다.

```yaml
stage:
  deploy:
    SCD_STRATEGY: "compact"
```

## `SCD_THREADS`

- **기본값**—자동
- **버전**—Adobe Commerce 2.1.4 이상

정적 콘텐츠 배포를 위한 스레드 수를 설정합니다. 기본값은 감지된 CPU 스레드 수를 기준으로 설정되며 값 4를 초과하지 않습니다. 스레드 수를 늘리면 정적 콘텐츠 배포 속도가 빨라지고 스레드 수를 줄이면 속도가 느려집니다. 다음과 같이 스레드 값을 설정할 수 있습니다.

```yaml
stage:
  deploy:
    SCD_THREADS: 2
```

배포 시간을 더 줄이려면 `scd-dump` 명령과 함께 [구성 관리](../store/store-settings.md)를 사용하여 정적 배포를 빌드 단계로 이동하십시오.

## `SEARCH_CONFIGURATION`

- **기본값**—_설정되지 않음_
- **버전**—Adobe Commerce 2.1.4 이상

이 환경 변수를 사용하여 배포 간에 사용자 지정된 검색 서비스 설정을 유지합니다. For example:

Elasticsearch 구성:

```yaml
stage:
  deploy:
    SEARCH_CONFIGURATION:
      engine: elasticsearch
      elasticsearch_server_hostname: http://elasticsearch.internal
      elasticsearch_server_port: '9200'
      elasticsearch_index_prefix: magento2
      elasticsearch_server_timeout: '15'
```

OpenSearch 구성(Commerce 2.4.6 이상):

```yaml
stage:
  deploy:
    SEARCH_CONFIGURATION:
      engine: opensearch
      opensearch_server_hostname: 'http://opensearch.internal'
      opensearch_server_port: '9200'
      opensearch_index_prefix: 'magento2'
      opensearch_server_timeout: '15'
```

{{merge-options}}

다음 예제에서는 새 값을 기존 구성에 병합합니다.

```yaml
stage:
  deploy:
    SEARCH_CONFIGURATION:
      engine: elasticsearch
      elasticsearch_server_port: '9200'
      _merge: true
```

## `SESSION_CONFIGURATION`

- **기본값**—_설정되지 않음_
- **버전**—Adobe Commerce 2.1.4 이상

Redis 세션 저장소를 구성합니다. 세션 저장소 변수에 `save`, `redis`, `host`, `port` 및 `database` 옵션이 필요합니다. For example:

```yaml
stage:
  deploy:
    SESSION_CONFIGURATION:
      redis:
        bot_first_lifetime: 100
        bot_lifetime: 10001
        database: 0
        disable_locking: 1
        host: redis.internal
        max_concurrency: 10
        max_lifetime: 10001
        min_lifetime: 100
        port: 6379
      save: redis
```

{{merge-options}}

다음 예제에서는 새 값을 기존 구성에 병합합니다.

```yaml
stage:
  deploy:
    SESSION_CONFIGURATION:
      _merge: true
      redis:
        max_concurrency: 10
```

## `SKIP_SCD`

- **기본값**— _설정되지 않음_
- **버전**—Adobe Commerce 2.1.4 이상

배포 단계 동안 정적 콘텐츠 배포를 건너뛰려면 `true`(으)로 설정하십시오.

배포 단계에서 배포 단계 동안 정적 콘텐츠 빌드가 발생하지 않도록 `SKIP_SCD: true`을(를) 설정하십시오. 이 설정은 배포 시간을 최소화하고, 배포 중에 정적 콘텐츠 빌드가 실패할 경우 발생할 수 있는 사이트 가동 중단을 방지합니다. [정적 콘텐츠 배포](../deploy/static-content.md)를 참조하세요.

```yaml
stage:
  deploy:
    SKIP_SCD: true
```

## `UPDATE_URLS`

- **기본값**—`true`
- **버전**—Adobe Commerce 2.1.4 이상

배포 시 데이터베이스의 Adobe Commerce 기본 URL을 [`MAGENTO_CLOUD_ROUTES`](variables-cloud.md) 변수에 지정된 프로젝트 URL로 바꾸십시오. 이 구성은 로컬 환경에 대한 기본 URL이 설정되는 로컬 개발에 유용합니다. Cloud 환경에 배포하면 프로젝트 URL을 사용하여 상점 및 관리자에 액세스할 수 있도록 URL이 업데이트됩니다.

Pro 또는 스타터 스테이징 및 프로덕션 환경에 배포할 때 URL을 업데이트해야 하는 경우 [`FORCE_UPDATE_URLS`](#force_update_urls) 변수를 사용하십시오.

```yaml
stage:
  deploy:
    UPDATE_URLS: false
```

## `VERBOSE_COMMANDS`

- **기본값**—_설정되지 않음_
- **버전**—Adobe Commerce 2.1.4 이상

배포 단계에서 수행되는 `bin/magento` CLI 명령에 대해 [Symfony](https://symfony.com/doc/current/console/verbosity.html) 디버그 세부 정보 수준을 활성화하거나 비활성화합니다.

>[!NOTE]
>
>VERBOSE_COMMANDS 설정을 사용하여 성공 및 실패한 `bin/magento` CLI 명령에 대한 명령 출력의 세부 정보를 제어하려면 [MIN_LOGGING_LEVEL](variables-global.md#minlogginglevel) `debug`을(를) 설정해야 합니다.

로그에 제공되는 세부 사항 수준을 선택합니다.

- `-v`= 일반 출력
- `-vv`= 더 자세한 출력
- `-vvv` = 디버그에 적합한 세부 정보 출력

```yaml
stage:
  deploy:
    VERBOSE_COMMANDS: "-vv"
```
